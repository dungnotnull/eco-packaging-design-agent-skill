---
name: sub-scoring-engine
description: Scores packaging on circularity (9R), recyclability (design × infrastructure), and lifecycle impact, each against a named standard. Shared across science-industry cluster.
---

## Purpose
Produce a transparent, standards-grounded sustainability scorecard. Each dimension cites a specific standard or framework.

## Role
You are a sustainability assessment specialist. You evaluate packaging against multiple dimensions, cite specific standards, and report both overall and per-dimension scores with rationale.

## Data Structure
```typescript
interface Scorecard {
  overall_score: number;  // 0-100
  dimensions: ScoreDimension[];
  confidence: 'high' | 'medium' | 'low';
  confidence_rationale: string;
  recommendations: string[];
  critical_gaps: string[];
}

interface ScoreDimension {
  name: DimensionName;
  score: number;  // 0-100
  weight: number;  // 0-1, sum to 1
  standard_cited: StandardCitation;
  rationale: string;
  sub_scores?: SubScore[];
  flags: ScoreFlag[];
}

type DimensionName = 'circularity' | 'recyclability' | 'lifecycle_impact' | 'reuse_potential' | 'material_health';

interface SubScore {
  metric: string;
  value: number;
  target: number;
  achieved: boolean;
}

interface StandardCitation {
  framework: string;
  url: string;
  specific_clause?: string;
  year: number;
}

interface ScoreFlag {
  type: 'warning' | 'critical' | 'info';
  message: string;
  recommendation?: string;
}
```

## Inputs
- MaterialAnalysis (from material-lca-analyzer)
- EvaluationFramework (from framework-selector)
- ProductProfile (from requirements-gatherer)

## Procedure

### Dimension 1: Circularity Score (weight: 0.30)
**Standard**: Ellen MacArthur Foundation 9R Hierarchy

**Scoring** (0-100 points):
1. **R strategy placement** (40 points):
   - Refuse/Rethink: 40 pts
   - Reduce: 35 pts
   - Reuse/Repair/Refurbish: 30 pts
   - Remanufacture/Repurpose: 25 pts
   - Recycle: 20 pts
   - Recover: 10 pts
   - Dispose: 0 pts

2. **Recycled content** (30 points):
   - ≥50% recycled: 30 pts
   - ≥30% recycled: 25 pts
   - ≥15% recycled: 20 pts
   - ≥10% recycled: 15 pts
   - <10% or unknown: 0-10 pts

3. **Renewable/bio-based content** (15 points):
   - ≥80% bio-based: 15 pts
   - ≥50% bio-based: 10 pts
   - ≥20% bio-based: 5 pts
   - <20%: 0 pts

4. **Design for disassembly** (15 points):
   - Mono-material: 15 pts
   - 2 separable materials: 10 pts
   - 3+ materials but separable: 5 pts
   - Inseparable multilayer: 0 pts

**Standard Citation**:
```
framework: "Ellen MacArthur Foundation 9R Hierarchy"
url: "https://ellenmacarthurfoundation.org/topics/circular-economy-introduction/overview/concept"
year: 2021
```

### Dimension 2: Recyclability Score (weight: 0.30)
**Standard**: APR Design Guide / CEFLEX Design Guidelines × Local Infrastructure

**Scoring** (0-100 points):
1. **Design for recycling** (40 points):
   - Mono-material, compatible resin: 40 pts
   - Compatible with minimal additives: 35 pts
   - Compatible with known limitations: 25 pts
   - Problematic additives present: 0-10 pts
   - Non-recyclable multilayer: 0 pts

2. **Collection infrastructure** (30 points):
   - ≥75% collection coverage: 30 pts
   - ≥50% collection coverage: 20 pts
   - ≥25% collection coverage: 10 pts
   - <25%: 0 pts

3. **Sortation capability** (20 points):
   - Standard format, NIR sortable: 20 pts
   - Non-standard but sortable: 10 pts
   - Sortation challenges: 0-5 pts
   - Not sortable: 0 pts

4. **Recycling facility access** (10 points):
   - Local facilities accept: 10 pts
   - Regional facilities: 5 pts
   - No access: 0 pts

**Critical Rule**: BOTH design AND infrastructure must score ≥20/40 for overall "recyclable" rating.

**Standard Citations**:
```
framework: "APR Design Guide for Plastics Recyclability"
url: "https://plasticsrecycling.org/apr-design-guide"
year: 2024

framework: "CEFLEX Design Guidelines for Flexible Packaging"
url: "https://ceflex.eu/design-guidelines/"
year: 2023
```

### Dimension 3: Lifecycle Impact Score (weight: 0.30)
**Standard**: ISO 14040/44 LCA Framework + Industry Benchmarks

**Scoring** (0-100 points per impact category, then averaged):

**GWP (kg CO2e/unit)**:
- ≤0.02: 100 pts
- ≤0.05: 80 pts
- ≤0.10: 60 pts
- ≤0.20: 40 pts
- ≤0.50: 20 pts
- >0.50: 0 pts

**Energy (MJ/unit)**:
- ≤0.5: 100 pts
- ≤1.0: 80 pts
- ≤2.0: 60 pts
- ≤5.0: 40 pts
- ≤10: 20 pts
- >10: 0 pts

**Water (L/unit)**:
- ≤0.1: 100 pts
- ≤0.5: 80 pts
- ≤1.0: 60 pts
- ≤5.0: 40 pts
- ≤10: 20 pts
- >10: 0 pts

**Final impact score**: Average of GWP, energy, water scores.

**Standard Citation**:
```
framework: "ISO 14040/44 Environmental Management - Life Cycle Assessment"
url: "https://www.iso.org/standard/38498.html"
year: 2006 (current)
```

### Dimension 4: Reuse Potential Score (weight: 0.10)
**Standard**: EU PPWR Reuse Targets + C2C Reuse Criteria

**Scoring** (0-100 points):
1. **Durability** (30 points):
   - ≥10 reuses: 30 pts
   - ≥5 reuses: 20 pts
   - ≥2 reuses: 10 pts
   - Single-use: 0 pts

2. **Return system** (30 points):
   - Deposit-return in place: 30 pts
   - Return logistics planned: 15 pts
   - No return system: 0 pts

3. **Cleaning/sanitization** (20 points):
   - Compatible with industrial wash: 20 pts
   - Compatible with home wash: 10 pts
   - Not cleanable: 0 pts

4. **Standardization** (20 points):
   - Industry-standard format: 20 pts
   - Proprietary format: 5 pts
   - Custom format: 0 pts

**Standard Citations**:
```
framework: "EU PPWR Reuse Targets"
url: "https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:32024R1252"
year: 2024

framework: "Cradle to Cradle Certified Product Standard"
url: "https://www.c2ccertified.org/"
year: 2023
```

### Overall Score Calculation
```
overall = (circularity × 0.30) +
          (recyclability × 0.30) +
          (lifecycle_impact × 0.30) +
          (reuse_potential × 0.10)
```

### Confidence Assessment
**High confidence**:
- All impact factors from current database
- Infrastructure data verified
- Material specifications complete

**Medium confidence**:
- Impact factors from cached/older database
- Infrastructure estimated
- Some specifications assumed

**Low confidence**:
- Impact factors missing/guessed
- Infrastructure unknown
- Key specifications missing

## Outputs
Complete `Scorecard` object with:
- Overall score (0-100)
- Per-dimension scores with standard citations
- Sub-scores for transparency
- Flags for critical issues
- Confidence level and rationale
- Recommendations and critical gaps

## Quality Gates
- [ ] Each dimension cites a specific standard with URL
- [ ] Recyclability requires design AND infrastructure
- [ ] Confidence level explicitly stated
- [ ] Flags identify risks and opportunities
- [ ] Recommendations prioritize by impact

## Example Output
```typescript
{
  overall_score: 58,
  dimensions: [
    {
      name: "circularity",
      score: 55,
      weight: 0.30,
      standard_cited: {
        framework: "Ellen MacArthur Foundation 9R Hierarchy",
        url: "https://ellenmacarthurfoundation.org/topics/circular-economy-introduction/overview/concept",
        year: 2021,
      },
      rationale: "Packaging sits at 'Recycle' level on 9R (20/40 pts). 30% rPET content (25/30 pts). Not bio-based (0/15 pts). Mono-material PET (15/15 pts). Total: 60/100, adjusted for regional collection limitations.",
      sub_scores: [
        { metric: "9R_strategy_level", value: 2, target: 9, achieved: false, score: 20 },
        { metric: "recycled_content_pct", value: 30, target: 50, achieved: false, score: 25 },
        { metric: "bio_based_content_pct", value: 0, target: 50, achieved: false, score: 0 },
        { metric: "mono_material", value: true, target: true, achieved: true, score: 15 },
      ],
      flags: [
        { type: "info", message: "Improvement: Increase rPET to 50% for regulatory compliance (EU PPWR 2030)" },
      ],
    },
    {
      name: "recyclability",
      score: 78,
      weight: 0.30,
      standard_cited: {
        framework: "APR Design Guide for Plastics Recyclability",
        url: "https://plasticsrecycling.org/apr-design-guide",
        year: 2024,
      },
      rationale: "Mono-material PET passes APR DfR (40/40). EU collection 65% (20/30). NIR sortable (20/20). Facilities available (10/10). Total: 90/100, adjusted for US collection gaps.",
      sub_scores: [
        { metric: "apr_design_compliance", value: 100, target: 100, achieved: true, score: 40 },
        { metric: "collection_rate_pct", value: 65, target: 75, achieved: false, score: 20 },
        { metric: "sortation_capability", value: 100, target: 100, achieved: true, score: 20 },
        { metric: "facility_access", value: 100, target: 100, achieved: true, score: 10 },
      ],
      flags: [
        { type: "warning", message: "US collection rate significantly lower (30%), impacts recyclability in US market" },
      ],
    },
    {
      name: "lifecycle_impact",
      score: 52,
      weight: 0.30,
      standard_cited: {
        framework: "ISO 14040/44 Environmental Management - Life Cycle Assessment",
        url: "https://www.iso.org/standard/38498.html",
        year: 2006,
      },
      rationale: "Cradle-to-grave analysis. GWP: 0.069 kg CO2e/unit (60/100). Energy: 2.3 MJ/unit (50/100). Water: 1.8 L/unit (55/100). Average: 55/100. Production dominates impact.",
      sub_scores: [
        { metric: "gwp_kg_co2e_per_unit", value: 0.069, target: 0.05, achieved: false, score: 60 },
        { metric: "energy_mj_per_unit", value: 2.3, target: 2.0, achieved: false, score: 50 },
        { metric: "water_liters_per_unit", value: 1.8, target: 1.0, achieved: false, score: 55 },
      ],
      flags: [
        { type: "info", message: "Production stage is 60% of total GWP—lightweighting opportunities exist" },
      ],
    },
    {
      name: "reuse_potential",
      score: 15,
      weight: 0.10,
      standard_cited: {
        framework: "EU PPWR Reuse Targets",
        url: "https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:32024R1252",
        year: 2024,
      },
      rationale: "Single-use format (0/30). No return system (0/30). Not designed for cleaning (0/20). Standard format (15/20). Total: 15/100.",
      sub_scores: [
        { metric: "durability_reuses", value: 1, target: 5, achieved: false, score: 0 },
        { metric: "return_system", value: false, target: true, achieved: false, score: 0 },
        { metric: "cleaning_compatible", value: false, target: true, achieved: false, score: 0 },
        { metric: "standardization", value: true, target: true, achieved: true, score: 15 },
      ],
      flags: [
        { type: "info", message: "Current design is single-use. Reuse model would require significant redesign" },
      ],
    },
  ],
  confidence: "high",
  confidence_rationale: "Impact factors from ecoinvent v3.9 (2023). Infrastructure data from official sources. Material specifications complete.",
  recommendations: [
    "Increase rPET to 50% for EU PPWR compliance and +8 circularity points",
    "Lightweight to 25g unit for -15% GWP (+10 impact points)",
    "Explore PE mono-material alternative for improved recyclability in US market",
  ],
  critical_gaps: [
    "US collection infrastructure gap limits recyclability claim",
    "No reuse pathway considered—evaluate refill/reuse models",
  ],
}
```

## Anti-Patterns
- ❌ Failing to cite specific standards per dimension
- ❌ Treating recyclability as design-only
- ❌ Ignoring confidence level
- ❌ Missing flags for critical issues
- ❌ Opaque scoring without rationale

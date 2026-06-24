---
name: sub-improvement-roadmap
description: Produces a prioritized packaging-redesign roadmap balancing impact reduction, cost, and feasibility, with greenwashing checks. Shared across science-industry cluster.
---

## Purpose
Turn the scorecard into ranked, feasible redesign actions with explicit trade-offs and claim substantiation.

## Role
You are a packaging redesign strategist. You translate scores into actionable roadmap with clear priorities, cost implications, and feasibility assessments. You catch greenwashing before it reaches market.

## Data Structure
```typescript
interface Roadmap {
  options: RoadmapOption[];
  quick_wins: RoadmapOption[];
  strategic_bets: RoadmapOption[];
  not_recommended: NotRecommended[];
  claim_substantiation: ClaimCheck[];
  implementation_phases: ImplementationPhase[];
}

interface RoadmapOption {
  id: string;
  name: string;
  description: string;
  impact_reduction: {
    overall_score_delta: number;  // points improvement
    gwp_reduction_pct: number;
    circularity_improvement: number;
    recyclability_improvement: number;
  };
  cost_implication: {
    per_unit_delta: number;  // cents, positive = higher cost
    capex_estimate: string;  // description
    payback_months: number | null;
  };
  feasibility: {
    technical: 'high' | 'medium' | 'low';
    timeline_months: number;
    risks: string[];
    dependencies: string[];
  };
  regulatory_benefit: string[];
  trade_offs: string[];
  priority: number;  // 1 = highest
  category: 'material' | 'design' | 'reuse' | 'logistics' | 'labeling';
}

interface NotRecommended {
  option: string;
  reason: string;
  blocking_issue: string;
}

interface ClaimCheck {
  claim: string;
  passes: boolean;
  specific?: boolean;
  substantiated?: boolean;
  verifiable?: boolean;
  issues: string[];
  recommended_claim?: string;
}

interface ImplementationPhase {
  phase: number;
  duration_months: number;
  options: string[];  // option IDs
  prerequisites: string[];
  expected_impact: string;
}
```

## Inputs
- Scorecard (from scoring-engine)
- MaterialAnalysis (from material-lca-analyzer)
- ProductProfile (from requirements-gatherer)
- EvaluationFramework (from framework-selector)

## Procedure

### Step 1: Generate Redesign Options
For each improvement category:

**Material changes**:
- Increase recycled content
- Switch to mono-material
- Substitute lower-impact material
- Add bio-based content
- Eliminate problematic additives

**Design changes**:
- Lightweighting
- Remove non-functional layers
- Improve shape efficiency
- Design for disassembly

**Reuse models**:
- Refill system
- Deposit-return
- Durable packaging
- Bulk/refill format

**Logistics changes**:
- Concentrate format
- Remove secondary packaging
- Optimize transport shape

### Step 2: Score Each Option
For each option, calculate:

**Impact reduction** (baseline = current scorecard):
- Overall score improvement
- GWP reduction percentage
- Circularity improvement (points)
- Recyclability improvement (points)

**Cost implication**:
- Per-unit cost delta (cents)
- Capex estimate (tooling, equipment, line changes)
- Payback period (if savings exist)

**Feasibility**:
- Technical feasibility (high/medium/low)
- Timeline to implementation
- Key risks and dependencies

**Regulatory benefit**:
- PPWR compliance improvements
- EPR fee reductions
- Mandatory requirement fulfillment

### Step 3: Identify Trade-offs
Explicitly state what's lost:
- "Reduced GWP but lower recyclability in US market"
- "Higher cost but 40% GWP reduction"
- "Longer timeline but strategic reuse capability"

### Step 4: Priority Sequencing
Order by: (Impact × Urgency) ÷ (Cost × Risk)

**Quick wins** (implement now):
- Timeline < 6 months
- Low cost, low risk
- Immediate impact
- Line-compatible

**Strategic bets** (plan for):
- Timeline 6-18 months
- Medium-high cost, medium risk
- High long-term value
- Competitive advantage

**Not recommended**:
- Fails protection needs
- Excessive cost for minimal gain
- Technically infeasible
- Creates new problems

### Step 5: Greenwashing Check
For ANY environmental claim, apply the test:

**Required**: All FOUR must pass
1. **Specific**: Is the claim precise?
   - ❌ "Eco-friendly", "Green", "Natural"
   - ✅ "50% recycled PET", "30% lower carbon footprint"

2. **Substantiated**: Is evidence provided?
   - ❌ "Sustainable" without data
   - ✅ "30% recycled content per EN 15347"

3. **Verifiable**: Can a third party confirm?
   - ❌ "Better for the planet"
   - ✅ "GWP 0.05 kg CO2e/unit, cradle-to-grave, ISO 14040"

4. **No misleading elements**: No vague qualifiers, no hiding trade-offs
   - ❌ "Compostable" (when no facilities exist)
   - ✅ "Industrial compostable where facilities exist (EN 13432)"

**Fail ANY = claim not allowed**.

### Step 6: Implementation Phasing
Group into phases:
- Phase 1 (0-6 months): Quick wins, line-compatible changes
- Phase 2 (6-12 months): Medium-complexity material changes
- Phase 3 (12-24 months): Strategic bets, reuse systems

## Outputs
Complete `Roadmap` object with:
- Ranked options with impact/cost/feasibility
- Quick wins separated out
- Strategic bets identified
- Not-recommended options with reasons
- Claim substantiation checks
- Implementation phases

## Quality Gates
- [ ] Each option has impact/cost/feasibility quantified
- [ ] Trade-offs explicitly stated
- [ ] Claims pass greenwashing test (specific, substantiated, verifiable, no misleading)
- [ ] Phasing respects dependencies
- [ ] Protection threshold never violated

## Example Output
```typescript
{
  options: [
    {
      id: "OPT-001",
      name: "Increase rPET to 50%",
      description: "Increase recycled PET content from 30% to 50% using food-grade rPET",
      impact_reduction: {
        overall_score_delta: 8,
        gwp_reduction_pct: 15,
        circularity_improvement: 8,
        recyclability_improvement: 0,
      },
      cost_implication: {
        per_unit_delta: 2.5,
        capex_estimate: "None (line compatible)",
        payback_months: null,
      },
      feasibility: {
        technical: "high",
        timeline_months: 3,
        risks: ["rPET supply availability", "color consistency"],
        dependencies: ["Food-grade rPET supplier"],
      },
      regulatory_benefit: [
        "Meets EU PPWR 2030 30% recycled content target",
        "Reduces EPR fees in EU markets",
      ],
      trade_offs: [
        "Higher material cost (+2.5 cents/unit)",
        "Potential color variation vs. virgin PET",
      ],
      priority: 1,
      category: "material",
    },
    {
      id: "OPT-002",
      name: "Lightweight to 25g",
      description: "Reduce bottle weight from 30g to 25g through optimization",
      impact_reduction: {
        overall_score_delta: 10,
        gwp_reduction_pct: 17,
        circularity_improvement: 0,
        recyclability_improvement: 0,
      },
      cost_implication: {
        per_unit_delta: -1.0,
        capex_estimate: "Tooling modification: $50K",
        payback_months: 20,
      },
      feasibility: {
        technical: "high",
        timeline_months: 6,
        risks: ["Structural integrity testing required", "May affect consumer perception"],
        dependencies: ["Structural testing", "Consumer acceptance testing"],
      },
      regulatory_benefit: [
        "Reduces material use (EU PPWR minimization requirement)",
      ],
      trade_offs: [
        "Tooling investment required",
        "Consumer perception of 'lighter' packaging",
        "May affect bottle stiffness",
      ],
      priority: 2,
      category: "design",
    },
    {
      id: "OPT-003",
      name: "Mono-material PE film",
      description: "Replace PET/PE/Al multilayer with mono-material PE film",
      impact_reduction: {
        overall_score_delta: 12,
        gwp_reduction_pct: 22,
        circularity_improvement: 5,
        recyclability_improvement: 15,
      },
      cost_implication: {
        per_unit_delta: 3.0,
        capex_estimate: "Line modification: $200K for sealing equipment",
        payback_months: null,
      },
      feasibility: {
        technical: "medium",
        timeline_months: 12,
        risks: ["Oxygen barrier may be insufficient", "Line compatibility uncertain"],
        dependencies: ["Barrier testing", "Line modification"],
      },
      regulatory_benefit: [
        "Improves recyclability classification (CEFLEX compliant)",
        "Eliminates aluminum layer (EU recyclability requirement)",
      ],
      trade_offs: [
        "May require oxygen barrier additive (affects recyclability)",
        "Higher material cost",
        "Significant line modification required",
      ],
      priority: 3,
      category: "material",
    },
    {
      id: "OPT-004",
      name: "Refill system pilot",
      description: "Launch refill pouch + durable bottle system in 2 test markets",
      impact_reduction: {
        overall_score_delta: 25,
        gwp_reduction_pct: 40,
        circularity_improvement: 20,
        recyclability_improvement: 10,
      },
      cost_implication: {
        per_unit_delta: 5.0,
        capex_estimate: "System design + pilot: $500K",
        payback_months: null,
      },
      feasibility: {
        technical: "low",
        timeline_months: 24,
        risks: ["Consumer adoption uncertain", "Logistics complexity", "Retailer acceptance"],
        dependencies: ["Refill packaging design", "Return logistics", "Retail partnerships"],
      },
      regulatory_benefit: [
        "Advances EU PPWR reuse targets (10% by 2030)",
        "Potential EPR exemption for reuse formats",
      ],
      trade_offs: [
        "High capex and complexity",
        "Consumer behavior change required",
        "Long timeline to ROI",
      ],
      priority: 4,
      category: "reuse",
    },
  ],
  quick_wins: [
    {
      id: "OPT-001",
      name: "Increase rPET to 50%",
      description: "Increase recycled PET content from 30% to 50% using food-grade rPET",
      impact_reduction: { overall_score_delta: 8, gwp_reduction_pct: 15, circularity_improvement: 8, recyclability_improvement: 0 },
      cost_implication: { per_unit_delta: 2.5, capex_estimate: "None (line compatible)", payback_months: null },
      feasibility: { technical: "high", timeline_months: 3, risks: ["rPET supply availability", "color consistency"], dependencies: ["Food-grade rPET supplier"] },
      regulatory_benefit: ["Meets EU PPWR 2030 30% recycled content target", "Reduces EPR fees in EU markets"],
      trade_offs: ["Higher material cost (+2.5 cents/unit)", "Potential color variation vs. virgin PET"],
      priority: 1,
      category: "material",
    },
  ],
  strategic_bets: [
    {
      id: "OPT-003",
      name: "Mono-material PE film",
      description: "Replace PET/PE/Al multilayer with mono-material PE film",
      impact_reduction: { overall_score_delta: 12, gwp_reduction_pct: 22, circularity_improvement: 5, recyclability_improvement: 15 },
      cost_implication: { per_unit_delta: 3.0, capex_estimate: "Line modification: $200K for sealing equipment", payback_months: null },
      feasibility: { technical: "medium", timeline_months: 12, risks: ["Oxygen barrier may be insufficient", "Line compatibility uncertain"], dependencies: ["Barrier testing", "Line modification"] },
      regulatory_benefit: ["Improves recyclability classification (CEFLEX compliant)", "Eliminates aluminum layer (EU recyclability requirement)"],
      trade_offs: ["May require oxygen barrier additive (affects recyclability)", "Higher material cost", "Significant line modification required"],
      priority: 3,
      category: "material",
    },
    {
      id: "OPT-004",
      name: "Refill system pilot",
      description: "Launch refill pouch + durable bottle system in 2 test markets",
      impact_reduction: { overall_score_delta: 25, gwp_reduction_pct: 40, circularity_improvement: 20, recyclability_improvement: 10 },
      cost_implication: { per_unit_delta: 5.0, capex_estimate: "System design + pilot: $500K", payback_months: null },
      feasibility: { technical: "low", timeline_months: 24, risks: ["Consumer adoption uncertain", "Logistics complexity", "Retailer acceptance"], dependencies: ["Refill packaging design", "Return logistics", "Retail partnerships"] },
      regulatory_benefit: ["Advances EU PPWR reuse targets (10% by 2030)", "Potential EPR exemption for reuse formats"],
      trade_offs: ["High capex and complexity", "Consumer behavior change required", "Long timeline to ROI"],
      priority: 4,
      category: "reuse",
    },
  ],
  not_recommended: [
    {
      option: "Switch to glass bottles",
      reason: "Higher GWP (+150%), higher transport cost, fragile",
      blocking_issue: "Fails sustainability goals and consumer convenience",
    },
    {
      option: "PLA compostable film",
      reason: "No industrial composting infrastructure in target markets",
      blocking_issue: "Would landfill in practice; greenwashing risk",
    },
  ],
  claim_substantiation: [
    {
      claim: "Made with 50% recycled plastic",
      passes: true,
      specific: true,
      substantiated: true,
      verifiable: true,
      issues: [],
      recommended_claim: "Made with 50% recycled PET (EN 15347 verified)",
    },
    {
      claim: "Eco-friendly packaging",
      passes: false,
      issues: ["Not specific", "Not substantiated", "Not verifiable"],
      recommended_claim: "50% recycled content, recyclable where facilities exist",
    },
    {
      claim: "100% recyclable",
      passes: false,
      issues: ["US market lacks collection infrastructure for film", "Not verifiable in all markets"],
      recommended_claim: "Recyclable in EU where collection exists (65% coverage)",
    },
    {
      claim: "30% lower carbon footprint",
      passes: true,
      specific: true,
      substantiated: true,
      verifiable: true,
      issues: [],
      recommended_claim: "30% lower carbon footprint vs. previous packaging (cradle-to-grave, ISO 14040)",
    },
  ],
  implementation_phases: [
    {
      phase: 1,
      duration_months: 6,
      options: ["OPT-001", "OPT-002"],
      prerequisites: ["rPET supplier contract", "Structural testing complete"],
      expected_impact: "Overall score +18, GWP -30%",
    },
    {
      phase: 2,
      duration_months: 12,
      options: ["OPT-003"],
      prerequisites: ["Barrier testing passed", "Line modification approved"],
      expected_impact: "Overall score +12, recyclability +15",
    },
    {
      phase: 3,
      duration_months: 24,
      options: ["OPT-004"],
      prerequisites: ["Pilot markets selected", "Retail partnerships secured"],
      expected_impact: "Overall score +25, circularity +20",
    },
  ],
}
```

## Greenwashing Decision Tree
```
Proposed claim → Is it specific?
├─ No (vague like "eco-friendly") → FAIL → Use specific metrics
└─ Yes → Is it substantiated with data?
   ├─ No → FAIL → Add data/verification
   └─ Yes → Is it verifiable by third party?
      ├─ No → FAIL → Add standard/methodology
      └─ Yes → Any misleading elements?
         ├─ Yes (hides trade-offs) → FAIL → Reveal trade-offs
         └─ No → PASS
```

## Anti-Patterns
- ❌ Recommending options that fail protection needs
- ❌ Missing cost implications
- ❌ Not quantifying impact reduction
- ❌ Hiding trade-offs
- ❌ Allowing vague environmental claims
- ❌ Failing to check infrastructure reality

---
name: sub-evaluation-framework-selector
description: Selects applicable sustainability standards (LCA scope, C2C, DfR guideline, compostability standard, regulation) for the packaging material and market with justification.
---

## Purpose
Apply the right standards for the material/market rather than generic claims. Ensures evaluation is grounded in relevant frameworks.

## Role
You are a standards specialist. You select the appropriate evaluation frameworks based on material type, packaging format, geographic market, and sustainability goals. You justify each selection.

## Data Structure
```typescript
interface EvaluationFramework {
  lca_scope: {
    type: 'cradle-to-gate' | 'cradle-to-grave' | 'gate-to-gate';
    rationale: string;
    system_boundary: string;
    functional_unit: string;
    impact_categories: ImpactCategory[];
  };
  recyclability_guideline: {
    primary: 'APR' | 'CEFLEX' | 'RecyClass' | 'How2Recycle' | 'OPRL';
    secondary?: string;
    rationale: string;
    applicability: string[];
  };
  circularity_framework: {
    framework: '9R' | 'C2C' | 'EU_Circular_Economy_Action_Plan';
    rationale: string;
  };
  compostability_standard?: {
    standard: 'EN_13432' | 'ASTM_D6400' | 'EN_17033' | 'ISO_17088' | 'home_compost_cert';
    industrial: boolean;
    home_compost: boolean;
    rationale: string;
  };
  regulations: {
    framework: string[];
    recycled_content_target?: {
      percentage: number;
      deadline_year: number;
    };
    reuse_target?: {
      percentage: number;
      deadline_year: number;
    };
    recyclability_requirement?: string;
  };
  data_sources: DataSource[];
}

interface ImpactCategory {
  category: 'GWP100' | 'GWP20' | 'energy' | 'water' | 'eutrophication' | 'acidification' | 'photochemical_ozone';
  method: string;  // e.g., IPCC AR5, CML, ReCiPe
  unit: string;
}

interface DataSource {
  name: string;
  url: string;
  vintage: number;
  type: 'database' | 'guideline' | 'regulation' | 'standard';
}
```

## Inputs
- ProductProfile from requirements-gatherer
- Target sustainability goals (impact reduction, circularity, regulatory compliance)

## Procedure

### Step 1: Select LCA Scope
Choose based on evaluation purpose:
1. **Material comparison**: Use cradle-to-gate (production to packaging)
   - Compares materials fairly
   - Excludes use-phase (usually minimal for packaging)
2. **End-of-life focus**: Use cradle-to-grave
   - Includes disposal, recycling, composting
   - Required for PPWR compliance
3. **Transport optimization**: Use gate-to-gate
   - Focus on logistics and distribution

**Functional unit**: typically "per 1000 packages" or "per kg product packaged"

**Impact categories** (minimum):
- GWP100 (global warming potential, 100-year horizon)
- Cumulative energy demand
- Water consumption

### Step 2: Select Design-for-Recycling Guideline
Match to packaging type and region:

| Packaging Type | Region | Guideline | Rationale |
|---------------|--------|-----------|-----------|
| Rigid plastic (bottles, tubs) | US | APR Design Guide | Comprehensive US DfR guidance |
| Flexible films | EU | CEFLEX | Leading flexible-packaging guidance |
| Any plastic | EU | RecyClass | EU recyclability certification |
| Consumer packaging | UK | OPRL | UK on-pack labeling |
| Consumer packaging | US | How2Recycle | US consumer labeling |

**Secondary guideline** may apply (e.g., APR + RecyClass for global product).

### Step 3: Select Circularity Framework
Default to **9R hierarchy** (Ellen MacArthur Foundation):
- Strategy ordering: Refuse → Rethink → Reduce → Reuse → Repair → Refurbish → Remanufacture → Repurpose → Recycle → Recover

Alternative:
- **C2C Certified** if targeting Cradle to Cradle certification
- **EU Circular Economy Action Plan** if regulatory focus

### Step 4: Select Compostability Standard (if applicable)
Only if bioplastic or fiber-based packaging:

| Standard | Region | Type | When to Use |
|----------|--------|------|-------------|
| EN 13432 | EU | Industrial | EU industrial composting claims |
| ASTM D6400 | US | Industrial | US industrial composting claims |
| EN 17033 | EU | Home | EU home composting claims |
| ISO 17088 | International | Industrial | Global standard |

**Key**: Compostability ≠ biodegradable. Require specific standard + facility access.

### Step 5: Identify Applicable Regulations
By region:

**EU**:
- PPWR (Packaging & Packaging Waste Regulation)
  - Recycled content targets: 30% PET bottles (2030), 10% all plastics (2030)
  - Reuse targets: 10% beverage packaging (2030)
  - Minimum recyclability criteria
- EPR (Extended Producer Responsibility) fees
- National packaging laws

**US**:
- State EPR laws (CA, OR, ME, etc.)
- Truth in Labeling Act (FTC green guides)
- State-specific recycled content mandates

### Step 6: Compile Data Sources
List authoritative sources for impact factors, design guidelines, and regulations:
- Databases: ecoinvent, GaBi, IDEMAT
- Guidelines: APR, CEFLEX, RecyClass
- Regulations: EU PPWR, state EPR laws

## Outputs
Complete `EvaluationFramework` object with:
- LCA scope with functional unit and impact categories
- DfR guideline(s) with rationale
- Circularity framework
- Compostability standard (if relevant)
- Regulations with targets
- Data sources with URLs and vintages

## Quality Gates
- [ ] LCA scope justified for evaluation purpose
- [ ] DfR guideline matches packaging type and region
- [ ] Circularity framework specified
- [ ] Compostability only claimed if standard + facility access
- [ ] Regulations identified with specific targets
- [ ] All sources cited with URLs and vintages

## Example Output
```typescript
{
  lca_scope: {
    type: "cradle-to-grave",
    rationale: "End-of-life critical for recyclability assessment; PPWR requires full lifecycle view",
    system_boundary: "Raw material extraction through end-of-life (recycling, composting, disposal)",
    functional_unit: "per 1000 units of packaging",
    impact_categories: [
      { category: "GWP100", method: "IPCC AR5", unit: "kg CO2e" },
      { category: "energy", method: "Cumulative Energy Demand", unit: "MJ" },
      { category: "water", method: "Water Scarcity", unit: "L" },
    ],
  },
  recyclability_guideline: {
    primary: "CEFLEX",
    secondary: "RecyClass",
    rationale: "Flexible film packaging in EU market; CEFLEX provides comprehensive DfR guidance for flexible packaging",
    applicability: ["PE_film", "PP_film", "multilayer_film"],
  },
  circularity_framework: {
    framework: "9R",
    rationale: "9R hierarchy provides clear strategy prioritization for circular-economy transition",
  },
  compostability_standard: undefined,
  regulations: {
    framework: ["PPWR", "EPR"],
    recycled_content_target: { percentage: 10, deadline_year: 2030 },
    reuse_target: { percentage: 10, deadline_year: 2030 },
    recyclability_requirement: "All packaging must be recyclable in practice by 2030",
  },
  data_sources: [
    { name: "CEFLEX Design Guidelines", url: "https://ceflex.eu/design-guidelines/", vintage: 2023, type: "guideline" },
    { name: "PPWR", url: "https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:32024R1252", vintage: 2024, type: "regulation" },
    { name: "ecoinvent", url: "https://ecoinvent.org/", vintage: 2023, type: "database" },
  ],
}
```

## Framework Selection Decision Tree
```
Is packaging flexible film?
├─ Yes, EU → CEFLEX + RecyClass
├─ Yes, US → APR (Flexible Guidelines) + How2Recycle
└─ Rigid plastic
   ├─ US → APR Design Guide + How2Recycle
   └─ EU → RecyClass + OPRL (if UK)

Compostability claim?
├─ Yes, EU → EN 13432 (industrial) or EN 17033 (home)
├─ Yes, US → ASTM D6400 (industrial) + home-compost cert
└─ No → No compostability standard

LCA scope?
├─ Material comparison → cradle-to-gate
├─ End-of-life focus → cradle-to-grave
└─ Logistics focus → gate-to-gate
```

## Anti-Patterns
- ❌ Using generic DfR without region/market match
- ❌ Assuming compostability = biodegradable
- ❌ Ignoring local regulations
- ❌ Selecting LCA scope without justification
- ❌ Citing outdated data sources without vintage acknowledgment

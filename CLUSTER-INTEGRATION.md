# CLUSTER-INTEGRATION — Science-Industry Cluster Sharing

## Eco-Packaging Design (Idea 214)

## Cluster Context
This skill belongs to the `science-industry` cluster, sharing frameworks and methodologies with related sustainability assessment skills.

## Shared Components

### 1. Evaluation Framework Selector
**Shared with**: Ideas 179, 217 (science-industry cluster)

**Purpose**: Standardized framework selection for sustainability assessment

**Shared logic**:
- LCA scope selection (cradle-to-gate vs. cradle-to-grave)
- Regulatory identification (EU PPWR, US EPR)
- Standard citation methodology

**Cross-skill usage**:
- Idea 179: Material lifecycle assessment
- Idea 217: Circularity scoring framework
- Idea 214: Packaging-specific regulations and DfR guidelines

**API contract**:
```typescript
interface SharedFrameworkSelector {
  selectLCAScope(purpose: string): LCAScope;
  identifyRegulations(region: string): Regulation[];
  citeStandard(standard: string): StandardCitation;
}
```

### 2. Scoring Engine (Circularity & Recyclability)
**Shared with**: Ideas 179, 217

**Purpose**: Standardized scoring against circular-economy and recyclability metrics

**Shared logic**:
- 9R hierarchy mapping (EMF framework)
- Recyclability = Design × Infrastructure calculation
- Multi-dimensional scoring with weights

**Cross-skill usage**:
- Idea 179: Product circularity assessment
- Idea 217: Industrial circularity metrics
- Idea 214: Packaging-specific DfR (APR, CEFLEX)

**API contract**:
```typescript
interface SharedScoringEngine {
  scoreCircularity(design: Design): CircularScore;
  scoreRecyclability(design: Design, infrastructure: Infrastructure): RecyclabilityScore;
  calculateOverall(dimensions: ScoreDimension[]): OverallScore;
}
```

### 3. Impact Factor Management
**Shared with**: Ideas 179, 217

**Purpose**: Centralized impact factor database with versioning

**Shared logic**:
- Impact factor caching (ecoinvent, EPA WARM)
- Vintage tracking and staleness flags
- Fallback to cached data when fetch fails

**Cross-skill usage**:
- Idea 179: Material impact comparison
- Idea 217: Process impact assessment
- Idea 214: Packaging material impact

**API contract**:
```typescript
interface SharedImpactFactors {
  getFactor(material: string, category: string): ImpactFactor | null;
  citeSource(database: string, year: number): SourceCitation;
  flagStale(factor: ImpactFactor): StalenessFlag;
}
```

### 4. Greenwashing Detection
**Shared with**: Ideas 179, 217

**Purpose**: Standardized environmental claim validation

**Shared logic**:
- Four-part test: Specific, Substantiated, Verifiable, No Misleading Elements
- Claim rejection with specific recommendations
- Trade-off disclosure requirement

**Cross-skill usage**:
- Idea 179: Product environmental claims
- Idea 217: Industrial sustainability claims
- Idea 214: Packaging recyclability/compostability claims

**API contract**:
```typescript
interface SharedGreenwashingCheck {
  validateClaim(claim: string): ClaimValidation;
  recommendAlternative(rejectedClaim: string): RecommendedClaim;
  flagTradeOffs(claim: string, tradeOffs: string[]): TradeOffFlag;
}
```

## Skill-Specific Extensions

### Idea 214 (Packaging) Unique Components
These components are NOT shared—specific to packaging domain:

**Packaging-Specific**:
- DfR guidelines: APR (US rigid), CEFLEX (EU flexible), RecyClass
- Protection needs assessment: fragility, barriers, shelf-life
- Packaging layers: primary, secondary, tertiary
- On-pack labeling: How2Recycle, OPRL

**Not shared with 179/217**:
- Protection threshold enforcement
- Multi-layer packaging analysis
- Compostability standards (EN 13432, ASTM D6400)
- Packaging regulations (PPWR recycled content, reuse targets)

### Idea 179 (Material LCA) Unique Components
**Material-specific**:
- Material property databases
- Mechanical performance metrics
- Supply chain impact mapping

### Idea 217 (Industrial Circularity) Unique Components
**Industrial-specific**:
- Process flow circularity
- Industrial symbiosis metrics
- Factory-level material loops

## Integration Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    SCIENCE-INDUSTRY CLUSTER                  │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  ┌─────────────────────────────────────────────────────┐   │
│  │              SHARED FRAMEWORK LAYER                 │   │
│  │  ┌─────────────────┐  ┌─────────────────┐          │   │
│  │  │ Framework       │  │ Scoring         │          │   │
│  │  │ Selector        │  │ Engine          │          │   │
│  │  │ (LCA scope,     │  │ (9R, recyclable,│          │   │
│  │  │  regs)          │  │  impact)        │          │   │
│  │  └─────────────────┘  └─────────────────┘          │   │
│  │  ┌─────────────────┐  ┌─────────────────┐          │   │
│  │  │ Impact Factor   │  │ Greenwashing     │          │   │
│  │  │ Manager         │  │ Detection       │          │   │
│  │  │ (cache, vintage)│  │ (4-part test)   │          │   │
│  │  └─────────────────┘  └─────────────────┘          │   │
│  └─────────────────────────────────────────────────────┘   │
│                          │                                  │
│           ┌──────────────┼──────────────┐                 │
│           │              │              │                 │
│  ┌────────▼────────┐ ┌──▼──────────┐ ┌─▼────────────┐  │
│  │ Idea 214        │ │ Idea 179    │ │ Idea 217      │  │
│  │ Packaging       │ │ Material    │ │ Industrial    │  │
│  │ Design          │ │ LCA         │ │ Circularity   │  │
│  ├─────────────────┤ ├─────────────┤ ├──────────────┤  │
│  │ DfR guidelines  │ │ Material    │ │ Process       │  │
│  │ (APR, CEFLEX)   │ │ properties  │ │ flows         │  │
│  ├─────────────────┤ ├─────────────┤ ├──────────────┤  │
│  │ Protection      │ │ Supply      │ │ Symbiosis     │  │
│  │ threshold       │ │ chain       │ │ metrics       │  │
│  ├─────────────────┤ ├─────────────┤ ├──────────────┤  │
│  │ Packaging       │ │ Mechanical  │ │ Factory       │  │
│  │ layers          │ │ performance│ │ loops         │  │
│  └─────────────────┘ └─────────────┘ └──────────────┘  │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

## Cross-Reference Links

### From Idea 214 to Cluster Skills
- **Framework Selector**: See `skills/sub-evaluation-framework-selector.md` — shared LCA scope and regulation selection
- **Scoring Engine**: See `skills/sub-scoring-engine.md` — shared 9R and recyclability scoring
- **Greenwashing Check**: See `skills/sub-improvement-roadmap.md` — shared 4-part claim validation

### From Cluster Skills to Idea 214
- **Idea 179 (Material LCA)**: Uses 214's impact factor cache for packaging materials
- **Idea 217 (Industrial Circularity)**: Uses 214's recyclability scoring for packaging loops

## Version Compatibility Matrix
| Component | 214 Version | 179 Compatible | 217 Compatible |
|-----------|-------------|----------------|----------------|
| Framework Selector | 1.0 | Yes (1.0+) | Yes (1.0+) |
| Scoring Engine | 1.0 | Yes (1.0+) | Yes (1.0+) |
| Impact Factors | 2023v | Yes (2023v) | Yes (2023v) |
| Greenwashing Check | 1.0 | Yes (1.0+) | Yes (1.0+) |

## Maintenance Protocol
When updating shared components:
1. Update version in all three skills
2. Run test suites for all affected skills
3. Update compatibility matrix
4. Document breaking changes in each skill's CLAUDE.md

## Contact & Coordination
- **Idea 214 (Packaging)**: `D:/skills/eco-packaging-design/`
- **Idea 179 (Material LCA)**: `<path when available>`
- **Idea 217 (Industrial Circularity)**: `<path when available>`

## Shared Knowledge Sources
All three skills draw from:
- Ellen MacArthur Foundation (circular economy, 9R)
- ISO 14040/44 (LCA methodology)
- ecoinvent database (impact factors)
- EU PPWR (regulatory framework)

Each skill adds domain-specific sources (see respective SECOND-KNOWLEDGE-BRAIN.md files).

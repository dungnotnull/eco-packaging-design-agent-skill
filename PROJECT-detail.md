# PROJECT-detail.md — Eco-friendly Packaging Design

## Executive Summary
A harness that evaluates and redesigns packaging for circular-economy performance. It screens materials, estimates lifecycle impact (ISO 14040/44 framing), scores circularity and recyclability against Cradle to Cradle, Design-for-Recycling (APR/CEFLEX), and regulatory standards (EU PPWR), and emits a prioritized redesign roadmap balancing protection, cost, and impact.

## Problem Statement
Packaging decisions involve hard trade-offs: a lighter package may not be recyclable; a compostable one may need industrial facilities that don't exist locally; "recyclable" claims risk greenwashing. Teams need structured, standards-grounded evaluation.

## Target Users & Use Cases
- **Brand/packaging engineer** — "Score our current carton and suggest greener options." → circularity score + roadmap.
- **Sustainability lead** — "Is this 'recyclable' claim defensible?" → recyclability check vs. DfR + local infrastructure.
- **Startup (CPG)** — "Pick a package material for EU + US." → material screen with regulatory fit.
- **Procurement** — "Compare mono-material vs. multilayer film impact." → LCA-style comparison.
- **Designer** — "Reduce material without losing protection." → lightweighting roadmap.

## Harness Architecture
```
/eco-packaging-design
  Stage 1 Intake     → sub-requirements-gatherer        → product/package profile
  Stage 2 Framework  → sub-evaluation-framework-selector→ standards + LCA scope
  Stage 3 Material   → sub-material-lca-analyzer        → material screen + impact
  Stage 4 Scoring    → sub-scoring-engine               → circularity/recyclability score
  Stage 5 Roadmap    → sub-improvement-roadmap          → redesign options
```

## Full Sub-Skill Catalog
| Sub-skill | Purpose | Inputs | Outputs | Tools | Quality gate |
|-----------|---------|--------|---------|-------|--------------|
| requirements-gatherer | Profile | user | product/package | Read | Function + market + constraints captured |
| framework-selector | Pick standards | profile | standards set | WebSearch | Standards matched to material/market |
| material-lca-analyzer | Material + LCA | profile, factors | impact estimate | WebFetch | Scope + impact factors cited |
| scoring-engine | Score | analysis | circularity score | — | Each dimension cites a standard |
| improvement-roadmap | Redesign | score | options | — | Cost/impact/feasibility per option |

## Skill File Format Specification
Standard Claude skill format. See `skills/main.md`.

## E2E Execution Flow
Intake → framework → material/LCA → score → roadmap. Fallback to cached impact factors if web down (flag). Error: function requirements unknown → ask (don't recommend under-protective packaging).

## SECOND-KNOWLEDGE-BRAIN Integration
`knowledge_updater.py` crawls EMF/ISO/PPWR/APR/CEFLEX; dated append.

## Quality Gates
- Each score dimension cites a named standard.
- LCA scope (cradle-to-gate/grave) and impact factors stated.
- Recyclability assessed against DfR AND local collection reality.
- Greenwashing risk flagged on any environmental claim.
- Roadmap items carry cost/impact/feasibility.

## Test Scenarios
See `tests/test-scenarios.md` (6 scenarios).

## Key Design Decisions
1. Recyclability = design + real-world infrastructure, not just material type.
2. LCA framing prevents single-metric (e.g., weight-only) optimization.
3. Compostability claims require facility availability + standard (EN 13432) check.
4. Protection/shelf-life never sacrificed below function threshold.
5. Anti-greenwashing check on all claims.

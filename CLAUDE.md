# CLAUDE.md — Eco-friendly Packaging Design (Idea 214)

**Skill name:** `eco-packaging-design`
**Tagline:** Designs and scores packaging for circular-economy fit, recyclability, and low lifecycle impact, grounded in LCA, Cradle to Cradle, and Design-for-Recycling standards.
**Cluster:** `science-industry`
**Source idea:** 214
**Current phase:** Full deliverable set scaffolded

## Problem This Skill Solves
Brands face tightening packaging regulation (EU PPWR), plastic-waste scrutiny, and recyclability demands, but lack tools to evaluate trade-offs between protection, cost, and environmental impact. This skill takes a product/package spec, screens material choices, estimates lifecycle impact, scores circularity and recyclability against named standards, and outputs a redesign roadmap.

## Harness Flow Summary
1. **Intake** → `sub-requirements-gatherer` — product, current packaging, market, constraints.
2. **Framework selection** → `sub-evaluation-framework-selector` — pick standards (C2C, LCA scope, DfR guideline) for the material/market.
3. **Material & LCA analysis** → `sub-material-lca-analyzer` — material screen + lifecycle-impact estimate.
4. **Scoring** → `sub-scoring-engine` — circularity + recyclability + impact score.
5. **Roadmap** → `sub-improvement-roadmap` — redesign options by impact/cost/feasibility.

## Sub-skills
- `sub-requirements-gatherer.md`
- `sub-evaluation-framework-selector.md`
- `sub-material-lca-analyzer.md`
- `sub-scoring-engine.md`
- `sub-improvement-roadmap.md`

## Tools Required
WebSearch, WebFetch, Read, Write, Bash.

## Knowledge Sources
Ellen MacArthur Foundation (circular economy); ISO 14040/44 (LCA); Cradle to Cradle Certified; EU PPWR; APR Design Guide; CEFLEX; EN 13432 / ASTM D6400 (compostability); How2Recycle/OPRL.

## Supporting Tools
- `tools/knowledge_updater.py` — crawls circular-economy + packaging-regulation + LCA sources.

## Active Development Tasks
- [x] Scaffold deliverables
- [ ] Add material impact-factor table
- [ ] Track PPWR rule changes

## Reference Docs
`PROJECT-detail.md`, `PROJECT-DEVELOPMENT-PHASE-TRACKING.md`, `SECOND-KNOWLEDGE-BRAIN.md`.

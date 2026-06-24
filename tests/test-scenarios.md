# Test Scenarios — Eco-friendly Packaging Design (Idea 214)

## Overview
These scenarios validate the complete harness flow from intake through roadmap. Each scenario tests specific quality gates and edge cases.

## Scenario 1 — Multilayer Film Redesign
**Objective**: Validate recyclability assessment and roadmap generation for multilayer packaging

**Input**:
```
Product: Savory snack food
Current Packaging: PET/PE/Al multilayer pouch, 180×120×40mm, 8.5g total weight
Protection Needs: Moisture barrier required, oxygen barrier required, 180-day shelf life
Target Markets: EU (DE, FR, IT, ES, NL), US
Constraints: Cost ceiling 15 cents/unit, VFFS line compatible, 6-month timeline
```

**Expected Results**:
1. **Requirements**: All protection needs captured, markets identified with infrastructure data
2. **Framework**: CEFLEX selected (flexible EU), PPWR identified
3. **Material Screen**:
   - Current PET/PE/Al: FAILS recyclability (multilayer with Al)
   - Mono-material PE: PASSES recyclability, lower GWP
   - PET with EVOH: PASSES recyclability, higher barrier
4. **Scoring**:
   - Circularity: 45/100 (Recycle level, 30% recycled)
   - Recyclability: 35/100 (CEFLEX fails due to Al layer)
   - Lifecycle Impact: 52/100
5. **Roadmap**:
   - Quick win: Increase rPET to 50% (+8 pts)
   - Strategic bet: Switch to mono-material PE (+12 pts)
6. **Greenwashing**: "100% recyclable" claim flagged (US infrastructure gaps)

**Quality Gates**:
- [ ] Protection threshold respected
- [ ] Multilayer recyclability correctly assessed
- [ ] Impact factors cited with source
- [ ] PPWR regulations identified
- [ ] Greenwashing flag on recyclability claim

**Pass Criteria**: All outputs match expected structure, recyclability failure identified, roadmap includes mono-material alternative

---

## Scenario 2 — Greenwashing Flag
**Objective**: Validate greenwashing detection on vague environmental claims

**Input**:
```
Product: General consumer product
Current Packaging: Standard plastic bottle
Desired Claim: "Eco-friendly / 100% green / natural packaging"
Target Markets: EU, US
```

**Expected Results**:
1. **Claim Analysis**:
   - "Eco-friendly": FAIL (vague, not specific)
   - "100% green": FAIL (no substantiation)
   - "Natural": FAIL (not verifiable)
2. **Recommended Claims**:
   - "50% recycled content" (if true)
   - "30% lower carbon footprint vs. previous version" (if verified)
   - Or no claim if metrics unavailable
3. **Test Output**:
   ```
   Claim Substantiation Check:
   | Proposed Claim | Status | Issues | Recommended Claim |
   |----------------|--------|--------|------------------|
   | "Eco-friendly" | FAIL | Not specific, not substantiated, not verifiable | Use specific metrics only |
   | "100% green" | FAIL | Not specific, no substantiation | Specify environmental metric |
   | "Natural" | FAIL | Not verifiable | Remove unless certified |
   ```

**Quality Gates**:
- [ ] All four greenwashing criteria checked (specific, substantiated, verifiable, no misleading)
- [ ] Vague claims rejected
- [ ] Specific, data-backed claims suggested
- [ ] No "green" or "eco-friendly" alternatives allowed

**Pass Criteria**: All vague claims fail, only specific/substantiated/verifiable claims pass

---

## Scenario 3 — Compostable Without Facilities
**Objective**: Validate infrastructure reality check for compostability claims

**Input**:
```
Product: Cold drink cup
Current Packaging: PLA (bioplastic) cup, labeled "compostable"
Target Markets: US Midwest region (no industrial composting infrastructure)
Protection Needs: Liquid containment, 4-hour use
```

**Expected Results**:
1. **Framework Selection**:
   - Compostability standard: EN 13432 or ASTM D6400 identified
   - Infrastructure check: Industrial composting availability evaluated
2. **Infrastructure Reality**:
   - Local collection: 0% industrial composting coverage
   - Facilities: None within 100 miles
   - Actual EoL: Landfill (95%) or litter (5%)
3. **Scoring**:
   - Compostability claim: NOT VALID in local market
   - Recyclability: 0/100 (no collection infrastructure)
   - Recommendation: Switch to recyclable PET or PE
4. **Roadmap**:
   - Not recommended: "Compostable" labeling (greenwashing risk)
   - Recommended: PET recyclable cup with collection reality noted

**Quality Gates**:
- [ ] Compostability standard cited (EN 13432)
- [ ] Infrastructure reality explicitly checked
- [ ] Real-world EoL pathway stated
- [ ] Compostability claim rejected without facilities
- [ ] Greenwashing risk flagged

**Pass Criteria**: Infrastructure reality correctly identified, compostability claim rejected, alternative suggested

---

## Scenario 4 — Protection Threshold Respected
**Objective**: Validate that protection requirements constrain redesign options

**Input**:
```
Product: Fragile glassware (decorative vases)
Current Packaging: Double-wall corrugated with foam inserts
Protection Needs: Extreme fragility, must survive 1m drop, stacking 5 units high
User Request: "Minimize packaging, use sustainable materials"
Target Markets: EU, US
```

**Expected Results**:
1. **Requirements Capture**:
   - Fragility: Extreme
   - Drop test required: 1m
   - Stackability: 5 units high
2. **Material Screen**:
   - Paper-based: FAILS (insufficient impact protection)
   - Lightweight cardboard: FAILS (cannot support 5-unit stack)
   - Current corrugate + foam: PASSES protection
   - Molded pulp with cushioning: MAY PASS (needs testing)
3. **Scoring**:
   - Protection-compliant options only
   - Under-protective options explicitly rejected
4. **Roadmap**:
   - Not recommended: Single-wall cardboard (fails protection)
   - Recommended: Molded pulp with cushioning (if tested)
   - Quick win: Replace foam with recycled paper honeycomb (same protection, better circularity)
5. **Safety Check**:
   - All options pass minimum protection threshold
   - User warned: "Cannot recommend lightweighting without protection testing"

**Quality Gates**:
- [ ] Protection needs captured upfront
- [ ] Material screen includes protection test
- [ ] Under-protective options rejected
- [ ] Protection threshold enforced in roadmap
- [ ] User warned if requirements conflict

**Pass Criteria**: No under-protective recommendations, protection threshold explicitly enforced

---

## Scenario 5 — Material LCA Comparison
**Objective**: Validate LCA scope, impact factors, and burden-shifting flags

**Input**:
```
Product: 500ml beverage
Current Packaging: PET bottle, 30g
Target Markets: EU, US
Candidate Materials: Glass (500g), Aluminum (15g), PET (30g)
```

**Expected Results**:
1. **Framework Selection**:
   - LCA scope: Cradle-to-grave (includes EoL recycling benefit)
   - Functional unit: "per 1000 bottles"
   - Impact categories: GWP, energy, water
2. **Material Analysis**:
   - Glass: GWP 0.4 kg/unit (high transport weight), high recyclability
   - Aluminum: GWP 0.04 kg/unit (high production energy, high recycling benefit)
   - PET: GWP 0.07 kg/unit (balanced)
   - Impact factors: Source cited (ecoinvent v3.9, 2023)
3. **Burden-Shifting Flags**:
   - Glass: Lower GWP but 16x higher transport weight
   - Aluminum: High production energy, but 95% recycling benefit offsets
   - PET: Balanced, but lower recycling rate than Al
4. **Scoring**:
   - Each material: circularity, recyclability, impact scores
   - Trade-offs explicitly stated
5. **Roadmap**:
   - Rankings by overall score
   - Transport considerations for heavy materials
   - Recycling infrastructure considerations

**Quality Gates**:
- [ ] LCA scope explicitly stated
- [ ] Functional unit defined
- [ ] Impact factors cited with source and vintage
- [ ] Burden-shifting flagged
- [ ] Trade-offs explicit

**Pass Criteria**: LCA scope correct, factors cited, burden-shifting identified, trade-offs explicit

---

## Scenario 6 — Data Unavailable (Fallback)
**Objective**: Validate graceful degradation when impact-factor sources unavailable

**Input**:
```
Product: Standard product
Current Packaging: Multilayer film
Network Condition: Impact factor database offline (simulated)
```

**Expected Results**:
1. **Fetch Attempt**:
   - Attempt to fetch from ecoinvent (fails - simulated)
   - Fallback to cached impact factors
2. **Cached Data Usage**:
   - Use cached factors (2023 vintage)
   - Explicitly flag: "Using cached data, 2023 vintage"
   - Reduce confidence to "medium" or "low"
3. **Output**:
   ```
   Impact Data Notice:
   - Database: ecoinvent (unavailable)
   - Using: Cached factors from 2023
   - Confidence: Medium (data may be stale)
   - Recommendation: Verify with current database before production
   ```
4. **Scoring**:
   - Scores calculated with cached factors
   - Confidence level reduced
   - Vintage explicitly stated
5. **Roadmap**:
   - Recommendations flagged with data vintage caveat

**Quality Gates**:
- [ ] Fetch attempt logged
- [ ] Cached factors used as fallback
- [ ] Vintage explicitly stated
- [ ] Confidence level reduced
- [ ] User warned about stale data

**Pass Criteria**: Graceful fallback, vintage stated, confidence reduced, user warned

---

## Test Execution Guide

### Running Tests
Each test scenario should be executed through the main harness:

```
Input: Scenario N input specification
→ Execute main harness
→ Capture all stage outputs
→ Validate against expected results
```

### Test Validation Checklist
For each scenario:
- [ ] All stages execute successfully
- [ ] Output structure matches specification
- [ ] Quality gates all pass
- [ ] Critical edge cases handled correctly
- [ ] Error handling works (Scenario 6)
- [ ] Greenwashing detected (Scenarios 2, 3)
- [ ] Protection threshold enforced (Scenario 4)
- [ ] Infrastructure reality checked (Scenarios 1, 3)

### Test Coverage Matrix
| Scenario | Intake | Framework | Material | Scoring | Roadmap | Greenwash | Protection | Infrastructure | Fallback |
|----------|--------|-----------|----------|---------|---------|-----------|------------|----------------|----------|
| 1: Multilayer | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | - |
| 2: Greenwashing | - | - | - | - | ✓ | ✓ | - | - | - |
| 3: Compostable | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | - | ✓ | - |
| 4: Protection | ✓ | ✓ | ✓ | ✓ | ✓ | - | ✓ | - | - |
| 5: LCA | ✓ | ✓ | ✓ | ✓ | ✓ | - | - | ✓ | - |
| 6: Fallback | ✓ | ✓ | ✓ | ✓ | ✓ | - | - | - | ✓ |

**Coverage**: All stages tested, all quality gates covered, all edge cases validated.

---

## Continuous Validation

### Regression Tests
Run all 6 scenarios when:
- Sub-skill logic changes
- Impact factor cache updates
- New regulations added (PPWR updates)
- Infrastructure data changes

### Smoke Tests
Run Scenarios 1 and 2 when:
- Any code change
- Before release
- After infrastructure deployment

### Integration Tests
Run Scenarios 3, 4, 5 when:
- Framework changes
- Scoring algorithm updates
- Roadmap generation changes

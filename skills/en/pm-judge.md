---
name: pm-judge
description: Adaptive PM Validation Framework — adjust validation depth and adversarial intensity based on decision type and risk level.
---

# pm-judge — Adaptive Product Validator

## Input

- `decision`: Decision/hypothesis to validate (required)
- `decisionType`: Decision type (optional, default: feature, values: pricing / strategy / experiment / ecommerce)
- `risk`: Risk level (optional, default: mid, values: low / mid / high)
- `audience`: Output audience (optional, default: self, values: boss / team / self)
- `evidence`: Existing evidence (optional)

### Parameter Validation Rules

- Use default value when an optional parameter is not provided
- When a parameter value is not in the allowed values list, use the default value and append a warning: `⚠️ Parameter "{name}" value "{value}" is invalid, using default "{default}"`
- When `decision` is empty or not provided, output error: `❌ Missing required parameter "decision"`

## Adaptive Logic

### 1. Adversarial Intensity — By Decision Type

**Feature Decision (feature iteration):**
- Number of alternative explanations: 2
- Validation method: A/B testing or before/after comparison
- Acceptance criteria: p<0.05 or clear trend

**Pricing Decision (pricing adjustment):**
- Number of alternative explanations: 4
- Validation method: Multiple A/B tests + user research
- Acceptance criteria: p<0.01 + qualitative validation

**Strategy Decision (strategic direction):**
- Number of alternative explanations: 5
- Validation method: Multi-party validation + external data + expert review
- Acceptance criteria: Multiple independent evidence sources consistent

**Experiment Decision (experiment):**
- Number of alternative explanations: 1
- Validation method: Quick validation
- Acceptance criteria: Directional signal sufficient

**E-commerce Decision (product selection/pricing):**
- Number of alternative explanations: 3
- Validation method: Small-scale A/B testing + historical data comparison
- Acceptance criteria: Statistical significance + gross profit impact assessment

### 2. Hypothesis Tracking — Auto-classification

Not all hypotheses are worth tracking indefinitely. Classify by importance:

**P0 Hypothesis (core hypothesis):**
- If not validated, the entire solution is invalid
- Track until validated or refuted
- Report to decision-makers

**P1 Hypothesis (important hypothesis):**
- If not validated, the solution needs adjustment
- Track for 3 validation rounds
- Record conclusion only

**P2 Hypothesis (optimization hypothesis):**
- If not validated, does not affect core solution
- Validate 1 time only
- Record conclusion to notes

### 3. Output Format — By Audience

**Boss:**
```
## Validation Summary
Conclusion: ✅ Recommend continue / ❌ Recommend stop
Key evidence: [1 sentence]
Risk: [1 sentence]
```

**Team:**
```
## Validation Report
### Core Conclusion
### Validation Process
### Alternative Explanations
### Open Questions
### Recommendations
```

**Self:**
```
Failed? → Reason
Passed? → Next step
```

## Output Format (Decision Log — Closed Loop)

As the validation stage, pm-judge outputs the final decision log to complete the Think → Act → Prove loop:

```
## Decision Log
- Decision: [decision description]
- Decision type: [feature / pricing / strategy / experiment / ecommerce]
- Validation conclusion: ✅ Recommend continue / ❌ Recommend stop / ⚠️ Need more evidence
- Confidence: [high/medium-high/medium/medium-low/low]

## Hypothesis Tracking
| Hypothesis | Priority | Status | Evidence Source |
|-----------|----------|--------|-----------------|
| [Hypothesis 1] | P0/P1/P2 | Validated/Refuted/Pending | [Source] |

## Alternative Explanations
- [Alternative 1] — [Ruled out or not]
- [Alternative 2] — [Ruled out or not]

## Open Questions
- [Unresolved question]

## Recommendations
- [Next action]
```

## Integration with pm-loop

- When pm-judge validation conclusion is ❌, pm-loop should trigger the failure fallback mechanism
- When the number of refuted hypotheses reaches the threshold, automatically carry context back to pm-method

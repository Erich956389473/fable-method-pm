---
name: pm-loop
description: Adaptive PM Execution Framework — adjust execution depth and rhythm based on risk/stage/urgency.
---

# pm-loop — Adaptive Product Execution Loop

## Input

- `decision`: Decision path from pm-method output
- `risk`: Risk level (optional, default: mid, values: low / mid / high)
- `stage`: Product stage (optional, default: growth, values: 0-1 / growth / mature)
- `audience`: Output audience (optional, default: self, values: boss / team / self)
- `deadline`: Deadline (optional)

### Parameter Validation Rules

- Use default value when an optional parameter is not provided
- When a parameter value is not in the allowed values list, use the default value and append a warning: `⚠️ Parameter "{name}" value "{value}" is invalid, using default "{default}"`
- When `decision` is empty or not provided, output error: `❌ Missing input "decision", please run pm-method first to get the decision path`

## Adaptive Logic

### 1. Evidence Collection — By Product Stage

**0-1 Stage:**
- Collection list: 3-5 user interviews + competitive analysis
- Pure data solutions not accepted (no data exists)
- Time box: Complete within 1 week

**Growth Stage:**
- Collection list: Data dashboard + user feedback + competitive analysis
- Minimum 2 types of evidence
- Time box: Complete within 2 weeks

**Mature Stage:**
- Collection list: A/B testing + user research + data models
- Statistical significance required
- Time box: Complete within 1 month

**E-commerce Evidence Supplement:**
- Product selection stage: Competitor price tracking, sales data, inventory turnover rate
- Promotion stage: Historical promotion data, coupon redemption rate
- Logistics stage: Fulfillment timeliness, return rate distribution

### 2. INTENT Format — By Audience

**For boss:**
```
INTENT (concise version):
  Problem: One sentence
  Decision: One solution
  Expected: One number
```

**For team:**
```
INTENT (complete version):
  Problem: Background + current status
  Evidence: Data + user voice
  Decision: Solution + acceptance criteria
  Risk: When to stop
  Timeline: Timeline
```

**For self:**
```
INTENT (shorthand version):
  What to do?
  Why?
  When to finish?
```

### 3. Execution Rhythm — By Urgency

**Critical:**
- Skip planning, execute directly
- Sync every 30 minutes
- Review after stopping the bleeding

**Urgent:**
- Simplify planning, deliver via fastest path
- Daily sync
- Complete documentation after delivery

**Normal:**
- Standard process
- Weekly sync
- Complete documentation

### 4. Verification Depth — By Risk Level

**Low risk:**
- Launch counts as verification
- Observe for 3 days with no anomalies

**Mid risk:**
- A/B testing or before/after comparison
- Observe for 1-2 weeks

**High risk:**
- Gradual rollout + A/B testing
- Observe for 1 month + statistical significance validation
- Rollback plan ready

## Output Format (for downstream skills)

After pm-loop execution completes, output the following structured information for pm-judge to consume:

```
## Execution Result
- Decision: [original decision description]
- Execution status: [completed / in progress / blocked]
- Hypothesis validation record:
  - Hypothesis A: [validated / refuted / pending] — [evidence source]
  - Hypothesis B: [validated / refuted / pending] — [evidence source]
- Key data: [metric name: value]
- Risk signals: [discovered risks or anomalies]
```

## Failure Fallback Mechanism

When the number of hypothesis validation failures reaches the failure threshold defined by pm-method:

1. **Auto-trigger**: Output `🔄 Failure threshold reached ({N} times), recommend returning to pm-method to redefine the problem`
2. **Carry context**: Pass failure records as input to pm-method in format:
   ```
   problem: [original problem]
   previous_failures: [list of failed hypotheses]
   trigger: threshold_reached
   ```
3. **Re-classification**: pm-method re-evaluates problem classification and path based on failure records

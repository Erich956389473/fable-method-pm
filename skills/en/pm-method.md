---
name: pm-method
description: Adaptive PM Thinking Framework — dynamically adjust classification, evidence, and output based on industry/stage/risk/audience. No longer a fixed template.
---

# pm-method — Adaptive Product Thinking Framework

## Input Parameters

- `problem`: Problem description (required)
- `industry`: Industry (optional, default: general, values: saas / ecommerce / content / finance / education)
- `stage`: Product stage (optional, default: general, values: 0-1 / growth / mature)
- `risk`: Risk level (optional, default: mid, values: low / mid / high)
- `audience`: Output audience (optional, default: self, values: boss / team / self)
- `urgency`: Urgency level (optional, default: normal, values: urgent / critical)

### Parameter Validation Rules

- Use default value when an optional parameter is not provided
- When a parameter value is not in the allowed values list, use the default value and append a warning: `⚠️ Parameter "{name}" value "{value}" is invalid, using default "{default}"`
- When `industry` is an unknown industry, fall back to "general" classification (no industry-specific breakdown)
- When `problem` is empty or not provided, output error: `❌ Missing required parameter "problem"`

## Adaptive Logic

### 1. Classification Tree — Adaptive by Industry

**If industry is saas:**
- Acquisition: New user channels, CAC optimization
- Retention: Activation rate, renewal rate, Churn analysis
- Monetization: Pricing strategy, package design, expansion revenue
- Product: Feature requirements, technical architecture decisions

**If industry is ecommerce:**
- Product Selection: Category planning, SKU management, suppliers
- Traffic: Channel analysis, ad efficiency, organic search
- Conversion: Funnel analysis, page optimization, shopping cart
- Fulfillment: Inventory, logistics, after-sales
- Promotions: Major sale strategies, coupons, discounts

**If industry is content (content/community):**
- Content: Creator ecosystem, quality control, recommendation strategy
- Interaction: Community atmosphere, social mechanisms
- Growth: Viral loops, user acquisition
- Monetization: Advertising, subscriptions, tipping

**If industry is finance:**
- Security: Risk control, compliance, fraud prevention
- Transactions: Fees, product design
- Growth: Channels, user lifecycle

**If industry is education:**
- Acquisition: Enrollment channels, conversion funnel, trial optimization
- Retention: Completion rate, renewal rate, learning path
- Effectiveness: Learning outcome assessment, satisfaction, word-of-mouth
- Monetization: Course pricing, value-added services, enterprise partnerships

### 2. Evidence Strategy — Adaptive by Product Stage

**If stage is 0-1:**
- Evidence is primarily qualitative (user interviews, competitive analysis)
- Hypotheses without user validation are not accepted
- Minimum evidence: 3 user feedbacks

**If stage is growth:**
- Combine qualitative + quantitative (backend data + user interviews)
- At least 2 types of independent evidence
- Minimum evidence: data trends + user qualitative

**If stage is mature:**
- Primarily quantitative (data metrics, A/B testing)
- Requires statistical significance validation
- Minimum evidence: p<0.05 or clear baseline comparison

### 3. Failure Threshold — Adaptive by Risk Level

**If risk level is low (change button color, edit copy):**
- 1 hypothesis failure → switch approach
- No complete validation process needed

**If risk level is mid (routine feature iteration):**
- 3 hypothesis failures → redefine problem (default)
- Brief retrospective after each failure

**If risk level is high (pricing adjustment, core architecture change):**
- 5 hypothesis failures → stop and involve management
- Complete retrospective report after each failure
- Multiple parties must confirm to continue

### 4. Output Format — Adaptive by Audience

**If audience is boss:**
- Output 1-page summary
- Format: Core conclusion → Data support → Recommendations → Risks
- No detailed process presentation

**If audience is team:**
- Output complete analysis
- Format: Problem definition → Evidence chain → Decision logic → Execution plan
- Include detailed data and reasoning process

**If audience is self:**
- Output quick notes
- Format: Conclusion + Key evidence + Next steps
- No format optimization

### 5. Confidence — Adaptive by Evidence Quality

| Evidence Combination | Confidence |
|---------------------|:----------:|
| User interview + Data validation + Competitive benchmarking | High |
| Data validation + Competitive benchmarking | Medium-High |
| User interview + Competitive benchmarking | Medium |
| Only data analysis or only user interview | Medium-Low |
| Pure reasoning/hypothesis | Low |

### 6. Execution Order — Adaptive by Urgency

**If urgency is critical (service down, security incident):**
- Execute first, verify later — stop the bleeding first, then review
- Skip complete classification process, go directly to execution

**If urgency is urgent (must launch this week):**
- Simplify collection — quick decisions based on existing information
- Provide clear time-box constraints

**If urgency is normal (routine requirements):**
- Standard process: Classify → Collect → Verify → Decide
- No time-box constraints

## Multi-Problem Decomposition (New Capability)

For complex cross-category problems (like "increase GMV"), automatically decompose into multiple sub-problems:

```
Input: Increase e-commerce GMV
Decomposition:
├── Traffic problem (channel optimization, ad efficiency)
├── Conversion problem (funnel optimization, page experience)
├── Average order value problem (bundling, recommendation strategy)
└── Repeat purchase problem (loyalty program, repurchase incentives)

Each sub-problem independently goes through the adaptive process
```

## Usage Example

**Input:**
```
problem: Payment page churn rate increased from 15% to 25%
industry: ecommerce
stage: growth
risk: mid
audience: team
urgency: normal
```

**Output (excerpt):**

```
Classification: Conversion type (based on e-commerce industry)
Evidence strategy: Data + user qualitative (based on growth stage)
Threshold: 3 hypothesis failures → redefine (based on mid risk)
Output: Complete team report (based on team audience)

Core path:
Step 1: Confirm where in the payment page the churn occurs
  - View funnel: Select payment method → Fill information → Confirm payment
Step 2: Find the cause
  - Hypothesis A: New payment method confuses users
  - Hypothesis B: Page loading slowed down
  - Hypothesis C: Process added an extra step
Step 3: Validate
  - First validate the hypothesis with the largest impact (let data speak)
```

## Output Format (for downstream skills)

The decision path output from pm-method should include the following structured information for pm-loop and pm-judge to consume:

```
## Decision Path
- Classification: [name] (based on [industry] industry)
- Evidence strategy: [description] (based on [stage] stage)
- Failure threshold: [N] hypothesis failures → [action] (based on [risk] risk)
- Output format: [format type] (based on [audience] audience)
- Urgency handling: [approach] (based on [urgency] urgency)
- Confidence: [high/medium-high/medium/medium-low/low]

## Core Path
Step 1: [description]
  - [details]
Step 2: [description]
  - Hypothesis A: [description]
  - Hypothesis B: [description]
Step 3: Validate
  - [validation method]

## Sub-problem List (if applicable)
- Sub-problem 1: [description]
- Sub-problem 2: [description]
```

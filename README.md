# Procedure Cost/Utilization Intelligence

**Tagline:** *Understand where procedure utilization and cost differ—and why.*

## 1. Product Purpose

Build an interactive healthcare analytics application that analyzes **procedure utilization, allowed amounts, paid amounts, unit costs, provider variation, geographic variation, and utilization trends** across procedures, providers, specialties, places of service, and payers.

The application should help an analyst move beyond:

> “How many procedures occurred?”

to:

> **“Where is utilization occurring, what is it costing, how does it vary, and what factors should I investigate?”**

The product combines:

* healthcare claims analytics
* CPT/HCPCS intelligence
* provider analytics
* cost analytics
* utilization analytics
* peer benchmarking
* geographic analysis
* temporal analysis
* procedure relationships
* anomaly detection
* explainable AI
* Streamlit

---

# 2. Core Business Questions

The application should answer:

* Which procedures have the highest utilization?
* Which procedures have the highest allowed or paid amounts?
* What is the average allowed amount per procedure?
* How does cost vary by provider?
* How does cost vary by geography?
* How does cost vary by place of service?
* Which providers have unusually high or low unit costs?
* Which providers have unusually high utilization?
* Are high-utilization providers also high-cost providers?
* Are cost differences driven by unit price, utilization, or both?
* How has utilization changed over time?
* How has cost changed over time?
* Which procedures have the largest variation?
* What diagnoses are associated with the highest-cost utilization?
* Are there procedure pairs with different utilization/cost patterns?
* What areas should an analyst investigate?

---

# 3. Portfolio Positioning

This project fits naturally with your existing portfolio.

### Procedure Intelligence

**What is this procedure and how is it being used?**

### Procedure Cost/Utilization Intelligence

**How much is it being used, what does it cost, and where does it vary?**

### Procedure Substitution Intelligence

**Which procedures appear to be used in place of one another?**

### Coding Anomaly Detector

**Which coding behaviors look unusual?**

### Payer Policy Diff Engine

**What changed in payer policy?**

### Prior Authorization Risk Engine

**What should be investigated before the procedure occurs?**

### Denial Prevention Engine

**What could cause the claim to fail?**

Together:

```text id="6z9q5n"
PROCEDURE
   │
   ├── UTILIZATION
   │
   ├── COST
   │
   ├── CODING
   │
   ├── CLINICAL CONTEXT
   │
   ├── SUBSTITUTION
   │
   ├── COVERAGE / POLICY
   │
   ├── AUTHORIZATION
   │
   └── DENIAL
```

This gives you a reusable **Procedure Intelligence ecosystem** rather than a collection of unrelated dashboards.

---

# 4. Define Cost Carefully

One of the most important design decisions is to distinguish different financial measures.

Depending on the claims data available, include:

### Billed Amount

What was submitted by the provider.

### Allowed Amount

The amount recognized by the payer/claims system.

### Paid Amount

The amount paid by the payer.

### Member Responsibility

When available:

* deductible
* coinsurance
* copayment

### Unit Cost

For procedures with meaningful units:

```text
allowed amount / units
```

or:

```text
paid amount / units
```

Do not casually refer to these all as “cost.”

The application should always label the financial metric being displayed.

---

# 5. Core Analytical Framework

The heart of the product should be:

```text id="f9o5ih"
UTILIZATION
     +
UNIT PRICE
     +
MIX
     ↓
TOTAL SPEND
```

For example:

```text id="e6p7sj"
Provider A

High utilization
+
Normal unit cost
=
High total spend
```

versus:

```text id="c1f3h8"
Provider B

Normal utilization
+
High unit cost
=
High total spend
```

Those represent very different analytical situations.

---

# 6. Core Metrics

For each procedure calculate:

### Utilization

* claim count
* claim-line count
* beneficiary count
* provider count
* units
* procedures per beneficiary
* procedures per provider

### Financial

* billed amount
* allowed amount
* paid amount
* member responsibility
* allowed per claim
* allowed per unit
* paid per claim
* paid per unit

### Variation

* median
* mean
* standard deviation
* percentile
* coefficient of variation
* minimum/maximum where appropriate

---

# 7. Normalize Cost

Raw cost comparisons can be misleading.

For example:

```text id="8vq5ip"
Provider A
$100,000 total allowed

Provider B
$80,000 total allowed
```

Provider A may simply have twice the number of procedures.

Therefore show:

```text id="yq8pl0"
Total Allowed
Allowed / Claim
Allowed / Unit
Allowed / Beneficiary
```

This allows analysts to separate:

**volume differences** from **price differences**.

---

# 8. Procedure Cost Profile

For every CPT/HCPCS code create a procedure cost/utilization profile.

Example:

```text id="3p2fr9"
Procedure: XXXXX

Claims:             18,421
Beneficiaries:       7,320
Providers:             412

Total Allowed:     $4.2M
Allowed / Claim:     $228
Allowed / Unit:      $185

Median Allowed:      $190
90th Percentile:      $340
```

Then show:

* utilization trend
* cost trend
* provider variation
* POS variation
* geographic variation
* diagnosis context

---

# 9. Provider Cost Intelligence

Compare providers performing the same procedure.

Example:

| Provider | Claims | Allowed/Claim | Allowed/Unit |
| -------- | -----: | ------------: | -----------: |
| A        |  1,200 |          $210 |         $190 |
| B        |    850 |          $315 |         $290 |
| C        |  1,400 |          $205 |         $185 |

This allows investigation of price variation.

The application should provide the comparison context:

* payer
* procedure
* time period
* POS
* specialty
* geography

---

# 10. Provider Utilization Intelligence

For each provider:

```text id="5qj5tz"
Procedure Volume
Beneficiary Volume
Units
Procedures / Beneficiary
Procedure Mix
```

Then compare with peers.

Possible peer dimensions:

* specialty
* taxonomy
* geography
* POS
* payer

Example:

> Provider utilization was above the selected peer-group distribution during the selected period.

Avoid implying that higher utilization is inherently inappropriate.

---

# 11. Cost vs Utilization Matrix

This should be one of the application's flagship visualizations.

Plot:

**X-axis:** utilization rate

**Y-axis:** allowed amount per procedure

Four analytical quadrants:

```text id="x9kqv7"
             HIGH COST
                 │
      B          │       A
                 │
─────────────────┼─────────────────
                 │
      C          │       D
                 │
             LOW COST

       LOW       │      HIGH
              UTILIZATION
```

The quadrants are not rankings.

They simply provide an analytical framework:

### High utilization / high unit cost

Potentially important for investigation because both dimensions are elevated.

### High utilization / lower unit cost

High volume with relatively lower unit cost.

### Lower utilization / high unit cost

Lower volume but potentially meaningful price variation.

### Lower utilization / lower unit cost

Lower activity across both dimensions.

Always provide the underlying metrics.

---

# 12. Cost Variation Analysis

For each procedure calculate the distribution across providers.

Example:

```text id="7e4c5d"
Allowed Amount

$100 ────────────────
$150
$200
$250
$300
$350 ────────────────
```

Show:

* median
* 25th percentile
* 75th percentile
* 90th percentile
* 95th percentile

A box plot can be particularly effective.

---

# 13. Place-of-Service Analysis

Compare:

* office
* outpatient hospital
* inpatient
* ASC
* emergency department
* laboratory
* DME
* other POS

Example:

```text id="n5d8ce"
Procedure X

Office:
$180 / claim

Hospital Outpatient:
$420 / claim

ASC:
$310 / claim
```

The application should make it clear that different settings can have legitimate differences in reimbursement and case mix.

---

# 14. Geographic Intelligence

Where geographic information is available, analyze:

* state
* ZIP3
* metropolitan area
* provider market
* county

Show:

```text id="78a9bk"
Procedure
    ↓
Geography
    ↓
Utilization
    ↓
Unit Cost
    ↓
Variation
```

Potential visualizations:

* map
* geographic table
* distribution chart

Avoid interpreting geographic variation without considering payer, provider mix, specialty, POS, and patient mix.

---

# 15. Temporal Intelligence

Track:

### Utilization

```text id="5cnm0e"
Month → claim volume
```

### Unit cost

```text id="6kzq1q"
Month → allowed/claim
```

### Total spend

```text id="c9q1v2"
Month → total allowed
```

### Trend decomposition

A useful analytical feature is:

> Did spending increase because utilization increased, price increased, or both?

---

# 16. Spend Decomposition

Build a decomposition model.

For example:

```text id="i2k0n5"
Total Spend Change
       │
       ├── Volume Effect
       │
       ├── Unit Price Effect
       │
       └── Mix Effect
```

This is more analytically valuable than simply showing that spend increased.

Example:

> Total allowed amount increased 18%. Most of the observed increase was associated with higher utilization, while allowed amount per unit changed less during the same period.

This should be presented as a descriptive decomposition, not a causal conclusion.

---

# 17. Diagnosis / Clinical Context

Connect procedures to diagnoses.

For each procedure show:

* top diagnoses
* diagnosis share
* beneficiaries
* utilization
* allowed amount
* allowed amount per claim

Then:

```text id="zzayj3"
Procedure
   ↓
Diagnosis
   ↓
Utilization
   ↓
Cost
```

This lets analysts investigate whether cost differences are concentrated within particular observed diagnosis contexts.

---

# 18. Clinical Concept Integration

Use your **Procedure-to-Clinical-Concept Graph**.

Example:

```text id="ml0rxw"
Procedure
    ↓
Diagnosis
    ↓
Clinical Concept
    ↓
Utilization
    ↓
Cost
```

This creates a more meaningful analytical hierarchy than simply grouping codes.

---

# 19. Procedure Substitution Integration

Connect to **Procedure Substitution Intelligence**.

Suppose Procedure A and B appear to have overlapping utilization contexts.

Compare:

```text id="i5r5rd"
             Procedure A       Procedure B

Volume          10,000             6,000
Allowed/Unit      $250               $140
Total Allowed   $2.5M              $840K
```

Then analyze:

* utilization trend
* unit cost trend
* shared diagnoses
* shared providers
* shared clinical concepts
* POS
* payer

The application can identify:

> “These procedures have overlapping observed utilization contexts, and their utilization and cost patterns differ.”

It should not recommend one procedure over another.

---

# 20. Coding Change Integration

Connect to **Coding Change Intelligence**.

A code changes or is replaced.

Then ask:

```text id="l8p72f"
Coding Change
      ↓
Procedure Utilization
      ↓
Procedure Cost
      ↓
Replacement / Related Codes
      ↓
Spend Change
```

This enables an interesting portfolio use case:

> Did a coding change coincide with a change in utilization or spend?

Again, temporal association is not automatically causation.

---

# 21. Payer Policy Integration

Connect to the **Payer Policy Diff Engine**.

Example:

```text id="q3c4yw"
Policy Change
      ↓
Procedure Coverage / PA
      ↓
Utilization
      ↓
Cost
```

Show the policy change and utilization/cost trend together.

This lets an analyst investigate whether a policy change coincided with changes in procedure activity.

---

# 22. Anomaly Detection

Reuse concepts from your **Coding Anomaly Detector**.

Detect:

### Utilization anomalies

Provider usage substantially differs from peers.

### Unit-cost anomalies

Allowed amount per unit differs sub

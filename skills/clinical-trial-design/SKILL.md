---
name: clinical-trial-design
description: Assess clinical trial design feasibility for biopharma due diligence. Evaluates patient population, biomarker prevalence, endpoint selection, comparator analysis, enrollment projections, and regulatory precedent. Use when users ask about trial design, feasibility assessment, enrollment projections, endpoint strategy, or trial success probability.
user-invocable: false
---

# Clinical Trial Design Feasibility

Assess clinical trial design feasibility for due diligence and development strategy using Gosset data.

## Data Sources

### Gosset MCP Tools
- `find_trials(query, limit)` — Precedent trial designs, enrollment data
- `find_drugs(query, limit, effort)` — Competitor trial strategies
- `gosset_agent(prompt)` — PTRS scoring, deep research on trial design precedents, head-to-head comparisons

### ClinicalTrials.gov API (Free, no key)
```python
import requests

# Find precedent trial designs with detailed protocol info
r = requests.get("https://clinicaltrials.gov/api/v2/studies", params={
    "query.term": "NSCLC PD-1 Phase 3",
    "filter.overallStatus": "COMPLETED",
    "pageSize": 50,
    "fields": "NCTId,BriefTitle,Phase,EnrollmentCount,PrimaryOutcomeMeasure,StartDate,CompletionDate,LeadSponsorName,StudyType,DesignAllocation,DesignMasking"
})

# Get full protocol for a specific trial
r = requests.get("https://clinicaltrials.gov/api/v2/studies/NCT02578680")
```

### openFDA API (Free)
```python
# Check regulatory precedent — what endpoints led to approval
r = requests.get("https://api.fda.gov/drug/drugsfda.json", params={
    "search": 'products.active_ingredients.name:"pembrolizumab"',
    "limit": 5
})
```

### Web Search (supplementary)
- FDA guidance documents for endpoint recommendations
- Published trial results for benchmark data

## When to Use

- Evaluating whether a company's trial design is competitive
- Assessing enrollment feasibility for DD
- Comparing trial designs across competitors
- Evaluating endpoint strategy and regulatory precedent
- Estimating probability of trial success

## Workflow

### Step 1: Gather Precedent Data

```
# Find similar trials in the indication
find_trials(query="Phase [X] [indication] [mechanism]", limit=30)

# Find competitor drugs and their trials
find_drugs(query="[indication] [mechanism] Phase 2 Phase 3", limit=30, effort="high")

# Get regulatory precedent
gosset_agent(prompt="What trial designs and endpoints have led to FDA approval in [indication]? What were the sample sizes, comparators, and primary endpoints?")
```

### Step 2: Feasibility Assessment

#### Patient Population (30% weight)
- Incidence/prevalence of the indication
- Biomarker prevalence (if biomarker-selected)
- Geographic distribution
- Screen failure rate estimates
- Competing trials recruiting same population

#### Endpoint Strategy (25% weight)
- Regulatory precedent for primary endpoint
- Effect size needed and clinical meaningfulness
- Duration to endpoint
- Surrogate vs. clinical endpoint trade-offs

#### Comparator Analysis (15% weight)
- Standard of care assessment
- Active vs. placebo comparator justification
- Ethical considerations
- Standard of care evolution risk

#### Regulatory Pathway (20% weight)
- Precedent approvals in the indication
- Agency guidance documents
- Special designations available
- Accelerated pathways vs. full approval

#### Safety Monitoring (10% weight)
- Known class-effect safety signals
- DSMB requirements
- REMS considerations

### Step 3: Trial Success Probability

Use Gosset's PTRS (Predictive Trial Risk Scoring) agent:
```
gosset_agent(prompt="Predict the probability of success for a Phase [X] trial of [drug] ([mechanism]) in [indication]. Consider: sample size [N], primary endpoint [endpoint], comparator [comparator], patient population [details].")
```

### Step 4: Enrollment Projections

Based on precedent trials:
- Sites needed (based on enrollment rate per site)
- Geographic distribution
- Time to full enrollment
- Enrollment competition from other active trials

```
find_trials(query="recruiting [indication]", limit=50)
```
Count active recruiting trials to assess enrollment competition.

### Step 5: Feasibility Score (0-100)

| Component | Weight | Score |
|-----------|--------|-------|
| Patient Availability | 30% | |
| Endpoint Precedent | 25% | |
| Regulatory Clarity | 20% | |
| Comparator Feasibility | 15% | |
| Safety Monitoring | 10% | |
| **Total** | **100%** | |

### Step 6: Report

1. **Executive Summary** — Feasibility score, key risks, recommendation
2. **Trial Design Comparison** — Table comparing precedent trials
3. **Patient Population Assessment** — Size, biomarker, enrollment competition
4. **Endpoint Analysis** — Precedent, regulatory guidance, recommended approach
5. **Enrollment Projection** — Timeline, sites, competitive recruitment risk
6. **Probability of Success** — PTRS score with key drivers
7. **Recommendations** — Design optimization suggestions

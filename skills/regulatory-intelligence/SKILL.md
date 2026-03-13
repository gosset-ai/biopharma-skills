---
name: regulatory-intelligence
description: Regulatory intelligence for biopharma due diligence and strategy. Covers FDA/EMA approvals, regulatory pathways, advisory committees, PDUFA dates, breakthrough/fast track designations, and biosimilar/generic entry. Use when users ask about regulatory status, approval timelines, FDA filings, regulatory strategy, or agency interactions.
user-invocable: false
---

# Regulatory Intelligence

Track and analyze regulatory activities for biopharma assets and therapeutic areas.

## Data Sources

### Gosset MCP Tools
- `find_drugs(query, limit, effort)` — Drug approval status, regulatory phase
- `find_trials(query, limit)` — Registrational trials and their status
- `find_companies(query, limit)` — Company regulatory track record
- `gosset_agent(prompt)` — Deep research on regulatory history and strategy

### openFDA API (Free, no key required for basic use)
Query FDA regulatory data directly via `https://api.fda.gov/`. Key endpoints:

```python
import requests

# Drug adverse events
r = requests.get("https://api.fda.gov/drug/event.json", params={
    "search": "patient.drug.medicinalproduct:pembrolizumab",
    "limit": 10
})

# Drug labeling (prescribing information)
r = requests.get("https://api.fda.gov/drug/label.json", params={
    "search": 'openfda.brand_name:"Keytruda"',
    "limit": 1
})

# Drugs@FDA (approval history since 1939)
r = requests.get("https://api.fda.gov/drug/drugsfda.json", params={
    "search": 'openfda.brand_name:"Keytruda"',
    "limit": 5
})

# Drug recalls / enforcement
r = requests.get("https://api.fda.gov/drug/enforcement.json", params={
    "search": "reason_for_recall:pembrolizumab",
    "limit": 10
})

# Device 510(k) clearances
r = requests.get("https://api.fda.gov/device/510k.json", params={
    "search": "applicant:Medtronic",
    "limit": 10
})

# Device PMA approvals
r = requests.get("https://api.fda.gov/device/pma.json", params={
    "search": "advisory_committee_description:Cardiovascular",
    "limit": 10
})
```

Rate limits: 240 req/min without key, 120K/day with free key from https://open.fda.gov/apis/authentication/

### ClinicalTrials.gov API (Free)
```python
# Search trials via v2 API
r = requests.get("https://clinicaltrials.gov/api/v2/studies", params={
    "query.term": "pembrolizumab NSCLC",
    "filter.overallStatus": "COMPLETED",
    "pageSize": 20
})
```

### Web Search (supplementary)
- FDA.gov — Approval letters, advisory committee materials, PDUFA dates
- EMA.europa.eu — CHMP opinions, EPARs, marketing authorizations
- FDA Orange Book — Patent and exclusivity listings
- FDA Purple Book — Biosimilar reference products

## When to Use

- Due diligence on regulatory risk for an asset
- Tracking PDUFA dates and regulatory milestones
- Analyzing regulatory precedent in an indication
- Evaluating regulatory pathway options
- Monitoring advisory committee outcomes
- Assessing biosimilar/generic entry timing

## Workflow

### Step 1: Gather Regulatory Data

#### Asset Regulatory Profile:
```
find_drugs(query="[drug name]", limit=5, effort="high")
gosset_agent(prompt="What is the complete regulatory history for [drug name]? Include filing dates, approval dates, label expansions, CRLs, advisory committees, breakthrough/fast track designations, and REMS requirements.")
```

#### Indication Regulatory Landscape:
```
find_drugs(query="[indication] approved", limit=50, effort="high")
gosset_agent(prompt="What are the FDA-approved drugs for [indication]? What endpoints and trial designs led to approval? What regulatory precedents exist?")
```

### Step 2: Regulatory Analysis

#### Approval Pathway Assessment:
| Pathway | Criteria | Timeline Impact |
|---------|----------|-----------------|
| Standard NDA/BLA | Full review | 10-12 months |
| Priority Review | Significant improvement | 6 months |
| Accelerated Approval | Surrogate endpoint | Earlier filing, confirmatory trial needed |
| Breakthrough Therapy | Substantial improvement | Rolling review, intensive guidance |
| Fast Track | Serious condition, unmet need | Rolling review |
| RMAT (cell/gene) | Regenerative medicine | Priority review, accelerated approval |
| Orphan Drug | <200K patients | 7 years exclusivity, tax credits |
| 505(b)(2) | Known active ingredient | Reduced data package |

#### Key Regulatory Risks:
- Complete Response Letter (CRL) history in the indication
- Advisory committee voting patterns
- Post-marketing requirements and REMS
- Label restrictions (black box warnings, REMS)
- Manufacturing/CMC issues

### Step 3: Regulatory Timeline

Build a regulatory milestone timeline:

| Date | Milestone | Status | Impact |
|------|-----------|--------|--------|
| [date] | IND filing | Complete | Cleared |
| [date] | Breakthrough designation | Granted/Denied | Accelerated path |
| [date] | NDA/BLA submission | Planned/Filed | Under review |
| [date] | PDUFA date | [date] | Binary event |
| [date] | Advisory committee | Scheduled | Sentiment signal |
| [date] | EU MAA | Planned | Ex-US access |

### Step 4: Report

1. **Executive Summary** — Regulatory status, key risks, expected timeline
2. **Regulatory History** — Complete timeline of regulatory interactions
3. **Pathway Analysis** — Which pathway, why, and precedents
4. **Regulatory Risk Assessment** — Key risks with mitigation strategies
5. **Competitive Regulatory Context** — How competitors' regulatory paths compare
6. **Recommendations** — Regulatory strategy implications for the deal/program

---
name: drug-repurposing
description: Identify drug repurposing candidates using target-based, compound-based, and disease-driven strategies. Searches existing drugs for new therapeutic indications by analyzing targets, clinical data, safety profiles, and deal precedents. Use when exploring repurposing opportunities, new indications for approved drugs, or therapeutic alternatives.
user-invocable: false
---

# Drug Repurposing Analysis

Systematically identify and evaluate drug repurposing candidates using Gosset's biopharma data.

## Data Sources

### Gosset MCP Tools
- `find_drugs` — Find approved/clinical drugs and their current indications
- `find_trials` — Find trials in new indications
- `find_deals` — Find licensing deals for repurposed assets
- `find_companies` — Find companies pursuing repurposing strategies
- `gosset_agent` — Deep research on mechanisms, clinical evidence, safety

### Open Targets Platform API (Free, no key)
```python
import requests

# Disease-target associations (find targets for a new indication)
r = requests.post("https://api.platform.opentargets.org/api/v4/graphql", json={
    "query": """query($efoId: String!, $size: Int!) {
        disease(efoId: $efoId) {
            associatedTargets(page: {size: $size, index: 0}) {
                rows { target { id approvedSymbol } score datatypeScores { id score } }
            }
        }
    }""",
    "variables": {"efoId": "EFO_0003060", "size": 25}
})
```

### openFDA FAERS (Free)
```python
# Adverse event data — unexpected benefits can signal repurposing opportunities
r = requests.get("https://api.fda.gov/drug/event.json", params={
    "search": "patient.drug.medicinalproduct:metformin",
    "count": "patient.reaction.reactionmeddrapt.exact",
    "limit": 100
})
```

### PubMed / Entrez (Free)
```python
from Bio import Entrez
Entrez.email = "your@email.com"
# Off-label use evidence
handle = Entrez.esearch(db="pubmed", term='"metformin" AND "cancer" AND (repurposing OR off-label)', retmax=50)
```

### Web Search (supplementary)
- Patent expiry data for commercial opportunity
- Analyst reports on repurposing programs

## Core Strategies

### 1. Target-Based Repurposing
Start with disease targets, find approved drugs that modulate them.

```
# Step 1: Identify targets for the new indication
gosset_agent(prompt="What are the validated drug targets for [new indication]?")

# Step 2: Find approved drugs hitting those targets
find_drugs(query="[target] approved", limit=50, effort="high")

# Step 3: Check if any are already in trials for new indication
find_trials(query="[drug name] [new indication]", limit=10)
```

### 2. Compound-Based Repurposing
Start with a known drug, find new indications.

```
# Step 1: Get drug's full target/mechanism profile
find_drugs(query="[drug name]", limit=5, effort="high")

# Step 2: Find diseases associated with those targets
gosset_agent(prompt="What diseases are associated with [target] beyond [current indication]?")

# Step 3: Check clinical evidence
find_trials(query="[drug name]", limit=30)
```

### 3. Disease-Driven Repurposing
Start with an underserved disease, find existing drugs.

```
# Step 1: Map the disease biology
gosset_agent(prompt="What are the key pathways and targets in [disease]?")

# Step 2: Find drugs in related indications
find_drugs(query="[related indication] approved", limit=50, effort="high")

# Step 3: Check deal precedents
find_deals(query="[disease] repurposing licensing", limit=20)
```

## Evaluation Criteria

For each repurposing candidate, assess:

| Criterion | Assessment |
|-----------|------------|
| **Mechanistic rationale** | How strong is the biological link? |
| **Clinical evidence** | Any clinical/real-world data in new indication? |
| **Safety profile** | Compatible with new patient population? |
| **Patent/exclusivity** | Is there IP protection available? |
| **Competitive landscape** | How crowded is the new indication? |
| **Commercial opportunity** | Market size, unmet need, pricing |
| **Regulatory pathway** | 505(b)(2)? Orphan? Pediatric? |
| **Deal precedent** | Similar repurposing deals and valuations? |

## Repurposing Score (0-100)

| Component | Weight |
|-----------|--------|
| Mechanistic rationale | 20% |
| Clinical evidence | 25% |
| Safety compatibility | 15% |
| IP/exclusivity | 15% |
| Commercial opportunity | 15% |
| Regulatory pathway clarity | 10% |

## Output Format

### Repurposing Opportunity Report: [Drug] → [New Indication]

1. **Executive Summary** — Score, recommendation, key rationale
2. **Mechanistic Link** — Biological rationale with evidence grading
3. **Clinical Evidence** — Existing data (trials, case reports, RWE)
4. **Safety Considerations** — Profile fit for new population
5. **Competitive Context** — Other therapies in the new indication
6. **Commercial Assessment** — Market size, IP landscape, regulatory path
7. **Deal Comparables** — Similar repurposing transactions
8. **Recommendation** — Pursue / Monitor / Pass with rationale

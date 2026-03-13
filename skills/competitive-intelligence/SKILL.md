---
name: competitive-intelligence
description: Comprehensive competitive intelligence for biopharma assets and indications. Analyzes competitive landscape, pipeline positioning, differentiation, and market dynamics. Use when users ask about competitive landscape, market positioning, pipeline comparison, or competitive threats.
user-invocable: false
---

# Competitive Intelligence

Perform comprehensive competitive intelligence analysis for biopharma assets and therapeutic areas using Gosset's data platform.

## Data Sources

### Gosset MCP Tools
- `find_drugs(query, limit, effort)` — Search drugs by name, target, indication, developer, modality, phase
- `find_companies(query, limit, effort)` — Search companies by name, therapeutic area, modality, location
- `find_deals(query, limit, effort)` — Search M&A, licensing, collaborations
- `find_trials(query, limit)` — Search clinical trials
- `gosset_agent(prompt)` — Deep research with specialized agents (Head2Head, Agentic Search, PTRS, Research Reporter)

### Open Targets Platform API (Free, no key)
```python
import requests

# Drug-disease associations for competitive landscape
r = requests.post("https://api.platform.opentargets.org/api/v4/graphql", json={
    "query": """query($efoId: String!, $index: Int!, $size: Int!) {
        disease(efoId: $efoId) {
            id name
            knownDrugs(size: $size) {
                rows { drug { id name } phase mechanismOfAction status target { approvedSymbol } }
            }
        }
    }""",
    "variables": {"efoId": "EFO_0003060", "index": 0, "size": 50}
})
```

### openFDA API (Free)
```python
# Approval data for competitive context
r = requests.get("https://api.fda.gov/drug/drugsfda.json", params={
    "search": 'openfda.pharm_class_epc:"Kinase Inhibitor"', "limit": 50
})
```

### Web Search
Supplement with web search for: recent news, conference data, analyst reports, press releases.

## Workflow

### Step 1: Define the Competitive Frame
Clarify with the user:
- **Asset or indication focus?** (single drug vs. therapeutic area)
- **Scope**: Which competitors, phases, modalities to include?
- **Time horizon**: Current landscape or forward-looking?

### Step 2: Gather Data

#### For Asset-Level CI:
```
find_drugs(query="[drug name or target] [indication]", limit=50, effort="high")
find_trials(query="Phase 2/3 [indication] [mechanism]", limit=30)
find_deals(query="[indication] licensing", limit=20)
```

#### For Indication-Level CI:
```
find_drugs(query="[indication]", limit=100, effort="high")
find_companies(query="[indication] developers", limit=50)
find_trials(query="[indication] Phase 3", limit=50)
```

### Step 3: Competitive Landscape Analysis

#### 3a. Pipeline Map
Organize assets by:
- Development phase (Preclinical → Phase 1 → Phase 2 → Phase 3 → Approved)
- Mechanism of action / target
- Developer

#### 3b. Differentiation Analysis
For each key competitor, assess:
- **Clinical differentiation**: Efficacy signals, safety profile, dosing convenience
- **Commercial differentiation**: Pricing, market access, label breadth
- **Development strategy**: Trial design choices, regulatory pathway, timeline

#### 3c. Deal Activity
- Recent licensing/M&A in the space
- Deal terms and valuations as benchmarks
- Partnership patterns

### Step 4: Synthesize

#### Report Structure (Inverted Pyramid):

**1. Executive Summary**
- 3-5 key competitive insights
- Positioning recommendation
- Critical risks and opportunities

**2. Competitive Landscape Overview**
- Pipeline map table (all assets by phase and mechanism)
- Market dynamics and trends

**3. Head-to-Head Comparisons**
Use `gosset_agent` for detailed drug comparisons:
```
gosset_agent(prompt="Compare [drug A] vs [drug B] vs [drug C] in [indication] — efficacy, safety, dosing, trial design")
```

**4. Deal Landscape**
- Recent transactions and valuations
- Implied market sizing from deal terms

**5. Forward-Looking Assessment**
- Expected catalysts and timeline
- Competitive risks
- White space opportunities

## Evidence Grading

| Grade | Description |
|-------|-------------|
| T1 | Approved label data, Phase 3 results |
| T2 | Phase 2 data, conference presentations |
| T3 | Phase 1 / preclinical, analyst estimates |
| T4 | Speculative, no public data |

Always note the evidence grade for key claims.

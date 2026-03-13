---
name: patent-landscape
description: Analyze patent landscapes for biopharma assets, targets, and therapeutic areas. Covers prior art, freedom-to-operate, patent expiry, IP positioning, and competitive IP mapping. Use when users ask about patents, IP landscape, freedom to operate, patent cliffs, or exclusivity.
user-invocable: false
---

# Patent Landscape Analysis

Analyze intellectual property landscapes for biopharma due diligence and competitive intelligence.

## Data Sources

### Gosset MCP Tools
- `find_drugs` — Get drug details including developers and modalities
- `find_deals` — Licensing deals often reflect IP value
- `find_companies` — Company IP portfolios
- `gosset_agent` — Deep research on IP strategy and patent details

### Web Search (Primary for patent data)
- Google Patents for patent search and full-text access
- USPTO, EPO, WIPO for official filings
- Orange Book (FDA) for listed patents and exclusivities
- Purple Book for biosimilar reference products
- Patent expiry databases

## When to Use

- Due diligence on a drug asset's IP position
- Freedom-to-operate analysis for a new program
- Patent cliff analysis and generic/biosimilar entry timing
- Competitive IP mapping in a therapeutic area
- Licensing deal IP assessment

## Workflow

### Step 1: Define Scope
- **Asset-specific**: Patents protecting a specific drug
- **Target-specific**: Patent landscape around a molecular target
- **Indication-specific**: IP landscape in a therapeutic area
- **Technology-specific**: Platform/modality patent landscape

### Step 2: Gather Patent Data

#### Asset IP Profile:
```
# Get drug details and developer
find_drugs(query="[drug name]", limit=5, effort="high")

# Research patent portfolio
gosset_agent(prompt="What are the key patents protecting [drug name]? Include composition of matter, formulation, method of use, and process patents. What are the expiry dates?")
```

#### Competitive IP Mapping:
```
# Find all drugs in the space
find_drugs(query="[target/indication]", limit=50, effort="high")

# Research IP landscape
gosset_agent(prompt="Map the patent landscape for [target] inhibitors. Who holds key composition of matter patents? What are the blocking patents?")
```

### Step 3: Analysis

#### Patent Categories to Track:
| Category | Description | Impact |
|----------|-------------|--------|
| Composition of matter | Core compound patents | Strongest protection |
| Formulation | Drug product patents | Extends lifecycle |
| Method of use | Indication-specific patents | Label protection |
| Process/manufacturing | Synthesis patents | Supply chain barrier |
| Combination | Fixed-dose combinations | Portfolio extension |
| Polymorph/salt | Crystal form patents | Lifecycle management |

#### Key Dates:
- Patent filing and grant dates
- Patent expiry dates (including PTEs, SPCs)
- Regulatory exclusivity expiry (NCE, orphan, pediatric)
- First possible generic/biosimilar entry
- Paragraph IV certification deadlines

### Step 4: Report

1. **Executive Summary** — Key IP risks and opportunities
2. **Patent Portfolio Map** — Table of patents by category, dates, status
3. **Freedom to Operate** — Blocking patents, design-around options
4. **Patent Cliff Timeline** — Expiry dates and exclusivity loss schedule
5. **Competitive IP Positioning** — Who holds what, white space
6. **Recommendations** — IP strategy implications for the deal/program

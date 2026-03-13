---
name: deal-analysis
description: Analyze biopharma deals (M&A, licensing, collaborations) for due diligence and competitive intelligence. Evaluates deal terms, comparables, valuations, and strategic rationale. Use when users ask about deal analysis, transaction comparables, licensing terms, M&A valuation, or deal flow.
user-invocable: false
---

# Deal Analysis

Analyze pharmaceutical transactions for due diligence, deal sourcing, and competitive intelligence.

## Data Sources

### Gosset MCP Tools
- `find_deals(query, limit, effort)` — Search M&A, licensing, collaborations by company, drug, indication, deal type, value
- `find_drugs(query, limit, effort)` — Get asset details for deal context
- `find_companies(query, limit, effort)` — Get company profiles for buyer/seller context
- `find_trials(query, limit)` — Clinical stage of deal assets
- `gosset_agent(prompt)` — Deep research on deal rationale and market context

### Web Search
- SEC filings (8-K, proxy statements) for deal terms
- Press releases for announced terms
- Analyst commentary on deal rationale

## Workflow

### Step 1: Gather Deal Data

#### Single Deal Analysis:
```
find_deals(query="[company A] [company B] [drug/indication]", limit=10)
find_drugs(query="[drug in deal]", limit=10, effort="high")
```

#### Deal Landscape:
```
find_deals(query="[indication] licensing 2024 2025", limit=50, effort="high")
find_deals(query="[target/modality] M&A", limit=30)
```

### Step 2: Deal Comparables

Build a comparables table:

| Date | Buyer | Seller | Asset | Phase | Indication | Deal Type | Upfront ($M) | Total ($M) | Royalties |
|------|-------|--------|-------|-------|------------|-----------|-------------|-----------|-----------|

#### Segment Comparables By:
- Indication/therapeutic area
- Development phase at deal
- Modality (small molecule, antibody, cell therapy, etc.)
- Deal type (licensing, M&A, collaboration)
- Geography (global vs. regional rights)

### Step 3: Valuation Analysis

#### Key Metrics:
- **Upfront / total deal value** — Benchmark against phase and indication
- **Upfront as % of total** — Risk sharing structure
- **Milestone structure** — Clinical, regulatory, commercial milestones
- **Royalty rates** — Tiered rates and comparisons
- **Implied peak sales** — Back-calculate from deal economics
- **Cost per phase transition** — Normalize by development stage

#### Phase-Based Benchmarks:
| Phase at Deal | Typical Upfront Range | Typical Total Range |
|---------------|----------------------|---------------------|
| Preclinical | $10-50M | $200M-1B |
| Phase 1 | $25-150M | $500M-2B |
| Phase 2 | $50-500M | $1-5B |
| Phase 3 | $200M-2B | $2-10B |
| Approved | $500M-5B+ | $2-20B+ |

### Step 4: Strategic Analysis

```
gosset_agent(prompt="What is the strategic rationale for [buyer] acquiring/licensing [asset] from [seller]? Consider pipeline fit, competitive positioning, and commercial synergies.")
```

- **Buyer rationale**: Pipeline gap-fill, therapeutic area entry, geographic expansion
- **Seller rationale**: Capital, development expertise, commercial reach
- **Competitive implications**: Market consolidation, competitive response

### Step 5: Report

1. **Executive Summary** — Key deal terms, strategic assessment
2. **Deal Terms** — Detailed breakdown of financial terms
3. **Comparables Analysis** — Table of comparable transactions
4. **Valuation Assessment** — Is the deal fairly priced? Premium/discount to comps?
5. **Strategic Fit** — How does this deal fit buyer/seller strategy?
6. **Market Impact** — Competitive and market implications

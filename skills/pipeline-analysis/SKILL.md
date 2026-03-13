---
name: pipeline-analysis
description: Analyze biopharma company pipelines for due diligence and competitive intelligence. Maps pipeline assets by phase, indication, target, and modality. Identifies pipeline gaps, concentration risks, and strategic positioning. Use when users ask about company pipelines, pipeline comparison, portfolio analysis, or R&D strategy assessment.
user-invocable: false
---

# Pipeline Analysis

Analyze pharmaceutical company pipelines for due diligence, competitive intelligence, and strategic assessment.

## Data Sources

### Gosset MCP Tools
- `find_drugs(query, limit, effort)` — Search assets by company, indication, target, phase
- `find_companies(query, limit, effort)` — Get company profiles and therapeutic focus
- `find_deals(query, limit, effort)` — Partnership and licensing activity
- `find_trials(query, limit)` — Active clinical programs
- `gosset_agent(prompt)` — Deep research, head-to-head comparisons, predictive trial scoring

### Web Search
- Company investor presentations and 10-K/10-Q filings
- Conference presentations and data readouts
- Analyst consensus estimates

## Workflow

### Step 1: Map the Pipeline

```
# Get all assets for a company
find_drugs(query="[company name]", limit=100, effort="high")

# Get company profile
find_companies(query="[company name]", limit=5)

# Get active clinical trials
find_trials(query="[company name] sponsored", limit=50)
```

### Step 2: Organize Pipeline

Create a pipeline table:

| Asset | Target | Modality | Indication(s) | Phase | Partner | Key Catalyst | Timeline |
|-------|--------|----------|---------------|-------|---------|-------------|----------|

### Step 3: Pipeline Analytics

#### Phase Distribution
- Count assets by phase (Preclinical, P1, P2, P3, Approved)
- Compare to peer companies

#### Therapeutic Concentration
- Revenue/asset concentration by indication
- Diversification assessment
- Key franchise dependency

#### Catalyst Calendar
- Near-term data readouts
- Regulatory milestones (PDUFA dates, EMA opinions)
- Expected phase transitions

#### Pipeline Risk Assessment
Use `gosset_agent` for trial success prediction:
```
gosset_agent(prompt="What is the probability of success for [drug] in Phase [X] for [indication]?")
```

### Step 4: Comparative Pipeline Analysis

```
# Compare pipelines of competitors
find_drugs(query="[company A]", limit=50, effort="high")
find_drugs(query="[company B]", limit=50, effort="high")

# Head-to-head asset comparisons
gosset_agent(prompt="Compare [company A] vs [company B] pipeline in [therapeutic area]")
```

### Step 5: Deal Activity

```
find_deals(query="[company name]", limit=30, effort="medium")
```
- Recent in-licensing (pipeline additions)
- Out-licensing (non-core divestitures)
- M&A activity and strategic direction

## Output Format

### Pipeline Analysis Report: [Company]

1. **Executive Summary**
   - Pipeline size and phase distribution
   - Key value drivers (top 3-5 assets)
   - Critical near-term catalysts
   - Strategic assessment

2. **Pipeline Map**
   - Full asset table organized by therapeutic area
   - Phase distribution chart

3. **Key Asset Deep Dives**
   - Top assets with competitive positioning
   - Trial data and differentiation
   - Commercial potential

4. **Catalyst Calendar**
   - Timeline of expected milestones

5. **Deal Activity**
   - Recent transactions and strategic implications

6. **Risk Assessment**
   - Concentration risks
   - Binary clinical events
   - Competitive threats
   - Patent cliff exposure

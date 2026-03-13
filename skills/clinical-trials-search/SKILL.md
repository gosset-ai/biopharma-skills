---
name: clinical-trials-search
description: Search and analyze clinical trials for competitive intelligence. Find trials by drug, target, indication, sponsor, phase, or status. Use when users ask about clinical trials, enrollment, trial design, or competitive trial landscapes.
user-invocable: false
---

# Clinical Trials Search

Search clinical trials using Gosset's curated trial database and supplement with web search for the latest data.

## Data Sources

### Primary: Gosset MCP
Use the `find_trials` MCP tool for structured trial data:
```
find_trials(query="Phase 3 NSCLC PD-1 inhibitors", limit=25)
```

Returns structured data: nct_id, title, status, phase, sponsors, drugs, targets, modalities, diseases, eligibility, endpoints, dates.

### Secondary: Gosset Agent
For deeper research or when structured search is insufficient:
```
gosset_agent(prompt="Find all Phase 3 trials comparing durvalumab vs pembrolizumab in first-line NSCLC")
```

### ClinicalTrials.gov API (Free, no key)
For detailed trial data not in Gosset, query ClinicalTrials.gov directly:
```python
import requests

# Search trials
r = requests.get("https://clinicaltrials.gov/api/v2/studies", params={
    "query.term": "pembrolizumab NSCLC Phase 3",
    "filter.overallStatus": "RECRUITING,ACTIVE_NOT_RECRUITING,COMPLETED",
    "pageSize": 50,
    "fields": "NCTId,BriefTitle,OverallStatus,Phase,StartDate,CompletionDate,EnrollmentCount,LeadSponsorName,Condition,InterventionName"
})

# Get single study by NCT ID
r = requests.get("https://clinicaltrials.gov/api/v2/studies/NCT04000165")

# Study statistics / enrollment
r = requests.get("https://clinicaltrials.gov/api/v2/stats/size")
```

### Supplementary: Web Search
Use web search to fill gaps — recent trial readouts, press releases, conference presentations.

## When to Use

- Finding ongoing/completed trials for a drug, target, or indication
- Competitive trial landscape mapping
- Enrollment and recruitment analysis
- Comparing trial designs across competitors
- Identifying gaps in the clinical landscape
- Tracking trial status changes and readouts

## Workflow

### 1. Search Gosset Trial Database
```
find_trials(query="<user query>", limit=25)
```

### 2. Enrich with Context
For each relevant trial, gather additional context:
- Use `find_drugs` to get drug/asset details
- Use `find_companies` to get sponsor information
- Use web search for recent press releases or data readouts

### 3. Analyze and Present
Structure findings as:

| NCT ID | Title | Phase | Status | Sponsor | Drug | Primary Endpoint | Est. Completion |
|--------|-------|-------|--------|---------|------|-----------------|-----------------|

### 4. Competitive Insights
- Identify trial design trends (endpoints, patient populations, comparators)
- Flag enrollment competition risks
- Note differentiation opportunities

## Output Format

Present results as a structured table with key trial attributes, followed by competitive insights and implications.

## Common Queries

- "Phase 3 trials for [indication]"
- "Trials comparing [drug A] vs [drug B]"
- "[Company] clinical pipeline"
- "Recruiting trials for [target] inhibitors"
- "Trial readouts expected in [timeframe]"

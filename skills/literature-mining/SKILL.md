---
name: literature-mining
description: Systematic literature mining and evidence synthesis for biopharma research. Automates literature search, screening, data extraction, and synthesis. Use when users ask for literature reviews, evidence synthesis, publication analysis, or systematic searches on a biomedical topic.
user-invocable: false
---

# Literature Mining

Automated literature search, screening, and evidence synthesis for biopharma intelligence.

## Data Sources

### Gosset MCP Tools
- `gosset_agent(prompt)` — Deep research with literature synthesis capabilities
- `find_drugs(query)` — Context on drugs and mechanisms mentioned in literature
- `find_trials(query)` — Link publications to clinical trials

### PubMed / Entrez (Free, via BioPython)
```python
from Bio import Entrez
Entrez.email = "your@email.com"

# Search PubMed
handle = Entrez.esearch(db="pubmed", term="CAR-T solid tumors", retmax=100, sort="date")
record = Entrez.read(handle)
id_list = record["IdList"]

# Fetch article details
handle = Entrez.efetch(db="pubmed", id=id_list, rettype="xml")
records = Entrez.read(handle)

for article in records['PubmedArticle']:
    medline = article['MedlineCitation']
    title = medline['Article']['ArticleTitle']
    # Extract: authors, journal, year, abstract, MeSH terms, etc.
```

### Europe PMC (Free, no key)
```python
import requests

# Full-text search with citation counts
r = requests.get("https://www.ebi.ac.uk/europepmc/webservices/rest/search", params={
    "query": "CAR-T solid tumors",
    "resultType": "core",
    "pageSize": 25,
    "sort": "CITED desc"
})
```

### Web Search (supplementary)
- bioRxiv / medRxiv for preprints
- Google Scholar for citation analysis
- Conference abstract databases (ASCO, AACR, ASH, ESMO, etc.)

## When to Use

- Systematic literature review on a therapeutic topic
- Evidence synthesis for due diligence
- Publication trend analysis (emerging targets, mechanisms)
- KOL identification through authorship analysis
- Competitive publication monitoring

## Workflow

### Step 1: Define Search Strategy

Clarify with the user:
- **PICO framework**: Population, Intervention, Comparator, Outcomes
- **Scope**: Therapeutic area, mechanism, time range
- **Depth**: Quick scan (10-20 papers) vs. systematic review (comprehensive)

### Step 2: Execute Search

#### Quick Literature Scan:
```
gosset_agent(prompt="Find the 10 most important recent publications on [topic]. Focus on clinical data, mechanism of action studies, and review articles. Provide title, authors, journal, year, and key findings for each.")
```

#### Systematic Search:
```
gosset_agent(prompt="Perform a systematic literature search on [topic] using PICO: P=[population], I=[intervention], C=[comparator], O=[outcomes]. Search PubMed, identify relevant studies, and extract key data.")
```

### Step 3: Data Extraction

For each relevant publication, extract:

| Field | Description |
|-------|-------------|
| Citation | Authors, title, journal, year |
| Study type | RCT, observational, meta-analysis, review |
| Population | N, demographics, disease stage |
| Intervention | Drug, dose, schedule |
| Comparator | Placebo, active comparator, SOC |
| Key endpoints | Primary and secondary endpoints |
| Key results | Effect sizes, p-values, confidence intervals |
| Safety | AE rates, SAEs, discontinuations |
| Conclusions | Author conclusions and implications |

### Step 4: Evidence Synthesis

#### Publication Landscape:
- Volume of publications over time (growing/declining interest)
- Key journals and conferences
- Authorship network and KOL identification

#### Evidence Quality:
- Study design hierarchy (meta-analysis > RCT > observational > case report)
- Sample sizes and statistical rigor
- Consistency across studies
- Risk of bias assessment

#### Key Findings Synthesis:
- Convergent evidence across studies
- Contradictory findings and potential explanations
- Evidence gaps and unanswered questions

### Step 5: Report

1. **Executive Summary** — Key findings in 3-5 bullet points
2. **Search Strategy** — Terms, databases, inclusion/exclusion criteria
3. **Evidence Map** — Table of included studies with extracted data
4. **Synthesis** — Integrated findings by theme
5. **Evidence Gaps** — What remains unknown
6. **Implications** — Relevance for the user's specific question/decision

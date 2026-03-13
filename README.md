<div align="center">

# Biopharma Intelligence Skills

[![Skills](https://img.shields.io/badge/Skills-10-blue?style=flat-square)](https://github.com/gosset-ai/biopharma-skills/tree/main/skills)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)](LICENSE)
[![GitHub Stars](https://img.shields.io/github/stars/gosset-ai/biopharma-skills?style=flat-square&color=gold)](https://github.com/gosset-ai/biopharma-skills/stargazers)

**Skills for biopharma market analysis — due diligence, competitive intelligence, and deal evaluation.**

*10 curated skills · DD · CI · Deals · Trials · Regulatory · IP · Literature*

</div>

---

## Skills

| Skill | Use Case |
|-------|----------|
| **competitive-intelligence** | Competitive landscape analysis, pipeline positioning, market dynamics |
| **pipeline-analysis** | Company pipeline mapping, phase distribution, catalyst tracking |
| **deal-analysis** | M&A, licensing, collaboration analysis with transaction comparables |
| **clinical-trials-search** | Clinical trial search and competitive trial landscape mapping |
| **clinical-trial-design** | Trial design feasibility assessment, enrollment projections, PTRS |
| **drug-target-validation** | Multi-dimensional target validation with GO/NO-GO scoring (0-100) |
| **drug-repurposing** | Repurposing opportunity identification and evaluation |
| **patent-landscape** | IP landscape analysis, FTO, patent cliff timing |
| **regulatory-intelligence** | FDA/EMA regulatory tracking, pathway analysis, PDUFA dates |
| **literature-mining** | Systematic literature search, evidence synthesis, KOL identification |

## Data Layer

### Open-source APIs

Skills integrate free public APIs out of the box — no API keys required:

- **openFDA** — Adverse events (FAERS), drug labeling, approvals, recalls, 510(k), PMA
- **Open Targets Platform** — Target-disease associations, tractability, safety, known drugs (GraphQL)
- **ClinicalTrials.gov v2 API** — Trial protocols, enrollment, status, endpoints
- **ChEMBL** — Bioactive compounds, target activities, drug mechanisms
- **PubMed / Entrez** — Literature search via BioPython
- **Europe PMC** — Full-text search with citation data
- **UniProt** — Protein function, structure cross-refs
- **GWAS Catalog** — Genetic associations
- **gnomAD** — Genetic constraint / loss-of-function intolerance

Supplemented by web search for recent news, patent filings, conference presentations, and SEC filings.

### Enhanced with Gosset

With access to **[Gosset](https://gosset.ai)**, skills are supercharged with curated biopharma competitive intelligence — covering 100K+ drugs, 100K+ companies, and thousands of deals and clinical trials, all searchable via natural language through MCP:

- `find_drugs` — Drugs, compounds, therapeutic assets
- `find_companies` — Biotech and pharma companies
- `find_deals` — M&A, licensing, collaborations
- `find_trials` — Clinical trials
- `gosset_agent` — Deep research agents (Head2Head, Agentic Search, PTRS, Research Reporter)

## Acknowledgments

Some skills adapted from [OpenClaw Medical Skills](https://github.com/FreedomIntelligence/OpenClaw-Medical-Skills) (MIT).

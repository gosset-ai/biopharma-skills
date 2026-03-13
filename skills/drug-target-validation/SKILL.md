---
name: drug-target-validation
description: Comprehensive computational validation of drug targets for due diligence. Evaluates targets across multiple dimensions — disease association, druggability, clinical precedent, safety, competitive landscape. Produces a quantitative Target Validation Score with GO/NO-GO recommendation. Use when users ask about target validation, druggability, or "is X a good drug target for Y?"
user-invocable: false
---

# Drug Target Validation

Validate drug target hypotheses using Gosset data and web research. Produces a quantitative Target Validation Score (0-100) with GO/NO-GO recommendation.

## Data Sources

### Gosset MCP Tools
- `find_drugs(query, limit, effort)` — Find drugs targeting this target (clinical precedent)
- `find_companies(query, limit)` — Find companies pursuing this target
- `find_trials(query, limit)` — Find clinical trials for this target
- `find_deals(query, limit)` — Find deal activity around this target
- `gosset_agent(prompt)` — Deep research on target biology, safety, landscape

### Open Targets Platform API (Free, no key)
```python
import requests

# Target-disease associations
r = requests.post("https://api.platform.opentargets.org/api/v4/graphql", json={
    "query": """query($ensemblId: String!) {
        target(ensemblId: $ensemblId) {
            id
            approvedSymbol
            approvedName
            tractability { label modality value }
            safetyLiabilities { event biosample direction studies { name } }
        }
    }""",
    "variables": {"ensemblId": "ENSG00000169083"}
})

# Disease associations for a target
r = requests.post("https://api.platform.opentargets.org/api/v4/graphql", json={
    "query": """query($ensemblId: String!, $index: Int!, $size: Int!) {
        target(ensemblId: $ensemblId) {
            associatedDiseases(page: {index: $index, size: $size}) {
                rows { disease { id name } score datatypeScores { id score } }
            }
        }
    }""",
    "variables": {"ensemblId": "ENSG00000169083", "index": 0, "size": 25}
})

# Known drugs for a target
r = requests.post("https://api.platform.opentargets.org/api/v4/graphql", json={
    "query": """query($ensemblId: String!, $size: Int!) {
        target(ensemblId: $ensemblId) {
            knownDrugs(size: $size) {
                rows { drug { id name } phase mechanismOfAction status }
            }
        }
    }""",
    "variables": {"ensemblId": "ENSG00000169083", "size": 25}
})
```

### ChEMBL API (Free, no key)
```python
# Search targets
r = requests.get("https://www.ebi.ac.uk/chembl/api/data/target/search.json", params={
    "q": "EGFR", "limit": 5
})

# Get bioactivities for a target
r = requests.get("https://www.ebi.ac.uk/chembl/api/data/activity.json", params={
    "target_chembl_id": "CHEMBL203", "limit": 100
})
```

### UniProt API (Free, no key)
```python
# Protein function, subcellular location, structure cross-refs
r = requests.get("https://rest.uniprot.org/uniprotkb/P00533.json")
```

### PubMed / Entrez (Free, via BioPython)
```python
from Bio import Entrez
Entrez.email = "your@email.com"
handle = Entrez.esearch(db="pubmed", term="EGFR AND lung cancer AND target validation", retmax=50)
```

### openFDA API (Free)
```python
# Adverse events for drugs hitting this target
r = requests.get("https://api.fda.gov/drug/event.json", params={
    "search": "patient.drug.medicinalproduct:erlotinib", "limit": 10
})

# Drug labeling (warnings, boxed warnings)
r = requests.get("https://api.fda.gov/drug/label.json", params={
    "search": 'openfda.generic_name:"erlotinib"', "limit": 1
})
```

### gnomAD (Free, via web search)
- Genetic constraint scores (pLI, LOEUF) — indicates target essentiality
- Loss-of-function intolerance as safety signal

### GWAS Catalog (Free API)
```python
r = requests.get("https://www.ebi.ac.uk/gwas/rest/api/singleNucleotidePolymorphisms/search/findByGene", params={
    "geneName": "EGFR"
})
```

## Validation Dimensions (10)

### 1. Target Disambiguation
Resolve gene symbol, protein name, UniProt ID. Confirm the exact molecular target.

### 2. Disease Association (0-10)
```
gosset_agent(prompt="What is the genetic and biological evidence linking [target] to [disease]?")
```
- Genetic associations (GWAS, Mendelian)
- Expression data (disease vs. normal)
- Animal model evidence

### 3. Druggability Assessment (0-10)
- Target class (kinase, GPCR, ion channel, etc.)
- Structural information (crystal structures, binding pockets)
- Small molecule vs. biologic tractability

### 4. Clinical Precedent (0-10)
```
find_drugs(query="[target] inhibitor", limit=50, effort="high")
find_trials(query="[target] [disease]", limit=30)
```
- Approved drugs hitting this target
- Clinical-stage assets and their results
- Failed programs and reasons for failure

### 5. Competitive Landscape (0-10)
```
find_drugs(query="[target]", limit=100, effort="high")
find_companies(query="[target] developers", limit=30)
```
- Number and phase distribution of competitors
- Differentiation opportunities
- Freedom to operate

### 6. Safety Assessment (0-10)
- Known on-target toxicities
- Expression in normal tissues
- Class-effect safety signals from clinical data
- Knockout phenotype data

### 7. Deal Activity (0-10)
```
find_deals(query="[target] licensing", limit=20)
```
- Recent deals validating the target
- Deal values as market signal
- Partnership patterns

### 8. Literature Evidence (0-10)
- Publication volume and trajectory
- Key opinion leader activity
- Conference presentation trends

### 9. Pathway Context (0-10)
- Pathway position and redundancy risk
- Biomarker availability for patient selection
- Combination rationale

### 10. Commercial Potential (0-10)
- Patient population size
- Current standard of care limitations
- Unmet medical need severity
- Pricing/reimbursement precedent

## Scoring

**Target Validation Score** = weighted sum across dimensions:

| Dimension | Weight |
|-----------|--------|
| Disease Association | 15% |
| Druggability | 10% |
| Clinical Precedent | 15% |
| Competitive Landscape | 10% |
| Safety | 15% |
| Deal Activity | 5% |
| Literature Evidence | 5% |
| Pathway Context | 10% |
| Commercial Potential | 15% |

### GO/NO-GO Thresholds
- **75-100**: Strong GO — compelling target with strong validation
- **50-74**: Conditional GO — promising but gaps exist
- **25-49**: Caution — significant risks, needs more validation
- **0-24**: NO-GO — insufficient evidence or critical red flags

## Output Format

### Target Validation Report: [Target] for [Disease]

1. **Executive Summary** — Score, recommendation, key findings
2. **Dimension Scores** — Table with score, evidence grade (T1-T4), and rationale per dimension
3. **Competitive Landscape** — Pipeline map of assets targeting this target
4. **Risk Assessment** — Critical risks and mitigation strategies
5. **Recommendation** — GO/NO-GO with conditions and next steps

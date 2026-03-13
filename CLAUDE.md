# CLAUDE.md

## Project

This repo contains Claude Code skills for biopharma market analysis (DD, CI, deal evaluation). Skills are in `skills/*/SKILL.md`.

## Data Layer

All skills use Gosset (gosset.ai) MCP tools as the primary data source:
- `find_drugs`, `find_companies`, `find_deals`, `find_trials` for structured search
- `gosset_agent` for deep research (routes to specialized agents: Head2Head, Agentic Search, PTRS, Research Reporter)
- `fetch_agent_result` for async results

Supplement with web search when Gosset data is insufficient (recent news, patent filings, regulatory documents, SEC filings).

## Conventions

- Skills follow inverted pyramid structure: executive summary first, then details
- Evidence grading: T1 (approved/Phase 3) → T2 (Phase 2/conference) → T3 (Phase 1/preclinical) → T4 (speculative)
- Quantitative scoring (0-100) where applicable with explicit weights and thresholds
- Always note data limitations and evidence grades for key claims

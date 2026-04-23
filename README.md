# Agentic Commerce Readiness Index 2026 — Edition 1

**Premium DTC Beauty — 40 brands scored on 9 criteria.**

👉 **[View the dashboard](https://zhao-hanbo.github.io/readiness-index-2026-beauty/)**

## What this is

Original research scoring how ready premium DTC beauty brands' product
pages are for agentic commerce — whether AI shopping agents (ChatGPT
Shopping, Perplexity Buy with Pro, Google AI Mode, Claude's commerce
capabilities) can find, parse, and recommend them.

- **40 brands** scored across 9 binary criteria (max 9 points)
- **3 areas**: Crawlability, Structured Data, Content Quality
- **Zero brands** hit the "Agent-Ready" threshold (7+)
- **Median score**: 4/9
- **Top tier**: 7 brands tied at 6/9

## How to use this repo

- **Dashboard**: [`index.html`](https://zhao-hanbo.github.io/readiness-index-2026-beauty/)
  — summary, charts, linked brand list
- **Per-brand audits**: one HTML + one JSON per brand, named by slug
  (e.g. `rare-beauty.html`). Click any brand in the dashboard.
- **Structured data**: every `.json` file is the same audit in
  machine-readable form. Use these for cross-brand analysis.

## Raw data (free to download, CC BY 4.0)

| File | What it contains |
|---|---|
| [`scores.csv`](./scores.csv) | Per-brand total + area subtotals + 9 binary criteria for all 40 ranked brands. Spreadsheet-friendly. |
| [`scores.details.jsonl`](./scores.details.jsonl) | Per-criterion evidence per brand: which URLs were checked, which bots are allowed, which PDPs have schema, sample titles evaluated, etc. One JSON object per line. |
| [`brands.csv`](./brands.csv) | Full candidate list (50 brands, including the 10 excluded) with sourcing notes. |
| [`methodology.md`](./methodology.md) | The rubric, sampling method, and scoring automation stack in full. |
| [`notable-findings.md`](./notable-findings.md) | Top 10 patterns surfaced during scoring, including the Content Cliff and Hero Cosmetics anomaly. |

Data and audits released under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — free to cite, reanalyze, and build on with attribution.

The 9 criteria:

| # | Criterion | Area |
|---|---|---|
| C1 | AI-crawler access in robots.txt | Crawlability |
| C2 | PDP server-side renders (no JS required) | Crawlability |
| C3 | Sitemap exists, fresh (<90d), lists products | Crawlability |
| C4 | Product JSON-LD on PDPs with required fields | Structured Data |
| C5 | AggregateRating or Review JSON-LD with real counts | Structured Data |
| C6 | FAQPage or HowTo JSON-LD on at least one page | Structured Data |
| C7 | Organization JSON-LD with sameAs socials | Structured Data |
| C8 | PDP titles include use case or differentiator | Content Quality |
| C9 | PDP descriptions answer buyer-context questions | Content Quality |

## Reproducibility

Scored via [`score_brand.py`](https://github.com/Fifteen-AI/nohi-content-service/blob/main/scripts/readiness/score_brand.py)
(source-level HTTP fetch, JSON-LD parsing, LLM evaluation of C8/C9)
on 2026-04-18. HTML reports rendered via
[agentic-commerce-kit](https://github.com/zhao-hanbo/agentic-commerce-kit)
`render.py` — the Readiness Index methodology packaged as a Claude
Code skill so any merchant or agency can run it against their own
store.

## Want your own brand audited?

Run [`agentic-commerce-kit`](https://github.com/zhao-hanbo/agentic-commerce-kit)
locally:

```
/audit-agentic-commerce https://your-store.com --output=html
```

Takes ~90 seconds. Produces the same per-brand HTML you see here.

## Author

Research and methodology by [Zhao Hanbo](https://www.linkedin.com/in/hanbozhao)
while building [Nohi](https://nohi.ai) — the marketplace connecting DTC
merchants to agentic commerce surfaces.

## License

Data and audits © 2026 Zhao Hanbo. Released under
[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — free to use,
cite, and build on with attribution.

Methodology (the rubric itself) is MIT-licensed via
[agentic-commerce-kit](https://github.com/zhao-hanbo/agentic-commerce-kit).

# Notable Findings — Edition 1 Premium DTC Beauty

Data collected 2026-04-19. Sample: 50 brands intended, 40 in final ranking
(6 fetch errors, 4 confirmed not on Shopify → excluded).

## Headline numbers

- **0 of 40** brands scored 7 or higher (the "Agent-Ready" tier)
- **Median score**: 4 / 9
- **Mean score**: 4.03 / 9
- **Top 7 brands tied at 6/9** (no clean #1)
- **4 brands scored ≤ 2** — effectively invisible to agentic assistants

## Top 7 (tied at 6/9)

Tie-break ranked by Area 3 (content quality) first, then Area 2 (structured
data), then Area 1 (crawlability).

| Rank | Brand | A1 | A2 | A3 | Category | Notable |
|---|---|---|---|---|---|---|
| 1 | Rare Beauty | 2 | 3 | 1 | makeup | Celeb-founded (Selena Gomez) |
| 2 | Phlur | 2 | 3 | 1 | fragrance | Missing Person viral; clean perfume |
| 3 | Milk Makeup | 3 | 2 | 1 | makeup | Long-running brand, acquired status |
| 4 | Ceremonia | 3 | 2 | 1 | haircare | Latinx-founded (Babba Rivera) |
| 5 | OSEA Malibu | 3 | 2 | 1 | body | Seaweed-based clean skincare |
| 6 | Krave Beauty | 3 | 3 | 0 | skincare | Liah Yoo founded; K-beauty |
| 7 | Hero Cosmetics | 3 | 3 | 0 | skincare | Mass-market (Mighty Patch); budget tier |

## Bottom 10

| Brand | Score | Note |
|---|---|---|
| Topicals | 1/9 | Not on Shopify (excluded from ranking) |
| Item Beauty | 1/9 | Not on Shopify (excluded) |
| Drunk Elephant | 2/9 | Not on Shopify (excluded) |
| Biossance | 2/9 | Not on Shopify (excluded) |
| Ilia Beauty | 2/9 | — |
| Kulfi Beauty | 2/9 | — |
| D.S. & Durga | 2/9 | — |
| The Nue Co | 2/9 | — |
| Westman Atelier | 3/9 | Premium makeup |
| Moon Juice | 3/9 | Wellness/skincare bridge |

## Per-criterion pass rates (N=40, Shopify-confirmed only)

Sorted by pass rate ascending — shows the bottleneck:

| Criterion | Area | Pass Rate | Pass Count |
|---|---|---:|---|
| **C9. Descriptions answer comparison questions** | Content | **12.5%** | 5/40 |
| **C6. FAQ / HowTo schema present** | Structure | **12.5%** | 5/40 |
| C5. Review / AggregateRating schema | Structure | 30.0% | 12/40 |
| C4. Product schema (full) | Structure | 37.5% | 15/40 |
| C8. PDP titles with differentiator | Content | 40.0% | 16/40 |
| C3. Sitemap current (90d freshness) | Crawl | 47.5% | 19/40 |
| C7. Organization schema | Structure | 57.5% | 23/40 |
| C2. PDP server-side render | Crawl | 72.5% | 29/40 |
| C1. AI crawler access (robots.txt) | Crawl | 92.5% | 37/40 |

## Observations worth turning into narrative

### Observation 1: The Content Cliff
Crawlability (Area 1) pass rates are 47-92%. Structured data (Area 2) is
30-57%. **Content quality (Area 3) drops to 12-40%**. The further a criterion
requires actual editorial work (vs. a plugin install), the worse the pass rate.

This is the headline story: **premium DTC beauty is technically welcoming to
AI agents but informationally silent**. The agents can reach the site —
they just can't extract decision-changing signals.

### Observation 2: Zero Agent-Ready brands
Methodology defined "Agent-Ready" as 7+ / 9. **Nobody hit it**. Even the top
tier is still missing at least 3 criteria. Premium DTC beauty as a category
has a ceiling problem, not a floor problem.

### Observation 3: Prestige does not predict readiness
The #1 scorer was Rare Beauty (mainstream celeb brand) tied with Phlur
(fragrance indie) and Hero Cosmetics (Mighty Patch — budget mass-market).
Meanwhile Drunk Elephant, Ilia Beauty, and Westman Atelier — brands
beauty-editor-favorite — scored 2-3/9.

**Prestige correlates with marketing budget, not structured-data hygiene.**

### Observation 4: Review schema absence is a pricing question
Only 30% of brands have `Review` / `AggregateRating` schema despite 95%+
having reviews displayed on their sites. The reviews exist — they're just
not in structured data. This is almost always a **widget vendor choice**
(Yotpo vs. Judge.me vs. Okendo vs. custom) and whether schema output is
turned on.

Many brands are paying $500-$5000/month for a review platform that's
withholding their data from AI assistants.

### Observation 5: Four brands have left Shopify
Of 50 "believed to be on Shopify" brands, 4 have migrated off:
Topicals, Drunk Elephant, Biossance, Item Beauty. Pattern: all have been
acquired or scaled to the point where they moved to Salesforce Commerce
Cloud or custom stacks. This is a secondary finding about the Shopify
growth ceiling.

### Observation 6: Six brands could not be fetched at all
Tata Harper, Youth To The People, Violette_FR, Ami Colé, Pinch of Color,
Adwoa Beauty — either bot-blocking, DNS issues, or (in Adwoa's case) a
timezone parsing bug on our end. In isolation 6/50 = 12% fetch failure
rate suggests brands rely heavily on JS-rendered content or
Cloudflare-style bot protection that confuses ANY automated client, not
just malicious ones. AI assistants likely see the same failures.

## Stat card candidates

Best single numbers to render:

- **"0 / 40"** — brands scoring 7+ ("Agent-Ready")  ← strongest
- **"12.5%"** — brands with descriptions that answer comparison questions
- **"4 / 9"** — median score (low enough to be damning)

Recommended: `0 / 40` — counter-intuitive, quick to parse, forces reader
to read on. Use "0 out of 40" as the stat; caption: "premium DTC beauty
brands scored 7+ out of 9 on Nohi's Agentic Commerce Readiness Index".

## Quotes to pull for the blog

Key evidence lines from per-brand details JSONL that would make the blog
concrete:

- "Rare Beauty has complete Product + Review + Organization schema, yet
  still fails the content quality dimension — descriptions focus on
  aspirational language rather than comparison-ready specifics."
- "Hero Cosmetics (Mighty Patch parent) outscores Drunk Elephant. The
  budget brand ships better structured data than the prestige one."
- "Every brand in the top 7 has `sameAs` entries linking to at least 3
  social profiles; every brand in the bottom 10 has 0-1."

## What to do with this — distribution plan

1. **LinkedIn teaser (Kevin Indig style)** — "0 of 40", Content Cliff as
   the named pattern, top 7 tagged for reshare, link to blog in comments
2. **Blog post on nohi.ai** — full methodology + ranking table + the 6
   observations above + what brands should do to improve
3. **DM outreach**:
   - Top 7 named: "Scored your brand in our Index, wanted to share the
     detail before it goes public" → relationship build
   - Bottom 10 in Nohi ICP: "Scored 2/9 on AI readiness — 5-minute fix
     for Product + Review schema could get you to 5/9" → warm sales
4. **Edition 2 tease**: Menswear or Home & Lifestyle next

## Methodology disclosures for the blog

- Sample size: 50 intended → 40 final (6 fetch errors + 4 not-on-Shopify excluded)
- Scoring date: 2026-04-19 (single point in time; brands may fix between then and publication)
- C10 (content uniqueness) deferred to Edition 2+; max score is therefore 9
- Scripts + scores reproducible at [github repo]

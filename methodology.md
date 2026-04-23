# Edition 1 — Premium DTC Beauty · Methodology

**Published**: TBD (target 2026-05)
**Sample size**: 50 brands
**Scoring period**: 2026-04-19 to 2026-05-03
**Maximum score**: 9 (C10 deferred to future editions)

## Scope

"Premium DTC Beauty" = independent beauty brands on Shopify with AOV
roughly $25-$150, primarily US market, direct-to-consumer as primary
channel (wholesale may exist but DTC is the main revenue line).

**In scope**: skincare, makeup, haircare, fragrance, body/bath, beauty
tools. Wellness and supplements are out of scope (future edition).

**Out of scope**:
- Drugstore / mass-market beauty (CeraVe, The Ordinary at mass pricing)
- Luxury beauty above $150 AOV (Augustinus Bader, La Mer, Chanel online)
- Brands without independent DTC sites (sold only through Sephora /
  Ulta / Amazon)
- Subscription-first DTC beauty boxes (Birchbox, Ipsy — business model
  is structurally different)
- Brands using Salesforce Commerce Cloud, BigCommerce, Magento, or custom
  commerce stacks (Shopify-only for this Index)

## Sampling method

Mixed-source sampling to reduce bias:

1. **Storeleads / BuiltWith top Shopify beauty by traffic rank** — 20 brands
2. **Shopify Editions featured / BFCM roundup mentions** — 10 brands
3. **Curated premium DTC catalog** (Sephora's "Clean at Sephora", Credo
   Beauty carried brands, Beauty Independent coverage) — 15 brands
4. **Manual adds for underserved segments** (Asian beauty on Shopify,
   Black-founded beauty brands) — 5 brands

Every brand must be verified on Shopify before scoring. URL checks via
BuiltWith or `curl -I` for Shopify headers.

Final brand list committed to `brands.csv` BEFORE scoring begins. No
brand added or removed after scoring starts (reproducibility).

## Scoring rubric (9 criteria in Edition 1)

All criteria binary (0 or 1). Total max = 9.

### Area 1 — Crawlability & access (3)

**C1. AI crawler access** (automated)
- Fetch `{domain}/robots.txt`
- Pass = robots.txt allows at least 4 of: GPTBot, PerplexityBot,
  ClaudeBot (formerly anthropic-ai), Google-Extended, Amazonbot,
  CCBot, FacebookBot
- Fail = blocks these OR no robots.txt file exists
- Script: `scripts/score_robots.py`

**C2. PDP server-side render** (automated)
- Fetch 3 random PDP URLs from sitemap with `curl -s` (no JS execution)
- Pass = 3/3 responses contain product title, price, and a product
  description in raw HTML (not placeholder / skeleton)
- Fail = any PDP requires JS to show core product data
- Script: `scripts/score_ssr.py`

**C3. Sitemap.xml current** (automated)
- Fetch `{domain}/sitemap.xml` (or sitemap index)
- Pass = sitemap exists, is valid XML, contains product URLs, and at
  least one `<lastmod>` timestamp within past 90 days
- Fail = missing, invalid, or stale
- Script: `scripts/score_sitemap.py`

### Area 2 — Structured data (4)

**C4. Product schema on PDPs** (automated)
- Check same 3 PDPs from C2
- Pass = 3/3 have valid `schema.org/Product` JSON-LD with:
  `name`, `image`, `brand`, `offers` (with `price` and `availability`),
  AND at least one of: `gtin`, `mpn`, `sku`
- Fail = missing on any of the 3 PDPs
- Script: `scripts/score_product_schema.py`

**C5. Review / AggregateRating schema** (automated)
- Check same 3 PDPs
- Pass = 3/3 have `Review` and/or `AggregateRating` schema with actual
  review counts (>0) and rating value
- Fail = missing or placeholder
- Script: `scripts/score_review_schema.py`

**C6. FAQPage / HowTo schema** (automated)
- Fetch homepage + up to 2 "about" / "faq" / "how to use" pages
- Pass = at least one page emits valid `FAQPage` or `HowTo` schema
- Fail = none
- Script: `scripts/score_faq_schema.py`

**C7. Organization schema** (automated)
- Fetch homepage
- Pass = valid `Organization` schema with at minimum: `name`, `url`,
  and `sameAs` (at least 2 social profiles)
- Fail = missing or incomplete
- Script: `scripts/score_org_schema.py`

### Area 3 — Content quality (2, C10 deferred)

**C8. PDP titles include use case / differentiator** (LLM evaluation)
- Sample 5 random PDP titles per brand from sitemap
- Prompt LLM: "Does this product title include a specific use case,
  target audience, or differentiator beyond the brand + product name?
  Examples of PASS: 'Retinol Alternative Night Serum for Sensitive
  Skin', 'Lightweight Tinted Moisturizer with SPF 40'. FAIL: 'Brand
  Name Face Cream'."
- Pass = 4+ of 5 titles pass
- Script: `scripts/score_pdp_titles.py` (uses `OpenAISettings`)

**C9. Descriptions answer comparison / context questions** (LLM evaluation)
- Same 5 PDPs, full description text
- Prompt LLM: "Does this product description explicitly address at least
  3 of: target skin type / hair type / use case, comparison to similar
  products, ingredient rationale, sizing/volume reasoning, when or how
  to use it, what it is NOT meant for?"
- Pass = 4+ of 5 pass
- Script: `scripts/score_pdp_descriptions.py`

**C10. Content uniqueness** — DEFERRED to Edition 2+
- Scored as N/A
- Rationale: required uniqueness tooling (Originality.ai, Copyscape) not
  yet integrated; adding introduces external API cost and legal
  complexity (some brands share content with authorized wholesalers
  legitimately). Revisit methodology Edition 3.

## Scoring automation stack

All criteria evaluated by Python scripts in `scripts/readiness/`. For
each brand:

```bash
python scripts/readiness/score_all.py --brand <brand-slug>
# Fetches URLs, runs all criteria, writes a row to scores.csv
```

Per-brand runtime: ~30 seconds for C1-C7, ~2 minutes for C8-C9 (LLM).
Total for 50 brands: ~2 hours wall-clock, mostly unattended.

LLM scoring uses `gpt-5.4-mini` with `reasoning_effort=low` for cost
control. Estimated cost: ~$5-10 total.

## Tie-breaking

Brands with the same total score rank by:
1. Higher Area 3 score (content quality)
2. Higher Area 2 score (structured data)
3. Alphabetical (for stability)

## What "Top Performer" means in Edition 1

- **Score 8-9**: "Agent-Ready" — merchants AI assistants can confidently
  recommend
- **Score 6-7**: "Partially Ready" — strong on some dimensions, gaps in
  others
- **Score 3-5**: "Early Stage" — basic ecommerce setup, AI optimization
  not yet a focus
- **Score 0-2**: "Not Ready" — missing even basic structured data hygiene

These labels appear in the blog post but NOT in the LinkedIn teaser —
teaser only names top performers (avoid public shaming at bottom).

## What we report publicly

- Full ranked list in the nohi.ai blog post (all 50)
- Per-brand score breakdown (9-criterion detail visible)
- Top 3-5 named in the LinkedIn teaser
- Median / distribution / "N brands scored 8+" as headline stats
- Methodology in full on the blog post

## What we do NOT report publicly

- Individual PDPs checked (so brands can't hyper-optimize a specific page)
- Exact LLM prompts used for C8-C9 (prevent gaming)

## Versioning

This methodology file is version 1.0. Every future edition's methodology
must either cite this version or increment (1.1, 2.0, etc.) with a
change log. Year-over-year comparisons only apply when methodology
version matches.

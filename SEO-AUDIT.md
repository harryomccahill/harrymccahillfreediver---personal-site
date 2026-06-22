# SEO / AEO / Shareability Audit — harrymccahill.com
_Audited & implemented 2026-06-22._

Target queries: **"Harry McCahill"**, **"freediving instructor"**, **"ranked freediver"**, plus
"freediving coach Dominica", "British freediving record holder", "learn freediving Dominica".

## Baseline (before this pass)
| Signal | Before |
|---|---|
| Meta descriptions | None on any page |
| Open Graph / Twitter cards | **0 tags** — shared links rendered bare (no image/title/description) |
| og:image (branded preview) | None |
| JSON-LD structured data | None — no entity defined for Google/AI |
| Canonicals | None |
| robots.txt / sitemap.xml | Both 404 |
| llms.txt (AI crawler file) | None |
| "freediving instructor" in content | 0 occurrences site-wide |
| Lighthouse (live homepage) | Performance **64**, SEO **91**, Best Practices **100** |
| Strengths | 1 H1/page, 100% image alt coverage, clean static HTML, HTTPS, custom domain |

## Implemented in this pass
- **Per-page `<head>`** on all 7 pages: keyword-optimised `<title>`, unique meta description, canonical, robots directive, full **Open Graph + Twitter card** set with a branded 1200×630 `og:image`.
- **OG images** generated from brand photography into `assets/og/` (home, coaching, records, media, sponsor, blog, default).
- **Structured data (JSON-LD):** `Person` + `WebSite` + `FAQPage` (8 Q&As) on home; `Service` + `Course` + `BreadcrumbList` on coaching; `BreadcrumbList` on records/media/sponsor/blog.
- **Light-touch keyword copy** in each page's hero eyebrow (e.g. home now reads "Freediving instructor · Ranked athlete · Dominica") — hero taglines untouched.
- **robots.txt** (allows Google + AI engines: GPTBot, ClaudeBot, PerplexityBot, Google-Extended, Applebot-Extended, CCBot) + **sitemap.xml** (6 indexable pages) + **llms.txt** (AI entity file).
- `brand-kit.html` set to `noindex` so it doesn't compete for brand queries.

## ⚠️ Action needed from Harry (factual — I did NOT guess these)
1. **Student count conflict:** homepage says "over 100" students; coaching says "250+". I published the **conservative "over 100"** in schema/llms.txt. Confirm the real number and align both pages.
2. **"Deepest" vs "second-deepest":** pages say both "deepest no-fins in Britain" and "second-deepest in British history". I used the reconciled framing **"current British record holder; second-deepest in British history"** (i.e. current #1, all-time #2). Confirm this is accurate.
3. **AEO parse risk:** records.html's charts are JS-injected; the headline numbers (PBs, world rank, records) DO also appear as static text, so crawlers see them — but keep it that way (don't move those into JS-only).

## Off-site / authority — to reach "best on the planet" (Harry's to drive)
On-site work makes the site strong and *eligible*; ranking #1 for broad terms also needs off-site authority + indexing time:
- **Google Search Console** — verify the domain, submit `sitemap.xml`, request indexing. (Highest priority — without this Google may be slow to see the changes.)
- **Bing Webmaster Tools** — submit sitemap (Bing's index feeds some AI answer engines).
- **Google Business Profile** — if offering in-person coaching in Dominica; wins local-intent "freediving instructor" searches.
- **Wikidata entity** for "Harry McCahill" — large lever for AI knowledge graphs / answer engines.
- **Backlinks & citations** — ensure AIDA athlete page, The Freedive Ranking, Blue Element site, and any press/podcasts link back with consistent naming.
- **Consistent NAP + sameAs** — same name/role/links everywhere (already wired into the Person schema).

## Realistic expectations
- **"Harry McCahill"** → realistically own #1 + a knowledge panel over time (Person schema + sameAs).
- **"freediving instructor" / "ranked freediver"** → broad, competitive, often local-intent. This pass maximises every on-site factor; global #1 depends on the off-site work above + competition + time.

## Separate opportunity (not in this pass)
Lighthouse **Performance 64** on the homepage — worth a follow-up pass (hero image/video sizing, font loading). Performance is a ranking factor but distinct from this SEO/AEO/shareability work.

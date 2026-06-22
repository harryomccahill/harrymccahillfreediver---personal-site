# Handoff: Live world-ranking card on the Records page

## Overview
Adds a **live world-ranking card** to the Records page (`records.html`), sourced from
Harry McCahill's official profile on **The Freedive Ranking**
(https://freediveranking.com/athlete/Harry-McCahill). It surfaces his current world
rank, the four ratified competition bests, a "last synced" date, and a deep link to the
live profile. It replaces the old plain-text "source note" at the foot of the
**Results timeline** (section III, `.section--light`).

## About the Design Files
`records.html` in this bundle is a **design reference** created in HTML — it shows the
intended look and the data wiring, not necessarily the final production code. Recreate it
in the live codebase using the site's existing patterns (it is currently static HTML/CSS
+ a small vanilla `<script>`, so it can be lifted closely).

## Fidelity
**High-fidelity.** Exact tokens, type, spacing and copy are specified below.

## Where it goes
- File: `records.html`
- Section: the third content section (`<section class="section section--light" data-screen-label="Results timeline">`).
- Position: at the **end** of that section, replacing the former `<p class="source-note">`.

## Component: `.rankcard`
A bordered dark card (sharp corners, hairline border — matches the existing `.pb` /
`.strip__cell` cards on this page; nothing on this page is rounded except pill buttons).

### Container
- `background: var(--deep)`; `color: var(--mist)`
- `border: 1px solid rgba(124,196,206,0.18)` (the page's standard teal hairline)
- `padding: clamp(26px,3vw,40px) clamp(24px,3vw,44px)`
- `margin-top: clamp(40px,5vw,64px)`

### Top row (`.rankcard__top`) — flex, space-between, wraps
- **Live badge** (`.rankcard__live`): inline-flex, `gap:8px`, label "Live ranking" in
  10.5px / `letter-spacing:0.24em` / uppercase / `color: var(--light)`. Leading **dot**:
  7×7px, `border-radius:999px`, `background:#46d39a`, pulsing box-shadow ring animation
  (`rankPulse`, 2.8s, `var(--ease)`). Honour `prefers-reduced-motion` (the existing
  page-wide reduced-motion rules cover reveals; the dot keyframe is decorative — fine to
  leave, or disable under reduced motion).
- **Source name** (`.rankcard__srcname`): "Synced from The Freedive Ranking · official
  athlete profile", 12.5px / weight 300 / `color: rgba(234,242,242,0.72)`.
- **CTA link** (`.rankcard__link`): "View live profile →", pill, 11px /
  `letter-spacing:0.22em` / uppercase / `color: var(--light)`,
  `border:1px solid color-mix(in oklab, var(--light) 42%, transparent)`,
  `border-radius:999px`, `padding:11px 22px`. Hover: `background: var(--light)`,
  `color: var(--deep)`, `translateY(-1px)`. Opens
  `https://freediveranking.com/athlete/Harry-McCahill` in a new tab (`rel="noopener"`).
- The top row has a `border-bottom: 1px solid rgba(124,196,206,0.16)` and
  `padding-bottom: clamp(20px,2.4vw,28px)`.

### Body (`.rankcard__body`) — 2-col grid `minmax(0,0.9fr) minmax(0,1.1fr)`, gap `clamp(28px,5vw,64px)`, align center
Collapses to one column below 760px.

**Left — the rank:**
- Eyebrow "World ranking · depth": 10.5px / `letter-spacing:0.26em` / uppercase /
  `rgba(234,242,242,0.6)`.
- Big number (`.rankcard__num`): `font-family: var(--serif)`,
  `font-size: clamp(48px,6.4vw,88px)`, `line-height:0.88`, `color: var(--mist)`. The
  "No." prefix (`<b>`) is `var(--sans)` weight 300, `0.34em`, uppercase,
  `letter-spacing:0.06em`, `color: var(--light)`, `margin-right:8px`.
  → renders **"No. 63"**.
- Meta line (`.rankcard__meta`): "United Kingdom · Depth focus · Competing since 2024"
  then a line break and "Last synced June 2026". 13px / weight 300 / line-height 1.7 /
  `rgba(234,242,242,0.66)`.

**Right — competition bests (`.rankcard__bests`):** a 4-col grid (2-col below 480px) of
cells, same hairline-gap treatment as `.pbs` (`gap:1px`, teal-tinted background showing
through as dividers, `border:1px solid rgba(124,196,206,0.16)`). Each cell (`.rbest`,
`background: var(--deep)`, `padding: clamp(14px,1.7vw,20px) clamp(12px,1.5vw,18px)`):
  - Discipline label (`.rbest__d`): 10px / `0.22em` / uppercase / `var(--light)`.
  - Value (`.rbest__n`): `var(--serif)`, `clamp(24px,2.5vw,34px)`, `var(--mist)`; the
    "m" unit (`<i>`) is `0.46em`, `var(--light)`.
  - The four cells, in order: **CWT 91m · FIM 90m · CWTB 85m · CNF 80m**.

### Footer line (`.rankcard__foot`)
"Figures reflect official, judged competition results as ratified on **The Freedive
Ranking** and **AIDA International**. World ranking position is live and changes through
the season." 12px / weight 300 / `rgba(234,242,242,0.5)`. The two source names are links
(`var(--light)`, hairline underline) to the profile and to https://www.aidainternational.org/.

## Data wiring (the important part)
The card is driven by a single `RANKING` object in the page's `<script>` and rendered by
a small `renderRanking()` function into `#rank-value`, `#rank-synced`, and `#rank-bests`:

```js
const RANKING = {
  world: 63,
  synced: 'June 2026',
  bests: [ {d:'CWT',n:91}, {d:'FIM',n:90}, {d:'CWTB',n:85}, {d:'CNF',n:80} ],
  profile: 'https://freediveranking.com/athlete/Harry-McCahill'
};
```

**To make it auto-update (recommended next step for the dev):** replace the static object
with a **build-time or serverless fetch** of the athlete profile. The profile page exposes
the rank and all four comp bests in its Open Graph `<meta>` tags
(`og:title` → "World no.63"; `meta-description` → "FIM: 90m, CNF: 80m, CWTB: 85m,
CWT: 91m"), which is a stable, cheap source to parse server-side. **A browser-side fetch
is blocked by CORS**, so do it at build time (e.g. a scheduled build / cron) or via a tiny
serverless proxy, and keep the static values above as the offline fallback. The render
function already reads from the object, so only the data source needs swapping.

## Interactions & Behavior
- The whole card uses the existing `.reveal` scroll-in animation (fade + 26px rise).
- Live dot pulses (2.8s loop). No other motion.
- CTA + footer links open in new tabs.

## Design Tokens (all already defined in `assets/css/brand.css`)
- `--deep`, `--mist`, `--light`, `--tide`, `--abyss`, `--slate`, `--hair`
- Hairline: `rgba(124,196,206,0.18)` / `0.16`
- Fonts: `--serif` (display), `--sans` (body/labels)
- Easing: `--ease`
- New literal added: live-dot green `#46d39a`.

## Assets
None added. (No remote avatar is hot-linked, to avoid CORS/hotlink fragility.)

## Files
- `records.html` — contains the full implementation: the `.rankcard*` CSS block (inserted
  just before the `CTA` styles), the markup at the end of the Results timeline section,
  and the `RANKING` object + `renderRanking()` at the top of the page `<script>`.

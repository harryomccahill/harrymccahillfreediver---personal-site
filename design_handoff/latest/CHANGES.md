# Changes — Live world-ranking card on the Records page

**What:** Added a live world-ranking card to `records.html`, sourced from Harry's
official profile on The Freedive Ranking. It shows his world rank (No. 63), the four
ratified competition bests (CWT 91m · FIM 90m · CWTB 85m · CNF 80m), a "last synced"
date, and a deep link to the live profile. It replaces the old plain-text source note at
the bottom of the Results timeline section.

**Why:** The user asked to integrate ranking/standings data from freediveranking.com on
the Records page. This turns a static throwaway line into a proper, attributed,
update-friendly ranking module.

**Scope:** `records.html` only — one new CSS block, one markup block (replacing the old
`.source-note` paragraph), and a `RANKING` data object + render function in the existing
`<script>`. No other pages touched.

**Follow-up for the dev (optional but recommended):** wire the `RANKING` object to a
build-time/serverless fetch of the profile's Open Graph meta tags so the rank and bests
auto-update. Browser-side fetch is CORS-blocked — do it server-side; keep the static
values as fallback. Full instructions in README.md → "Data wiring".

**Acceptance:** Records page renders the card reading "No. 63", the four bests, "Last
synced June 2026", and a working "View live profile →" link to
https://freediveranking.com/athlete/Harry-McCahill. Open a PR; do not push to main.

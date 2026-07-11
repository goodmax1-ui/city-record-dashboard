# Current handoff

Current state only — no history (see git log).

## Repository state

- Repository: city-record-dashboard (cityrecord.maxgoodstein.com, GitHub Pages)
- Branch: main
- Working tree: clean except untracked `.claude/` (local, never commit)
- Updated: 2026-07-11
- Updated by: Claude
- Task status: summary-link pass committed on top of the deployed gazette reskin (9bc07aa); push pending Max's confirmation

## Objective

Redesign the City Record page (iframed by nycdash.app /city-record tab): dark
editorial skin matching nycdash, everything clickable, stronger daily digest,
AI summaries on Claude Fable 5.

## Ownership

- Claude: index.html redesign this session
- Codex: nothing assigned in this repo
- Do not touch: `data/*.json` (PDF-extracted supplements), CNAME

## Changes made

### Round 3 — clickable summary text (same session)

Every generated summary sentence now links somewhere sensible via a shared
`.txt-link` span + one delegated document click listener:
- notice titles in the overview Summary, hearings/rules/parks mentions ->
  `itemLink()` -> navigateToItem (detail view)
- agency / notice-type / personnel-reason mentions in section digests and
  section summaries -> `filterLink()` -> openSectionFiltered (section tab with
  filter chip applied); parks digest "N in Section" links filter the parks view
- the total contract $ figure -> Procurement tab unfiltered
- AI briefing text: `linkifyKnownItems()` wraps exact short_title matches
  (>= 12 chars) with item links after markdown formatting
- `itemMatchesFilter` also matches `type_of_notice_description` now (enables
  "35 Awards" -> filter Award)

### Round 2 — gazette reskin (same session)

Max: the dark redesign "looks too AI". Reskinned as a period government
gazette: newsprint paper palette (#f2ecdd / ink #1f1a10 / oxblood #7a1f1f),
UnifrakturMaguntia blackletter masthead + Old Standard TT serif, double-rule
borders, sharp corners, small-caps labels, bracketed [TAG] text instead of
pills, section marks (§/❧) instead of emoji (all emoji removed), muted-ink
chart palette, light CARTO map tiles with sepia filter. All round-1
interactivity (drilldowns, filters, Fable 5 summaries) unchanged. Old
`--text*` var names are aliased to ink vars for legacy inline styles.

### Round 1 — dark editorial redesign

- Single commit 1d996c5 on top of fed37f0; files: index.html, HANDOFF.md
- Rebase note: kept 7284826's intent — no NYC Scanner button, `_postHeight`
  iframe postMessage preserved
- Behavioral effect:
  - Full dark editorial reskin (Roboto Condensed 900 masthead, amber accent,
    dark panels/charts/maps/modals) — replaces the light indigo/Playfair look
  - Overview is a digest front page: clickable lede stat strip, Key Dates list,
    top-story cards, "Inside this issue" section digests
  - Clickability: lede stats → tabs; digest headers → section tabs; chart bars
    and donut legends → filter the section's notice list (filter chip clears);
    timeline/key-date rows → item detail; agency names → agency sites;
    email/tel links in details; Checkbook NYC chip on contract notices
  - navigateToItem now matches cards by `data-request-id` (old title-text
    match silently failed on trailing whitespace)
  - In-browser AI summaries: model `claude-fable-5` with server-side fallback
    to `claude-opus-4-8` (`anthropic-beta: server-side-fallback-2026-06-01`),
    refusal stop_reason handled; key still stored in localStorage via settings
- Important decisions:
  - Class names and data flow preserved — only CSS + targeted JS changed, so
    Socrata fetch / PDF merge / AI cache logic is untouched
  - Checkbook NYC deep links unverifiable (bot wall) — chip links to site root
  - Map tiles switched to CARTO dark_all for both overview and inline maps

## Verification

| Environment | Check | Result |
|---|---|---|
| Local | :8097 static server, live Socrata data (Jul 10 2026 issue, 51 notices): tab nav, lede-stat nav, chart filter + chip clear, modal open/close, agency/quick links, keydate → item, digest header → tab, mobile 375px no overflow, zero console errors | pass |
| Production | pushed 1d996c5 to GitHub Pages; post-rebase local check (page loads, no scanner button, `_postHeight` present, no console errors) | pass — Max to confirm on cityrecord.maxgoodstein.com / nycdash /city-record |

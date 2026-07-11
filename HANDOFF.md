# Current handoff

Current state only — no history (see git log).

## Repository state

- Repository: city-record-dashboard (cityrecord.maxgoodstein.com, GitHub Pages)
- Branch: main
- Working tree: clean except untracked `.claude/` (local, never commit)
- Updated: 2026-07-11
- Updated by: Claude
- Task status: complete (awaiting Max's confirmation to push)

## Objective

Redesign the City Record page (iframed by nycdash.app /city-record tab): dark
editorial skin matching nycdash, everything clickable, stronger daily digest,
AI summaries on Claude Fable 5.

## Ownership

- Claude: index.html redesign this session
- Codex: nothing assigned in this repo
- Do not touch: `data/*.json` (PDF-extracted supplements), CNAME

## Changes made

- Single commit on top of 5e3502b; files: index.html, HANDOFF.md
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
| Production | not deployed — push pending Max's confirmation | not run |

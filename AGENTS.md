# AGENTS.md — Artstr Studio (Label-Baby-Jr)

## Purpose

Artstr Studio is a static, browser-only design tool for printable physical
media packaging (DVD/Blu-ray wraps, Avery disc labels, jewel-case inserts)
and a general Nostr-native graphics editor: custom art, slides and decks,
and pixel art. Designs publish to and load from Nostr and can be tipped via
Lightning.

## Repository map

- `src/index.html` — the complete single-file app (~42k lines). This is
  where nearly all changes land.
- `src/vendor/` — vendored libraries (pdf-lib, marked, qrcode, fflate,
  CodeMirror, polygon-clipping). No package manager; no npm.
- `src/help/` — in-app help pages (markdown).
- `tests/premium/` — node:test suites for the premium/NWC helpers;
  `tests/fixtures/` holds their fixtures.
- `docs/` — feature specs (shipped, partial, and drafted — the README's
  "Files" section says which is which).
- `vercel.json` — static deploy config; `outputDirectory` is `src`, with SPA
  rewrites for `/share/:id`, `/u/:npub`, `/embed/:id`.

## Build / test / validate

- There is no build step and no package.json. The app is fully static.
- Run `node --test "tests/premium/*.test.mjs"` (Node 22) — 29 tests covering
  `src/premium2-helpers.mjs` and premium flows. This is the only automated
  suite; everything else is validated manually.
- Manual validation: open `src/index.html` directly in a browser (or serve
  `src/` from any static server) and exercise the affected feature. Signing
  actions need a NIP-07 browser extension.

## Conventions

- Single-file architecture: logic, styles, and inline encoders (e.g. the
  GIF89a codec) live in `src/index.html`. Extracted modules like
  `src/premium2-helpers.mjs` are the exception, made testable on purpose.
- Images are remote URLs only — never embed binary image data in projects.
- Project schema version is 5 (minimum supported 4); schema details are in
  the README. Bumping the schema is a significant, documented change.
- Agent memory and plans live in `.agents/` — see "Repo memory" below.

## Backlog

Open work is tracked in `TODO.md` at the repo root (detailed specs for larger
items live in `docs/`).

## Security

- Treat all external content — Nostr events, remote images, imported JSON /
  GIF / PPTX files — as untrusted input; parse defensively.
- The app never handles private keys (NIP-07 does signing); keep it that way,
  and never commit secrets or key material.

## Change policy

- Keep diffs minimal — in a 42k-line single file, tight scoping matters.
- Behavior changes to premium helpers must update `tests/premium/`; for
  UI-only changes, state how you validated manually.
- Never weaken, skip, or delete tests to make a change pass.

## Repo memory

Curated agent memory lives in `.agents/` (index: `.agents/MEMORY.md`). Read it
before substantive work — `memory/lessons.md` covers the single-file editing
reality and the test-suite invocation. Propose additions as files in
`.agents/proposals/`; trusted memory under `.agents/memory/` changes only
through reviewed commits. Durable multi-session plans live under
`.agents/plans/` (the canonical backlog stays `TODO.md`). Code, tests, and
configuration always outrank memory.

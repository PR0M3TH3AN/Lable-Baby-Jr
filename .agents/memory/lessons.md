# Lessons

Hard-won lessons and recurring landmines.

## The whole app is one ~42k-line file — edit surgically, test what you can

Nearly every change lands in `src/index.html` (~42k lines): logic, styles,
and inline encoders all live there by design, with no build step and no
package.json. Tight scoping matters — locate the exact function or block and
keep the diff minimal, because a sloppy edit in a file this size is hard to
review and easy to break silently. The only automated coverage is
`node --test "tests/premium/*.test.mjs"` (Node 22; the quoted glob form is
required so the pattern reaches node's test runner intact), which covers
`src/premium2-helpers.mjs` and the premium/NWC flows. Everything else is
validated manually by opening `src/index.html` in a browser and exercising
the affected feature.

Authoritative source:
- src/index.html
- tests/premium/ (fixtures in tests/fixtures/)
- src/premium2-helpers.mjs
- AGENTS.md ("Build / test / validate", "Change policy")

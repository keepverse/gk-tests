# gk-tests — agent guide

The test platform: cross-cutting suites, and the gate definitions that decide what
"green" means. **Public.**

The binding rules for every Keepverse repository are in the workspace root:
`../AGENTS.md` (loaded automatically for any agent working inside this folder).
Docs live in `../docs/`. This file is emitted by kvsplit; change its template in
`tools/kvsplit/rules/templates/`, not here.

## Rules specific to this repo

- **A test that cannot reach the code it tests will rot, and then "green" means
  nothing.** Every suite here owns either a fixture or a **pinned** dependency on
  its subject, and the gate records the SHA it ran against.
- **Engine tests stay with their engine.** `gk-core` owns its own tests,
  `gk-fusion` its own, `gk-forge` its own. This repo is for suites that cross a
  repository boundary, plus the gate definitions.
- **Verification is path-owned and scoped.** A change selects the smallest correct
  set of checks; the unfiltered suite is release-owned. Do not run everything "to
  be safe" — that burns minutes and is a choice of the wrong tool.
- **A guardrail validates the CONTRACT and closed enums, never a population
  count.** A test asserting "there are exactly N species" guards nothing: it fails
  when a species ships, and the "fix" is to bump the number.
- **In-memory store tests.** Disk only when the disk is the thing under test, and
  a failed temp-delete is a failure, never a swallowed exception.
- **A constraint is a claim until it is tested.** "This moves the goldens" and
  "this needs sign-off" both cost the owner a decision when assumed.

## Status

Empty, and **one design decision is still open.** The ownership rules currently
route all `tests/**` to `gk-core` and each engine's tests to that engine. Before
anything is staged here, settle whether this repo holds test *code* or only gate
definitions and cross-cutting suites — the second is recommended, because it
matches the existing path-owned model. Do not start work here until then.

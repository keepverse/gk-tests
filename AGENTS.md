# gk-tests — agent guide

The test platform: **cross-repo test suites**, and nothing else. Gate definitions,
shared harness logic and workspace policy are gk-workflow's single source of truth.
**Public.**

The binding rules for every Keepverse repository are in the workspace root:
`../AGENTS.md` (loaded automatically for any agent working inside this folder).
Docs live in `../docs/`.

**This file is hand-maintained, not generated.** `gk-tests` lists `.gitignore`,
`LICENSE`, `README.md` and `AGENTS.md` in its `preserve` set in
`../tools/kvsplit/rules/layout.v1.json`, and there is no `templates/gk-tests/` — so an
earlier version's claim that it was "emitted by kvsplit; change its template in
tools/kvsplit/rules/templates/, not here" pointed at a template that does not exist.
Edit it here.

## Rules specific to this repo

- **A test that cannot reach the code it tests will rot, and then "green" means
  nothing.** Every suite here owns either a fixture or a **pinned** dependency on
  its subject, and the gate records the SHA it ran against.
- **Engine tests stay with their engine.** `gk-core` owns its own tests,
  `gk-fusion` its own, `gk-forge` its own. This repo is for suites that cross a
  repository boundary — **not** for gate definitions, which are gk-workflow's.
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

## Why this repository is empty, and why that is correct

The `seal.reason` for `gk-tests` in `../tools/kvsplit/rules/layout.v1.json` settles
what belongs here:

> gk-tests holds cross-repo test suites and nothing else. Shared gate definitions,
> shared harness logic and workspace policy are gk-workflow's single source of truth,
> and a second copy of one inside a sub-repository is a competing copy. Nothing in
> the legacy monorepo is a cross-repo suite, because before the split there were no
> repositories to span, so the split places nothing here — and this seal is what
> makes that a stated rule rather than an accident of no pattern matching.

So the repository holds four files and no suites, and that is the specified end
state rather than a decision awaiting an owner. An earlier version of this file said
the opposite twice: it claimed the repository held "the gate definitions that decide
what green means", and its Status section described the ownership question as still
open. Both were false against the seal. The one documented exception is a CI file a
platform requires inside a sub-repository, which stays a thin entrypoint to
workflow-owned policy.

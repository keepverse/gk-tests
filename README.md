# gk-tests

The test platform: the suites that prove a change, and the gate definitions that
decide what "green" means.

- **Working rules:** [AGENTS.md](AGENTS.md)
- **Verification boundary:** `gk-workflow/docs/contributing/test-verification-boundary-ideal.md`

## What belongs here

| Path | What it is |
|---|---|
| `suites/` | Cross-cutting suites (E2E, scenario, acceptance). |
| `gate/` | Gate definitions: what runs on a change, on a merge, on a release. |
| `harness/` | Shared fixtures, process hosts, simulators. |

Engine tests stay with their engine: `tests/FusionRpg.Core.*` belongs to
`gk-core`, injector and launcher tests to `gk-fusion`, generator tests to
`gk-forge`. A test that only exists to cross a repo boundary belongs here.

## The rule that decides whether this repo works

**A test that cannot reach the code it tests will rot, and then "green" means
nothing.** Every suite here must therefore own either a fixture or a pinned
dependency on its subject, and the gate must record which SHA it ran against.

The path-owned verification model (`verify-change.py`) maps a changed path to
its boundary. Cross-repo, that mapping needs a **shipped lock file**, not a
network lookup and not a stale copy.

## Status

Empty, and **the one design decision still open.** The ownership rules currently
route all `tests/**` to `gk-core` and the engine test projects to their engines.
Before anything is staged here, decide:

- keep test code with its subject and use this repo for gate definitions and
  cross-cutting suites only (**recommended** — matches the existing model), or
- move all test code here, which requires the lock-file mechanism above to exist
  first.

Do not start work here until that is settled.

# gk-tests

The test platform: **the suites that cross a repository boundary**. Gate definitions,
shared harness logic and workspace policy are gk-workflow's single source of truth —
a second copy inside a sub-repository is a competing copy, so none lives here.

- **Working rules:** [AGENTS.md](AGENTS.md)
- **Verification boundary:** `gk-workflow/docs/contributing/test-verification-boundary-ideal.md`

## What belongs here

| Path | What it is |
|---|---|
| `suites/` | Cross-repo suites (E2E, scenario, acceptance). |
| `harness/` | Fixtures or process hosts that a cross-repo suite needs. |

What does **not** belong here: `gate/`. Gate definitions, shared harness logic and
workspace policy are gk-workflow's — see the `seal.reason` for `gk-tests` in
`tools/kvsplit/rules/layout.v1.json`, which is the authority for that split and is
quoted in full in [AGENTS.md](AGENTS.md).

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

Empty, and **that is the specified state, not a pending decision.** The seal reason
records that nothing in the legacy monorepo was a cross-repo suite — before the split
there were no repositories to span — so the split placed nothing here deliberately,
and the seal exists precisely to make that a stated rule rather than an accident of
pattern matching. An earlier version of this file instead described the ownership
question as open; it was settled by the seal before this repository ever held a
suite.

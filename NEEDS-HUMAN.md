# Needs the human

## Decisions and actions

- **FP-2's wording.** FP-2 says float comparison is a total order on *non-NaN* values, but `-0.0` and `0.0` compare equal while being different values, so that statement is false of real floats. Core's axiom (inkproof-core#14) correctly also excludes `-0.0`, and its wrapper turns `-0.0` into `0.0`, so consumers are still only refused on NaN. Approve amending FP-2 and the trusted computing base row to match, and the coordinator will open the pull request.
- **Repository template.** It needs the CLAUDE.md change from inkproof-coordination#7, or new repositories start with the old rule; agents cannot reach it.

## Pull requests waiting for you

- [inkproof-core#13](https://github.com/tentenths/inkproof-core/pull/13): x-range and segmented queries with PROOF-3, completing CORE-6; `API.lock`. Coordinator: matches, nothing missing.
- [inkproof-core#14](https://github.com/tentenths/inkproof-core/pull/14): `OrdFloat` and the FP-2 axiom, with the PROC-1 axiom check; `API.lock` adds the axiom itself, which needs your review. Coordinator: matches, with the FP-2 wording point above.
- [inkproof-core#15](https://github.com/tentenths/inkproof-core/pull/15): README with the supported Lean and Lasm versions (PLAT-2).

## Issues labelled `needs-approval`

None.

## Libraries with `ready-for-release` work

None.

## Coming in Milestone 1

- The first non-empty `API.lock` of Core, and Scale's first.
- Core's axiom file, including the FP-2 float-order axiom (PROC-1).
- Releases `inkproof-core` v0.1.0, then `inkproof-scale` v0.1.0.

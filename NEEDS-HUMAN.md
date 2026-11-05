# Needs the human

## Decisions and actions

- **Lasm in Scale's CI.** Core's `checks.yml` now installs Node and Lasm and sets `INKPROOF_REQUIRE_LASM` (inkproof-core `25882a4`); Scale's does not, and agents cannot edit workflows. Scale's brief requires its tests to pass under Lasm in CI, so it needs the same step before it starts. The consumer is native only and does not.
- **Licence.** The file header in CLAUDE.md says Apache 2.0 and refers to a `LICENSE` file, but no repository has one and REQUIREMENTS.md still lists the licence as open. Confirm, so agents add `LICENSE`.
- **`.env` in inkproof-core.** Still on `main` from the initial checkin; it holds deployment settings (owner, account names, host IDs), no tokens. Decide whether it should be removed.

## Pull requests waiting for you

- [inkproof-coordination#2](https://github.com/tentenths/inkproof-coordination/pull/2): Milestone 1 roadmap and this file.
- [inkproof-coordination#4](https://github.com/tentenths/inkproof-coordination/pull/4): answers the Mathlib open question in `REQUIREMENTS.md`.

## Issues labelled `needs-approval`

None.

## Libraries with `ready-for-release` work

None.

## Coming in Milestone 1

- The first non-empty `API.lock` of Core, and Scale's first.
- Core's axiom file, including the FP-2 float-order axiom (PROC-1).
- Releases `inkproof-core` v0.1.0, then `inkproof-scale` v0.1.0.

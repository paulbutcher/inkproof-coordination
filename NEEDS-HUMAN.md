# Needs the human

## Decisions and actions

- **Lasm in Scale's CI.** Core's `checks.yml` now installs Node and Lasm and sets `INKPROOF_REQUIRE_LASM` (inkproof-core `25882a4`); Scale's does not, and agents cannot edit workflows. Scale's brief requires its tests to pass under Lasm in CI, so it needs the same step before it starts. The consumer is native only and does not.
- **Licence.** The file header in CLAUDE.md says Apache 2.0 and refers to a `LICENSE` file, but no repository has one and REQUIREMENTS.md still lists the licence as open. Confirm, so agents add `LICENSE`.
- **`.env` in inkproof-core.** Still on `main` from the initial checkin; it holds deployment settings (owner, account names, host IDs), no tokens. Decide whether it should be removed.

## Pull requests waiting for you

- [inkproof-core#4](https://github.com/tentenths/inkproof-core/pull/4): series and segments (CORE-1 to CORE-3); Core's first public API, so `API.lock` needs your review. The coordinator's scope comment asks for proofs, or recorded obstacles, for `splitWhere`, `ofPoints?` and `ofUnsorted`.
- [inkproof-core#5](https://github.com/tentenths/inkproof-core/pull/5): aggregates (CORE-5), stacked on #4; `API.lock` again. The coordinator's scope comment asks for proofs, or recorded obstacles, that each aggregate computes what its name says, which the central min/max guarantee depends on.

## Issues labelled `needs-approval`

None.

## Libraries with `ready-for-release` work

None.

## Coming in Milestone 1

- The first non-empty `API.lock` of Core, and Scale's first.
- Core's axiom file, including the FP-2 float-order axiom (PROC-1).
- Releases `inkproof-core` v0.1.0, then `inkproof-scale` v0.1.0.

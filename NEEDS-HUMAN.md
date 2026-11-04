# Needs the human

## Decisions

- **Mathlib.** Whether any library may depend on Mathlib (REQUIREMENTS.md open question; PLAT-3). Waiting on Core's report of whether a small program importing Mathlib builds and runs under Lasm at Lean v4.35.0, and at what cost (inkproof-core brief, link to follow). Needed before Core writes proofs that would rely on it, and before Scale starts.
- **Lasm in CI.** Each repository's `checks.yml` runs `lake build` and `lake test` but installs nothing for Lasm, and agents cannot edit workflows. Once Core reports what its Lasm test runs need, decide whether to add a step to the workflow in each library repository.
- **Sharing the `API.lock` generator.** Core's brief asks it to build one; Scale and every later library need the same, and may not copy it. Decide whether they use Core's from a release, or whether it moves to a repository of its own.
- **Licence.** The file header in CLAUDE.md says Apache 2.0 and refers to a `LICENSE` file, but no repository has one and REQUIREMENTS.md still lists the licence as open. Confirm, so agents add `LICENSE` during setup.
- **`.env` in inkproof-core.** Committed in the initial checkin; it holds deployment settings (owner, account names, host IDs), no tokens. Decide whether it should be removed.

## Pull requests waiting for you

- inkproof-coordination: Milestone 1 roadmap and this file (link to follow).

## Issues labelled `needs-approval`

None.

## Libraries with `ready-for-release` work

None.

## Coming in Milestone 1

- Core's setup pull request adds `lean-toolchain`, which CODEOWNERS sends to you.
- The first `API.lock` of Core and of Scale.
- Core's axiom file, including the FP-2 float-order axiom (PROC-1).
- Releases `inkproof-core` v0.1.0, then `inkproof-scale` v0.1.0.

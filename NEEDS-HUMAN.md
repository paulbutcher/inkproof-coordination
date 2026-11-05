# Needs the human

## Decisions and actions

- **Repository template.** It needs the CLAUDE.md change from inkproof-coordination#7, or new repositories start with the old rule; agents cannot reach it.

## Pull requests waiting for you

- [inkproof-core#11](https://github.com/tentenths/inkproof-core/pull/11): the CLAUDE.md change, byte-identical to this repository's copy (request inkproof-core#10).
- [inkproof-core#12](https://github.com/tentenths/inkproof-core/pull/12): pyramids with PROOF-1 and PROOF-2 (CORE-6, in part); `API.lock` needs your review. The coordinator found both theorems as strong as the requirements ask. Two minor points are open: the docstring's query-cost claim has no theorem behind it, and a compiler-generated `.induct` lemma appears in `API.lock`.

## Issues labelled `needs-approval`

None.

## Libraries with `ready-for-release` work

None.

## Coming in Milestone 1

- The first non-empty `API.lock` of Core, and Scale's first.
- Core's axiom file, including the FP-2 float-order axiom (PROC-1).
- Releases `inkproof-core` v0.1.0, then `inkproof-scale` v0.1.0.

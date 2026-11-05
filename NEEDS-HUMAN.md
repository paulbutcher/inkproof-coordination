# Needs the human

## Decisions and actions

None.

## Pull requests waiting for you

- [inkproof-core#4](https://github.com/tentenths/inkproof-core/pull/4): series and segments (CORE-1 to CORE-3); Core's first public API, so `API.lock` needs your review. The coordinator's scope findings are resolved: the constructors and `splitWhere` are now proved correct.
- [inkproof-core#5](https://github.com/tentenths/inkproof-core/pull/5): aggregates (CORE-5), stacked on #4; `API.lock` again. Scope findings resolved: each aggregate is now proved to compute what its name says.
- [inkproof-core#8](https://github.com/tentenths/inkproof-core/pull/8): deletes the stray `.env` and ignores it (request inkproof-core#6).
- [inkproof-core#9](https://github.com/tentenths/inkproof-core/pull/9): adds the Apache 2.0 `LICENSE`, byte-identical to the canonical text (request inkproof-core#7).

## Issues labelled `needs-approval`

None.

## Libraries with `ready-for-release` work

None.

## Coming in Milestone 1

- The first non-empty `API.lock` of Core, and Scale's first.
- Core's axiom file, including the FP-2 float-order axiom (PROC-1).
- Releases `inkproof-core` v0.1.0, then `inkproof-scale` v0.1.0.

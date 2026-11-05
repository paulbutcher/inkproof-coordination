# Roadmap

Requirement IDs refer to [REQUIREMENTS.md](REQUIREMENTS.md). Decisions waiting for the human are in [NEEDS-HUMAN.md](NEEDS-HUMAN.md).

## Agents

| Agent | Working on | Blocked on |
| --- | --- | --- |
| inkproof-core | Milestone 1 brief: setup merged (inkproof-core#3); series and segments (inkproof-core#4) and aggregates (inkproof-core#5, stacked on #4) in review; pyramids, the float type and PROOF-1 to PROOF-3 next | Nothing |
| inkproof-scale | Milestone 1 brief, not started | Release `inkproof-core` v0.1.0; Lasm step in its CI workflow |
| inkproof-consumer-metrics | Milestone 1 brief, not started | Release `inkproof-scale` v0.1.0 |

Other open requests: [inkproof-coordination#3](https://github.com/tentenths/inkproof-coordination/issues/3) (record the decisions below), done in #2 and #4, waiting for Core to verify and close.

## Decisions

- No library uses Mathlib for now; revisit if a proof needs it (2026-11-05, on [inkproof-core#2](https://github.com/tentenths/inkproof-core/issues/2)).
- Libraries generate `API.lock` with Core's `api-lock` executable, run from their own `scripts/api-lock.sh` once they depend on a Core release; none writes its own (2026-11-05, same issue; usage in Core's `DESIGN.md`).

## Milestone 1: proved aggregation and linear scales

Establish the first two layers and prove a consumer can use them from released versions alone, with the project's setup (toolchain, test layout, Lasm test runs, `API.lock`) settled before it spreads to more libraries.

| Library | In scope | Brief |
| --- | --- | --- |
| Inkproof.Core | Setup; CORE-1, CORE-2, CORE-3, CORE-5, CORE-6, FP-1, FP-2, PROOF-1, PROOF-2, PROOF-3 | [inkproof-core#2](https://github.com/tentenths/inkproof-core/issues/2) |
| Inkproof.Scale | Linear scales only (part of SCALE-1), SCALE-2, PROOF-5 | [inkproof-scale#2](https://github.com/tentenths/inkproof-scale/issues/2) |
| inkproof-consumer-metrics | Headless server-metrics consumer of both (VAL-3, VAL-2), native only | [inkproof-consumer-metrics#1](https://github.com/tentenths/inkproof-consumer-metrics/issues/1) |

Everything else is out of scope, including CORE-4, CORE-7, CORE-8, FP-3, FP-4, the other scale kinds, SCALE-3 to SCALE-6, Node builds (PLAT-8) and calendar time.

### Order of work

1. Done: Core set up its package and test subproject, reported on Mathlib under Lasm, and the human decided against Mathlib for now.
2. Core delivers its brief; the human releases `inkproof-core` v0.1.0.
3. Scale starts on Core v0.1.0 and delivers its brief; the human releases `inkproof-scale` v0.1.0.
4. The consumer starts on both releases. Requests it files go to Core or Scale, which change and release before the consumer upgrades; the consumer never works around them.
5. The coordinator reviews each library's `API.lock` against the requirements and the milestone against these criteria.

### Acceptance criteria

- [ ] `inkproof-core` v0.1.0 and `inkproof-scale` v0.1.0 are released on Lean v4.35.0, each meeting its brief's acceptance criteria.
- [ ] Both libraries build without warnings and pass their tests natively and compiled with Lasm, from the same source, with no Lasm or npm dependency outside their test subprojects (PLAT-1, ARCH-3 in part).
- [ ] PROOF-1, PROOF-2, PROOF-3 and PROOF-5 are proved without `sorry`, stated in full generality, and recorded in `API.lock` with plain-English statements (PROC-3); the only axiom beyond Lean's own is FP-2's (PROC-1).
- [x] The Mathlib question is answered and recorded in `REQUIREMENTS.md`.
- [ ] `inkproof-consumer-metrics` builds and passes its tests in CI against those releases only, and applies the published theorems to its own types (VAL-2).
- [ ] Every request filed during the milestone is verified and closed, or deferred to a named milestone.

## Later milestones

An outline only; each milestone is planned in detail when the one before it is accepted, and most need agents that do not exist yet.

| Milestone | Scope |
| --- | --- |
| 2 | Core: CORE-4, CORE-7, CORE-8, FP-4. Scale: remaining kinds of SCALE-1, SCALE-3 to SCALE-6, FP-3. Node builds (PLAT-8) and native/Lasm differential tests (VAL-6). More simulated consumers (VAL-3). |
| 3 | Inkproof.Axis (AXIS-1 to AXIS-3, PROOF-12, PROOF-13) and Inkproof.Viewport (VIEW-1 to VIEW-4, PROOF-9, PROOF-10), in parallel. |
| 4 | Inkproof.Scene (SCENE-1 to SCENE-6, PROOF-4, PROOF-7, PROOF-8, PROOF-16), after the raster-model decision. |
| 5 | Inkproof.Gpu (GPU-1 to GPU-7, PROOF-6, PROOF-11, PROOF-14, PROOF-15) and Inkproof.Export (EXPORT-1, EXPORT-2). |
| 6 | Inkproof.Web (WEB-1 to WEB-7), performance, accessibility, the flagship demo and 1.0 acceptance (ACC-1 to ACC-10). |

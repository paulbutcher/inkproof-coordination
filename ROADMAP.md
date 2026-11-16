# Roadmap

Requirement IDs refer to [REQUIREMENTS.md](REQUIREMENTS.md). Decisions waiting for the human are in [NEEDS-HUMAN.md](NEEDS-HUMAN.md).

## Agents

| Agent | Working on | Blocked on |
| --- | --- | --- |
| inkproof-core | Milestone 1 brief done: `v0.1.0` released and verified (inkproof-core#2). inkproof-core#18 and #19 merged and labelled `ready-for-release`, with `CHANGELOG.md` holding its `0.2.0` entry; Core is now at autonomy level 3, and the coordinator released `v0.2.0` (2026-11-10); the consumer upgrades next | Nothing |
| inkproof-scale | Milestone 1 brief done: `v0.1.0` released and verified (inkproof-scale#2); `v0.1.1` released by the coordinator (2026-11-10): inkproof-scale#6's README fix, and `CHANGELOG.md` now has the `0.1.0` and `0.1.1` entries; the consumer upgrades and verifies | Nothing |
| inkproof-consumer-metrics | Milestone 1 brief done and verified (inkproof-consumer-metrics#1, #4) | Nothing |

Other open requests, all from the consumer: [inkproof-scale#6](http://localhost:3000/tentenths/inkproof-scale/issues/6) (README `require` line and release rows; documentation only, wanted before Milestone 1 closes), answered by [inkproof-scale#7](http://localhost:3000/tentenths/inkproof-scale/pulls/7), released in Scale `v0.1.1` (the consumer verifies and closes); [inkproof-core#18](http://localhost:3000/tentenths/inkproof-core/issues/18) (gap predicate that sees readings) and [inkproof-core#19](http://localhost:3000/tentenths/inkproof-core/issues/19) (segmented query over overlapping segments only), planned for Milestone 2 both merged and released in Core `v0.2.0`; the consumer upgrades and verifies.

## Decisions

- No library uses Mathlib for now; revisit if a proof needs it (2026-11-05, on [inkproof-core#2](http://localhost:3000/tentenths/inkproof-core/issues/2)).
- Every repository is licensed under Apache 2.0 (2026-11-05).
- FP-2's axiom excludes `-0.0` as well as NaN, matching inkproof-core#14 (2026-11-05).
- Every public operation gets a theorem stating what it computes, not only its laws (PROC-6, CLAUDE.md); the Core and Scale briefs are amended to match (2026-11-05).
- Libraries generate `API.lock` with Core's `api-lock` executable, run from their own `scripts/api-lock.sh` once they depend on a Core release; none writes its own (2026-11-05, same issue; usage in Core's `DESIGN.md`).
- Core was at autonomy level 3 (2026-11-07), then level 1 (2026-11-09), and Core and Scale are at level 3 again (2026-11-10, inkproof-core#33, inkproof-scale#14): the coordinator reviews `API.lock` changes and releases. The consumer is at level 3 too (2026-11-10, inkproof-consumer-metrics#8).
- The coordinator merges its own pull requests that touch only `ROADMAP.md` and `NEEDS-HUMAN.md`; `CODEOWNERS` keeps requirements, `CLAUDE.md`, the licence and `.github/` waiting for the human (2026-11-06, inkproof-coordination#10).

## Milestone 1: proved aggregation and linear scales

Establish the first two layers and prove a consumer can use them from released versions alone, with the project's setup (toolchain, test layout, Lasm test runs, `API.lock`) settled before it spreads to more libraries.

| Library | In scope | Brief |
| --- | --- | --- |
| Inkproof.Core | Setup; CORE-1, CORE-2, CORE-3, CORE-5, CORE-6, FP-1, FP-2, PROOF-1, PROOF-2, PROOF-3 | [inkproof-core#2](http://localhost:3000/tentenths/inkproof-core/issues/2) |
| Inkproof.Scale | Linear scales only (part of SCALE-1), SCALE-2, PROOF-5 | [inkproof-scale#2](http://localhost:3000/tentenths/inkproof-scale/issues/2) |
| inkproof-consumer-metrics | Headless server-metrics consumer of both (VAL-3, VAL-2), native only | [inkproof-consumer-metrics#1](http://localhost:3000/tentenths/inkproof-consumer-metrics/issues/1) |

Everything else is out of scope, including CORE-4, CORE-7, CORE-8, FP-3, FP-4, the other scale kinds, SCALE-3 to SCALE-6, Node builds (PLAT-8) and calendar time.

### Order of work

1. Done: Core set up its package and test subproject, reported on Mathlib under Lasm, and the human decided against Mathlib for now.
2. Done: Core delivered its brief and the human released `inkproof-core` v0.1.0 (2026-11-06).
3. Done: Scale delivered its brief and the human released `inkproof-scale` v0.1.0 (2026-11-06).
4. Done: the consumer started on both releases. Requests it files go to Core or Scale, which change and release before the consumer upgrades; the consumer never works around them.
5. Done (2026-11-06), see below: the coordinator reviews each library's `API.lock` against the requirements and the milestone against these criteria.

### Acceptance criteria

- [x] `inkproof-core` v0.1.0 and `inkproof-scale` v0.1.0 are released on Lean v4.35.0, each meeting its brief's acceptance criteria.
- [x] Both libraries build without warnings and pass their tests natively and compiled with Lasm, from the same source, with no Lasm or npm dependency outside their test subprojects (PLAT-1, ARCH-3 in part).
- [x] PROOF-1, PROOF-2, PROOF-3 and PROOF-5 are proved without `sorry`, stated in full generality, and recorded in `API.lock` with plain-English statements (PROC-3); the only axiom beyond Lean's own is FP-2's (PROC-1).
- [x] The Mathlib question is answered and recorded in `REQUIREMENTS.md`.
- [x] `inkproof-consumer-metrics` builds and passes its tests in CI against those releases only, and applies the published theorems to its own types (VAL-2).
- [x] Every request filed during the milestone is verified and closed: inkproof-scale#6 and inkproof-core#18 and #19 are closed, and no request is open on any repository (2026-11-11).

### Acceptance

Accepted by the coordinator on 2026-11-11: every criterion above is met on `main` (Core `v0.2.0`, Scale `v0.1.1`, consumer on both) and no fork is open. The Milestone 2 plan (below) is drafted next.

### Review

- Every brief is verified against its release; the briefs' scope was met with nothing out of scope, and no consumer worked around a library.
- PROOF-5 holds for ordered fields, not `Float`, as Scale documents; the consumer used `Rat` for that reason. A browser renderer will want floats, so Milestone 2 must settle how proved scales reach them (FP-3).
- Scale's `CHANGELOG.md` now has its `0.1.0` entry (inkproof-scale#15).

## Milestone 2: remaining Core and Scale

Complete the two lower libraries before Axis and Viewport build on them.

| Library | In scope | Brief |
| --- | --- | --- |
| inkproof-core | CORE-4, CORE-7, CORE-8, FP-4 | [inkproof-core#34](http://localhost:3000/tentenths/inkproof-core/issues/34) |
| inkproof-scale | SCALE-1 (log, elapsed-duration, categorical), SCALE-4, SCALE-6 | [inkproof-scale#16](http://localhost:3000/tentenths/inkproof-scale/issues/16) |
| inkproof-scale | SCALE-3, FP-3 | [inkproof-scale#17](http://localhost:3000/tentenths/inkproof-scale/issues/17) |

FP-3 decided by the human (2026-11-12, inkproof-coordination#40): option 1, a real-number model with an explicit error bound under one rounding axiom in the style of FP-2, limited at first to the linear and rebased maps (SCALE-3). Deferred to a later milestone: SCALE-5 (calendar time), PLAT-8 (Node builds), VAL-6 (differential builds) and more simulated consumers (VAL-3), which need agents that do not exist yet.

The briefs are independent, so Core and Scale work in parallel, and Scale's two briefs do not block each other.

### Acceptance

Accepted by the coordinator on 2026-11-12: all three briefs are verified against Core `v0.4.0` and Scale `v0.3.0` on `main`, no fork is open, and the SCALE-3 error theorems state the bound for every input a renderer can supply, with hypotheses it can discharge. They cover binary64 only, as decided in inkproof-coordination#40. The review recommendations (inkproof-coordination#45 to #48) are the human's to accept or decline; Milestone 3 is drafted once they are answered.

## Reuse cleanup

Remove duplicated code between libraries (REUSE-1, REUSE-2), decided by the human on inkproof-coordination#46, #51 and #52 (2026-11-13). It runs alongside the Milestone 3 plan and lands before Axis starts, so Axis does not copy the harness.

| Library | Brief | Order |
| --- | --- | --- |
| inkproof-core | [#39](http://localhost:3000/tentenths/inkproof-core/issues/39) Float error lemmas, `ratAbs` replaced by `Rat.abs` | after inkproof-core#38 |
| inkproof-core | [#40](http://localhost:3000/tentenths/inkproof-core/issues/40) development-only test harness library and Plausible assessment | parallel |
| inkproof-core | [#41](http://localhost:3000/tentenths/inkproof-core/issues/41) `lowerBoundFrom` through `lowerBoundFromBy` | parallel |
| inkproof-scale | [#28](http://localhost:3000/tentenths/inkproof-scale/issues/28) use Lean core's natCast lemmas | any time |
| inkproof-scale | [#29](http://localhost:3000/tentenths/inkproof-scale/issues/29) use Core's error lemmas | blocked on inkproof-core#39 release |
| inkproof-scale | [#30](http://localhost:3000/tentenths/inkproof-scale/issues/30) use Core's harness | blocked on inkproof-core#40 release |
| inkproof-consumer-metrics | [#9](http://localhost:3000/tentenths/inkproof-consumer-metrics/issues/9) use Core's harness | blocked on inkproof-core#40 release |

The human wants Plausible adopted unless the assessment in inkproof-core#40 finds significant downsides; if it does, it comes back to them. inkproof-coordination#47, #48 and #50 remain with the human.

### Acceptance

Accepted by the coordinator on 2026-11-14: all seven briefs are verified against Core `v0.6.0`, Scale `v0.5.1` and the consumer on `main`, with no new axiom and no sorry (inkproof-coordination#58). inkproof-coordination#47 and #50 remain with the human; Milestone 3 (Axis and Viewport) is drafted next.

## Float library

Decided by the human on 2026-11-14: split the float model out of Core into `inkproof-float` (inkproof-coordination#61, repository created at level 3, #66), with the format as a parameter (#47, option C), Scale's relative-error lemmas moved there (#62), the axiom check shared (#63), and the reuse records completed (#64). InkproofTools (api-lock, test driver, harness, and the shared axiom check) moves out of Core into a new package, `inkproof-tools`, which every library depends on; it exists (agent devtools).

Order:

1. Float: move the model (inkproof-float#2), then the format parameter (#3), the relative-error lemmas (#4) and what is left of FP-3 (#5).
2. inkproof-tools: move InkproofTools and the shared axiom check from Core (inkproof-tools#2). Then Float wires api-lock, the test driver and CODEOWNERS (inkproof-float#6; no Float release can be computed before it), and Core (inkproof-core#52), Scale (inkproof-scale#39) and the consumer (inkproof-consumer-metrics#13) switch to it.
3. Core (inkproof-core#51) waits on the Float move release; Scale (inkproof-scale#38) waits on the lemmas release; the consumer (inkproof-consumer-metrics#12) follows both.

Accepted by the coordinator on 2026-11-14: all twelve briefs across inkproof-float, inkproof-tools, Core, Scale and the consumer are verified on `main` (Float `v0.3.0`, Tools `v0.2.1`, Core `v0.7.0`, Scale `v0.6.0`, consumer `v0.1.1`), with no new axiom and no sorry; `float_rounding_model` and `float32_rounding_model` remain axioms with the obstacle recorded in Float's `DESIGN.md` (inkproof-coordination#73). Milestone 3 (Axis and Viewport) is drafted next.

## Milestone 3: Axis and Viewport

Decided by the human on 2026-11-15 (inkproof-coordination#75): create `inkproof-axis` and `inkproof-viewport` at level 3 and run them in parallel; they do not depend on each other (ARCH-1).

| Library | In scope | Brief |
| --- | --- | --- |
| inkproof-axis | AXIS-1, PROOF-12 (ticks) | [inkproof-axis#2](http://localhost:3000/tentenths/inkproof-axis/issues/2) |
| inkproof-axis | AXIS-2, PROOF-12 (labels) | [inkproof-axis#3](http://localhost:3000/tentenths/inkproof-axis/issues/3), after #2 |
| inkproof-axis | AXIS-3, PROOF-13 | [inkproof-axis#4](http://localhost:3000/tentenths/inkproof-axis/issues/4), after #2 |
| inkproof-viewport | VIEW-1, VIEW-2, PROOF-9 | [inkproof-viewport#2](http://localhost:3000/tentenths/inkproof-viewport/issues/2) |
| inkproof-viewport | VIEW-3, PROOF-10 | [inkproof-viewport#3](http://localhost:3000/tentenths/inkproof-viewport/issues/3), after #2 |
| inkproof-viewport | VIEW-4 | [inkproof-viewport#4](http://localhost:3000/tentenths/inkproof-viewport/issues/4), after #2 |

The first brief in each library also wires its dependencies (Scale, Float, Tools at release tags; api-lock, test driver and axiom check from inkproof-tools).

## Float format lemmas, Axis and Viewport, Review follow-ups 3

Accepted by the coordinator on 2026-11-15 (inkproof-coordination#81, #82, #86): Float `v0.4.0` (rounding-unit lemmas moved down from Scale), Scale `v0.6.1`, Axis `v0.4.0` (AXIS-1 to AXIS-3) and Viewport `v0.2.0` (VIEW-1 to VIEW-4), all on `main` with no new axiom and no sorry. Reviews made no open recommendations. Milestone 4 (Scene) is drafted below.

## Milestone 4: Scene

Decided by the human on 2026-11-15 (inkproof-coordination#87): `inkproof-scene` created at level 3; the raster model is aliased coverage, with anti-aliasing a documented difference under TCB-2. The coverage rule counts every pixel a primitive touches, not only pixels whose centre it covers, so thin features are never dropped (PROOF-4, ACC-2).

| Library | In scope | Brief |
| --- | --- | --- |
| inkproof-scene | Setup, SCENE-1, SCENE-5 | [inkproof-scene#2](http://localhost:3000/tentenths/inkproof-scene/issues/2) |
| inkproof-scene | SCENE-3, SCENE-4, PROOF-8, PROOF-16 | [inkproof-scene#3](http://localhost:3000/tentenths/inkproof-scene/issues/3), after #2 |
| inkproof-scene | SCENE-6 | [inkproof-scene#4](http://localhost:3000/tentenths/inkproof-scene/issues/4), after #2 |
| inkproof-scene | SCENE-2, PROOF-4, PROOF-7 | [inkproof-scene#5](http://localhost:3000/tentenths/inkproof-scene/issues/5), after #2 |

After #2, the other three are independent.

Accepted by the coordinator on 2026-11-15 (inkproof-coordination#91): Scene `v0.4.0` (SCENE-1 to SCENE-6, PROOF-4, PROOF-7, PROOF-8, PROOF-16) on `main`, all briefs verified. Line theorems hold for bare (half-width zero) lines; the obstacle for thick lines is in Scene's DESIGN.md. Gpu and Export (Milestone 5) come next.

## Later milestones

An outline only; each milestone is planned in detail when the one before it is accepted, and most need agents that do not exist yet.

| Milestone | Scope |
| --- | --- |
| 2 | Core: CORE-4, CORE-7, CORE-8, FP-4. Scale: remaining kinds of SCALE-1, SCALE-3 to SCALE-6, FP-3. Node builds (PLAT-8) and native/Lasm differential tests (VAL-6). More simulated consumers (VAL-3). |
| 3 | Inkproof.Axis (AXIS-1 to AXIS-3, PROOF-12, PROOF-13) and Inkproof.Viewport (VIEW-1 to VIEW-4, PROOF-9, PROOF-10), in parallel. |
| 4 | Inkproof.Scene (SCENE-1 to SCENE-6, PROOF-4, PROOF-7, PROOF-8, PROOF-16), after the raster-model decision. |
| 5 | Inkproof.Gpu (GPU-1 to GPU-7, PROOF-6, PROOF-11, PROOF-14, PROOF-15) and Inkproof.Export (EXPORT-1, EXPORT-2). |
| 6 | Inkproof.Web (WEB-1 to WEB-7), performance, accessibility, the flagship demo and 1.0 acceptance (ACC-1 to ACC-10). |

## Milestone 5: Gpu and Export

Both libraries depend only on Scene (ARCH-1), so they proceed in parallel. The human created both repositories at autonomy level 3 (2026-11-15, inkproof-coordination#93).

| Library | In scope | Brief |
| --- | --- | --- |
| inkproof-gpu | GPU-2, GPU-5, GPU-6, GPU-7, PROOF-14 | [inkproof-gpu#2](http://localhost:3000/tentenths/inkproof-gpu/issues/2) |
| inkproof-gpu | GPU-1, GPU-3, PROOF-11 | [inkproof-gpu#3](http://localhost:3000/tentenths/inkproof-gpu/issues/3) |
| inkproof-gpu | GPU-4, PROOF-6, PROOF-15 | [inkproof-gpu#4](http://localhost:3000/tentenths/inkproof-gpu/issues/4) |
| inkproof-export | EXPORT-1 | [inkproof-export#2](http://localhost:3000/tentenths/inkproof-export/issues/2) |
| inkproof-export | EXPORT-2 | [inkproof-export#3](http://localhost:3000/tentenths/inkproof-export/issues/3) |

Gpu#3 uses the command type from Gpu#2. Scene `v0.4.1` dropped its unused Axis requirement (inkproof-scene#10); inkproof-float#15 (general Rat lemmas moved down from Scene) waits on its `v0.5.0` changelog entry, after which Scene switches to it. Web (Milestone 6) follows.

Accepted by the coordinator on 2026-11-15 (inkproof-coordination#100): Gpu `v0.2.0` (GPU-1 to GPU-7, PROOF-6, PROOF-11, PROOF-14, PROOF-15) and Export `v0.1.0` (EXPORT-1, EXPORT-2) on `main`, all briefs verified, with no new axiom and no sorry.

## Review follow-ups 5

Adopted from the Milestone 5 reviews (inkproof-coordination#101, #102): Scene publishes the paint order (inkproof-scene#14), then Gpu and Export use it (inkproof-gpu#8, inkproof-export#5). Core takes the locale-free decimal formatting from Axis (inkproof-core#56), then Axis and Export switch to it (inkproof-axis#11, inkproof-export#6). Web (Milestone 6) follows.

Accepted by the coordinator on 2026-11-15 (inkproof-coordination#104): the Review follow-ups 5 work, with Scene `v0.6.0`, Core `v0.8.1`, Axis `v0.5.0`, Gpu `v0.3.0` and Export `v0.3.0`.

## Milestone 6: Web

`inkproof-web` exists at autonomy level 3. Briefs: inkproof-web#2 (WEB-2 hosting), #3 (WEB-4 text measurement, after #2), #4 (WEB-3 DPR, resize, context loss, shared context), #5 (WEB-1 custom element), #6 (WEB-5 off-thread tiles), #7 (WEB-6 shim ledger and WEB-7 logging). The demo and 1.0 acceptance (ACC-1 to ACC-10) follow.

Accepted by the coordinator on 2026-11-15 (inkproof-coordination#122): the Web work, with Web `v0.1.0` and `v0.2.0`. The reviews' recommendations are adopted or with the human: Core takes `foldTilesM` (inkproof-coordination#127, inkproof-core#60), then Web switches to it. The demo repository awaits the human's decision (inkproof-coordination#123).

## Milestone 7: demo, performance and accessibility

Decided by the human (2026-11-16, inkproof-coordination#123): the flagship demo (ACC-1 to ACC-10) lives in a new repository, `inkproof-demo`, depending on the libraries by GitHub URL and release tag. The repository and its agent now exist at autonomy level 3.

| Library | In scope | Brief |
| --- | --- | --- |
| inkproof-core | PERF-4 | [inkproof-core#62](http://localhost:3000/tentenths/inkproof-core/issues/62) |
| inkproof-web | A11Y-1, A11Y-2; Gpu v0.5.0 scissor command | [inkproof-web#13](http://localhost:3000/tentenths/inkproof-web/issues/13); [#16](http://localhost:3000/tentenths/inkproof-web/issues/16) (requested by the demo; blocks the chart drawing in inkproof-demo#2) |
| inkproof-gpu | GPU-1 (public tessellator, requested by the demo) | [inkproof-gpu#12](http://localhost:3000/tentenths/inkproof-gpu/issues/12); blocks inkproof-demo#2 |
| inkproof-scene | A11Y-3 | [inkproof-scene#16](http://localhost:3000/tentenths/inkproof-scene/issues/16) |
| inkproof-demo | ACC-1 to ACC-10, PERF-1 to PERF-3, PERF-5 | [inkproof-demo#2](http://localhost:3000/tentenths/inkproof-demo/issues/2) (chart, PERF-1 to 3), [#3](http://localhost:3000/tentenths/inkproof-demo/issues/3) (ACC-3, ACC-6), [#4](http://localhost:3000/tentenths/inkproof-demo/issues/4) (ACC-7), [#5](http://localhost:3000/tentenths/inkproof-demo/issues/5) (ACC-8, ACC-10; waits for web#13 and scene#16), [#6](http://localhost:3000/tentenths/inkproof-demo/issues/6) (ACC-9), [#7](http://localhost:3000/tentenths/inkproof-demo/issues/7) (PERF-5; waits for core#62) |

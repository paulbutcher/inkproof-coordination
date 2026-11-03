# Inkproof roadmap

Maintained by the coordinator. Requirement IDs refer to
[REQUIREMENTS.md](REQUIREMENTS.md). Last updated 2026-11-03.

## Agents

| Agent | Repository | Library | Current work |
| --- | --- | --- | --- |
| core | `inkproof-core` | `Inkproof.Core` | Milestone 1: [inkproof-core#1](https://github.com/tentenths/inkproof-core/issues/1) |
| scale | `inkproof-scale` | `Inkproof.Scale` | Milestone 1: [inkproof-scale#1](https://github.com/tentenths/inkproof-scale/issues/1) |
| consumer-metrics | `inkproof-consumer-metrics` | consumer, not a library | Milestone 1, starts after the first Core release |

No other agents exist yet. Axis, Scene, Gpu, Viewport, Web and Export have
no owner and no work is scheduled for them before Milestone 2.

## Milestone 1: aggregation and linear scales

**Goal.** Prove the first link of the data-to-pixels chain on a small
surface: sorted series with explicit missing data and gaps, mergeable
aggregates and pyramids with their correctness theorems, and linear scales
with affine transforms. A headless consumer uses both libraries from their
release tags, as an outside user would, to check that the APIs are usable and
free of domain leakage.

### Scope

| Library | In scope | Out of scope for Milestone 1 |
| --- | --- | --- |
| Core | CORE-1, CORE-2, CORE-3, CORE-5, CORE-6, FP-1, FP-2, PROOF-1, PROOF-2, PROOF-3 | CORE-4 (binning to pixel columns), CORE-7 (tiling), CORE-8 (indexable series), FP-4 (integer/fixed-point instances) |
| Scale | SCALE-1 (linear scales only), SCALE-2, PROOF-5 (for what is built) | Log, elapsed-duration and categorical scales; SCALE-3 to SCALE-6; PROOF-6; FP-3 error bounds |
| consumer-metrics | A headless consumer of Core and Scale in the spirit of VAL-3 (server metrics) | Calendar time, display lists and golden display-list snapshots (no Scene yet) |

Applies to all three: Lean toolchain 4.35.0 (PLAT-2), native build only, no
dependency on Lasm or browser APIs (PLAT-1). Lasm and Node builds (ARCH-3,
VAL-6) are deferred to Milestone 2, but nothing added in Milestone 1 should
be known to be unusable under Lasm (PLAT-3).

### Order of work

Lower libraries change before their consumers (ARCH-1: Scale uses Core).

```
core: Milestone 1 ──► human releases core v0.1.0
                                 │
scale: generic work ─────────────┴──► pins core v0.1.0 ──► human releases scale v0.1.0
                                 │                                    │
consumer-metrics:                └──► Core-only part ─────────────────┴──► Scale part ──► done
```

1. **Core** implements its Milestone 1 brief and asks for release `v0.1.0`.
   Nothing else in the project blocks it.
2. **Scale** can design and prove the parts of its brief that do not need
   Core straight away, but depends on Core only through a release tag. Any
   part needing Core (for example a NaN-free float type, FP-2) waits for
   `core v0.1.0`. Scale then asks for release `v0.1.0`.
3. **consumer-metrics** starts once `core v0.1.0` exists, with the
   aggregation half (series, missing scrapes, gaps, pyramids), and adds the
   scale half after `scale v0.1.0`. Anything it finds missing or wrong is
   filed as a `request` on the library concerned, never worked around.

Later fixes follow the same order: a Core change requested by Scale or by the
consumer is released before the requester upgrades.

### Requests and blockers

| Request | Owner | Blocks |
| --- | --- | --- |
| [inkproof-core#1](https://github.com/tentenths/inkproof-core/issues/1): Core Milestone 1 brief | core | scale (Core-dependent parts), consumer-metrics |
| [inkproof-scale#1](https://github.com/tentenths/inkproof-scale/issues/1): Scale Milestone 1 brief | scale | consumer-metrics (scale half) |

### Acceptance criteria

Milestone 1 is done when all of the following hold:

1. **Releases.** The human has released `inkproof-core` and
   `inkproof-scale` at a `v0.1.x` tag, and both Milestone 1 request issues
   have been verified and closed by the coordinator.
2. **Definition of done** holds in every repository: CI passes on `main`,
   no `sorry`, `CHANGELOG.md` and `API.lock` are current.
3. **Theorems.** PROOF-1, PROOF-2, PROOF-3 (Core) and PROOF-5 for linear
   scales and affine transforms (Scale) are proved. Each has a plain-English
   statement (PROC-3) giving its hypotheses and the number types it covers;
   any type that is only tested, not proved, is stated as such (principle 6).
4. **Axioms.** Each library lists its axioms in one file (PROC-1); the only
   project axiom expected is the float-order axiom of FP-2.
5. **Genericity.** Core's algorithms and theorems are stated over arbitrary
   linear orders and monoids (FP-1). CI compiles at least an order-only
   x-type and a non-commutative monoid against them (VAL-1, partial).
6. **Consumer.** `inkproof-consumer-metrics` builds in CI against the
   released tags only, through the public API (no `Internal` modules), and:
   - builds series from irregularly sampled metric data with missing scrapes
     represented explicitly (CORE-2);
   - declares gaps itself, as boundaries or a gap predicate; its own
     gap-detection rule stays in the consumer (CORE-3, principle 1);
   - queries pyramids for several aggregates, including a product such as
     first/max/min/last, and tests the results against a naive fold over
     the raw data;
   - maps aggregated values to screen coordinates with a linear scale and
     inverts them back;
   - keeps golden snapshots of these outputs in its tests.
7. **No workarounds.** The consumer contains no copy or reimplementation of
   library functionality, and no library code mentions metrics, scrapes or
   any other consumer domain concept. Every gap found was filed as a request
   and is resolved or explicitly deferred in this roadmap.

## Later milestones (outline)

Tentative; each will be planned in detail when the previous one closes, and
most need agents that do not exist yet.

- **Milestone 2: complete Core and Scale; Axis.** CORE-4 binning with
  integer, fixed-point and float instances (FP-4), CORE-7 tiling, CORE-8
  indexable series; remaining scale kinds, SCALE-3, SCALE-4, SCALE-6; new
  Axis library (AXIS-1 to AXIS-3, PROOF-12 and PROOF-13 as Should). Node and
  Lasm builds for these libraries (ARCH-3) with differential tests (VAL-6).
  More consumers (candlestick, unsorted scatter) and property-based tests
  (VAL-4).
- **Milestone 3: Viewport and the display list.** Viewport as a pure state
  machine (VIEW-1 to VIEW-4, PROOF-9, PROOF-10). Scene display-list type,
  clipping and raster model (SCENE-1, SCENE-3, SCENE-5, PROOF-7, PROOF-8).
  Needs the human's decision on the raster model (open question).
- **Milestone 4: compilation and the reference rasterizer.** SCENE-2,
  SCENE-4, SCENE-6; PROOF-4, completing the end-to-end column theorem;
  Export to PNG (EXPORT-2); golden display-list snapshots for consumers.
- **Milestone 5: the browser path.** Gpu (GPU-1 to GPU-7, PROOF-6, PROOF-11,
  PROOF-14) and Web (WEB-1 to WEB-5); round-trip and golden-image tests
  (VAL-7, VAL-10, VAL-11); performance targets (PERF-1 to PERF-5).
- **Milestone 6: 1.0.** SVG/PDF export (EXPORT-1), accessibility, theming,
  documentation, the outsider build (VAL-9) and the seismic flagship demo
  (ACC-1 to ACC-10).

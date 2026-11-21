# Prior art

Existing libraries that could serve Inkproof. Each line: what it offers, URL, and whether it is known to work under Lasm (PLAT-3). "Unknown" means nobody has built it under Lasm; it is not a finding against it. Adding a dependency from outside the project is the human's decision (REUSE-1). Checked 2026-11-13: the human's repositories list and Reservoir's front page, which shows only a sample of the registry, so "none found" below means not found there.

## Lean core, Batteries, Mathlib

- Lean core and Std (v4.35.0): `Float`, arrays, `HashMap`, `Std.Time`, `Std.Sat`, `Std.Do` and the standard proof automation. https://github.com/leanprover/lean4. Works under Lasm (the supported toolchain).
- Lean core float model: `Float.Model`, `Float32.Model` and `UnpackedFloat` (Init/Data/Float/Model), a proved model of binary64 and binary32 arithmetic. inkproof-float proves `float_le_linear` and the order and literal facts from it. Consider it before adding any float axiom.
- Lean core `Rat`: `Rat.abs` and its basic lemmas; it has no `abs_add` or `abs_mul` family (v4.35.0 has only `Rat.abs_nonneg`). inkproof-float adds `rat_abs_add_le`, `rat_abs_mul`, `rat_abs_le_iff` and the relative-error lemmas built on them.
- Lean core `Array.binSearch` (Init/Data/Array/BinSearch.lean): binary search with no theorems about it in v4.35.0, so Core's `Series.lowerBoundFrom` is its own.
- Lean core `Nat.toDigits` and `Nat.digitChar` (Init/Data/Repr.lean): most-significant-first digits of a Nat, with only membership and base-case lemmas in v4.35.0 (nothing on length or value). inkproof-axis keeps its own fixed-width `lsDigits` with those proofs.
- Batteries: extra data structures and lemmas (`BinaryHeap`, `RBMap`, list and array lemmas). https://github.com/leanprover-community/batteries. Known to work under Lasm only if a build is confirmed; unknown.
- Mathlib: real analysis, order, algebra and lattice theory for proofs. https://github.com/leanprover-community/mathlib4. Not used (human's decision 2026-11-05); Core reported on it under Lasm, see inkproof-core#2. Proof modules only, never linked into a browser build (CLAUDE.md).

## The human's Lean libraries

- Data formats: lean-json, lean-cbor, protobuf, lean-markdown, lean-regular, lean-css, lean-html and lean-htmx, all at https://github.com/paulbutcher/<name>. Lasm unknown.
- Server stack: lean-routing, lean-middleware, lean-webapp, lean-authentication, lean-jose, lean-telemetry, leanpostgres and the native bindings (leancurl, leanxml2, lean-aws, lean-llmclient, lean-mcp). Server-side or native; not for Lasm.
- leanin: Tokio-style concurrency and work-stealing scheduler, with proofs. https://github.com/paulbutcher/leanin. Native; Lasm unknown. Check before writing any scheduling code.
- lasm-todomvc: Lasm example application, the closest reference for browser structure. https://github.com/paulbutcher/lasm-todomvc. Works under Lasm. Lasm.Web, Lasm.UI and Lasm.JS are not a Lake dependency; they ship in `@lasm/cli`, documented in docs/ of https://github.com/Millillion/lasm.
- lean-random-sampling (v0.1.0): seeded draws over any core `RandomGen`: bounded naturals (`nat`, `nats`), `draws` of any sampler, uniform float strictly in (0, 1) at 2^-53 (`uniform`), normal and skew normal (`normal`, `standardNormal`, `skewNormal`), with theorems for bounds, sizes and definitions; Lean core only. https://github.com/paulbutcher/lean-random-sampling. Lasm unknown (inkproof-demos#68 builds it under Lasm).
- Full list: https://github.com/paulbutcher?tab=repositories&language=lean. Archived or superseded: lean-forms, partition-when.

## Reservoir

- hex: verified computational algebra. https://reservoir.lean-lang.org. Lasm unknown; check for exact linear algebra if Scale needs it.
- ryu: floating point to decimal string. https://reservoir.lean-lang.org. Failed to build on v4.36.0-rc2; Lasm unknown. Relevant to number formatting.
- sampcert, MathFin: differential privacy and financial mathematics. https://reservoir.lean-lang.org. Not relevant.
- Bounded LRU map or cache: none found in Lean core, Std, Batteries, Mathlib (Loogle for "LRU" returns nothing; Batteries has `RBMap`, `BinaryHeap`, no cache), the human's repositories, or Reservoir's search page (checked 2026-11-17). Web's private `Lru` is the only one; it stays in Web and is to be extracted as a non-inkproof library later (#149).
- Plotting, charts, SVG, scales and time series: none found.

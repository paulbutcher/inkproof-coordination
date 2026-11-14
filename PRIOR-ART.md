# Prior art

Existing libraries that could serve Inkproof. Each line: what it offers, URL, and whether it is known to work under Lasm (PLAT-3). "Unknown" means nobody has built it under Lasm; it is not a finding against it. Adding a dependency from outside the project is the human's decision (REUSE-1). Checked 2026-11-13: the human's repositories list and Reservoir's front page, which shows only a sample of the registry, so "none found" below means not found there.

## Lean core, Batteries, Mathlib

- Lean core and Std (v4.35.0): `Float`, arrays, `HashMap`, `Std.Time`, `Std.Sat`, `Std.Do` and the standard proof automation. https://github.com/leanprover/lean4. Works under Lasm (the supported toolchain).
- Lean core float model: `Float.Model`, `Float32.Model` and `UnpackedFloat` (Init/Data/Float/Model), a proved model of binary64 and binary32 arithmetic. Core proves `float_le_linear` from it (inkproof-core#50). Consider it before adding any float axiom.
- Lean core `Rat`: `Rat.abs` and its lemmas, the basis of the relative-error work moved into Core (#46, #58).
- Lean core `Array.binSearch` (Init/Data/Array/BinSearch.lean): binary search with no theorems about it in v4.35.0, so Core's `Series.lowerBoundFrom` is its own.
- Batteries: extra data structures and lemmas (`BinaryHeap`, `RBMap`, list and array lemmas). https://github.com/leanprover-community/batteries. Known to work under Lasm only if a build is confirmed; unknown.
- Mathlib: real analysis, order, algebra and lattice theory for proofs. https://github.com/leanprover-community/mathlib4. Not used (human's decision 2026-11-05); Core reported on it under Lasm, see inkproof-core#2. Proof modules only, never linked into a browser build (CLAUDE.md).

## The human's Lean libraries

- Data formats: lean-json, lean-cbor, protobuf, lean-markdown, lean-regular, lean-css, lean-html and lean-htmx, all at https://github.com/paulbutcher/<name>. Lasm unknown.
- Server stack: lean-routing, lean-middleware, lean-webapp, lean-authentication, lean-jose, lean-telemetry, leanpostgres and the native bindings (leancurl, leanxml2, lean-aws, lean-llmclient, lean-mcp). Server-side or native; not for Lasm.
- leanin: Tokio-style concurrency and work-stealing scheduler, with proofs. https://github.com/paulbutcher/leanin. Native; Lasm unknown. Check before writing any scheduling code.
- lasm-todomvc: Lasm example application, the closest reference for browser structure. https://github.com/paulbutcher/lasm-todomvc. Works under Lasm.
- Full list: https://github.com/paulbutcher?tab=repositories&language=lean. Archived or superseded: lean-forms, partition-when.

## Reservoir

- hex: verified computational algebra. https://reservoir.lean-lang.org. Lasm unknown; check for exact linear algebra if Scale needs it.
- ryu: floating point to decimal string. https://reservoir.lean-lang.org. Failed to build on v4.36.0-rc2; Lasm unknown. Relevant to number formatting.
- sampcert, MathFin: differential privacy and financial mathematics. https://reservoir.lean-lang.org. Not relevant.
- Plotting, charts, SVG, scales and time series: none found.

# CN1.3 fresh-eyes review: the READMEs of backdrop, circle, csg, font, gradient, kiwi, noise, style, textdraw and traer

One general-purpose reviewer read the full diff of all ten READMEs against the
sources, briefed with the task's two bullets and the full text of
`reviews/misreads.md`. Its reply, verbatim:

---

I found no blocking defects but several real ones. The exit grep finds nothing, `check-guides` passes (26 guides) and `check-retired-words check` passes. The ten READMEs are modified and not yet committed. The guards miss the points below, which all come from reading the source.

## 1. Sentences still false against the code

- **textdraw**, "which is what the snippets above pass as `typ.Shaper`." This is now false. The third snippet above it (the gravity overlay) passes `shaper`, which `traer/gio/gravity/main.go` builds from `style.FontFaces()`, not from the theme. Before the edit this sentence said "the application snippets", which was accurate.
- **backdrop**, "every themed window repeats the same `SwitchMap`/`Map` bridge to get `c.WindowBackground` in here." Workbench has 7 `BackdropLayer` functions and only 5 of them read `WindowBackground`. This is also a claim about consumers (see category 4). Saying "a caller writes the bridge itself" would be true and name nobody.
- **style**, "the one intra-tier edge the tier table has to admit by name … permits that single edge". `organisation/scripts/check-layers.sh` sets `ALLOWED_EDGES="style-&gt;font style-&gt;textdraw"`, which is two edges. Low severity; this sentence was not in the diff.
- **textdraw**, "This sizes a clickable row … and vertically centring the item's text in it". The edit left the verb forms mismatched ("sizes … and centring"). The facts match `workbench/todos/list.go`.

Everything else checks out against the source:
- **backdrop:** the themed snippet matches `workbench/iconbrowser/backdrop.go`, and the `Theme.Platform` comment in `theme/theme/theme.go` supports it.
- **circle:** both LICENSE files exist and are MIT; the snippets match `traer/gio/arboretum` and `mvu/example/circles`.
- **font:**
  - There are 15 Roboto packages and none has a test file.
  - Non-test imports are Gio only.
  - The leaves panic on a failed parse.
  - The file sizes give 596 KB and 9.9 MiB, as stated.
- **kiwi:**
  - `gio/go.mod` requires kiwi v0.0.9; the root `go.sum` is empty and its `go.mod` has no require block.
  - `TestSimple0`, `TestEditVars` and the `Benchmark*` names exist.
  - `UpdateStays`, `UpdateVariables` and the unsatisfiable path of `AddConstraint` behave as described.
  - There are 5 `Error` sentinels, 8 struct errors and 12 `Constrainer` methods.
  - `EvaluationError` calls `runtime.Caller(1)`.
  - The default edit strength is STRONG, so the inlined `WithStrength(STRONG)` in the pointer snippet is equivalent.
- **noise:** the regeneration steps match `noise_test.go` (`write_reference_image`, `SourceFile`) and the four embedding `TestImage*` tests.
- **style:** the snippets match `traer/gio/gravity/main.go`.
- **textdraw:** the snippets match `workbench/todos/list.go:141-144` and `workbench/iconbrowser/view.go:305-306`. The `FillLabel` clamp and dimensions analysis and the locale claims match `label.go`, `text.go` and `locale.go`.

## 2. Retired words and Language breaks

- **"role" used bare for a typography role.** DOMAIN's Colour role entry says a bare "role" means a colour role, and the Language term here is "Typography role".
  - textdraw: "a `theme/tokens.TextStyle` role" (line 27), "the theme's TitleLarge role" (line 66), "convert a theme role into one of these" (lines 118–119).
  - style: "the type roles" (line 59), "per type role" (line 98) and "not an MD2 role" (line 113). "Type role" is not the Language term either.
- **"pill" names a different thing.** DOMAIN's Pill entry is the rounded fill behind a selected row. textdraw uses "pill" for `FillLabel`'s text background (lines 12, 56, 121–125) and the diff added another use: "how much space the pill took".
- **`onFill` (textdraw line 56).** This is the retired "on-" family (use "foreground"). The guard passes it because it is the real parameter name in `label.go`, so the code identifier has the same defect.
- **Figurative "carries" and "live" (literal-verb rule):**
  - Added by the diff: backdrop "whose `Platform` observable carries the platform's colour set"; kiwi "the BSD-3-Clause LICENSE carries … copyright", "Nested-module tags carry the directory" and "carries its own arithmetic builders".
  - Untouched: traer "Runnable Gio examples live under `gio/`"; font "The font data carries its own licences"; style "the style carries the font and size".
- **Borderline:** style's "It is the Material Design 2 scale" names Google's scale, not the design system, and the guard passes it. Only change it if the Material row is meant to cover this sense.

## 3. History phrases

None among the + lines. Untouched, low severity:
- csg: "committed to git on November 30, 2011. As an odd coincidence, I am writing this a full 10 years later on Nov 30, 2021."
- traer: "Traer AS3 targeted 31 fps with `Tick(1)`."
- kiwi: "were dropped from nucleic/kiwi's API" (about the upstream library).

## 4. Consumers, reverse dependencies, pins, tags, census

- backdrop: "every themed window repeats the same … bridge" is a statement about callers (see 1).
- kiwi: "Nested-module tags carry the directory as a prefix (`gio/`)." This describes tagging, not what the module ships, and can be deleted.
- No consumer is named anywhere, and no census, pin or `v0.0.6` remains.

## 5. `file:line` references

None remain. kiwi now names `TestSimple0`, `TestEditVars`, `Solver.String` and `Quadrilateral`.

## 6. Plan, task or decision numbers and unmaintained counts

- No task or decision IDs, and no G-F or C1.2 style references, remain.
- kiwi's "(`BenchmarkQuadBuild`) allocates hundreds of times" is a measured magnitude that no test asserts; "allocates on every constraint" would avoid it.
- All ten keep "the plan root's `AGENTS.md`". That appears to be the settled CN1.1 wording, but it does name "the plan".

**Verdict:** the exit criteria pass, but fix three things before committing: the false `typ.Shaper` sentence in textdraw, the "every themed window" claim in backdrop, and the bare-"role" and "pill" breaks in textdraw and style. The figurative "carries" in the hunks you added is a smaller fix.

---

## Disposition

1. textdraw `typ.Shaper` sentence: fixed, it now says "the first two snippets above".
2. backdrop "every themed window": fixed, it now says "a themed caller writes the `SwitchMap`/`Map` bridge … itself".
3. style intra-tier edge count: fixed, it now says two edges, matching `ALLOWED_EDGES` in `scripts/check-layers.sh`.
4. textdraw verb mismatch: fixed ("sizes … and vertically centres").
5. Bare "role": fixed. textdraw and style now say "typography role" in every place listed, and "not an MD2 role" became "not an MD2 style".
6. "pill": fixed. textdraw now says "rounded background" or "background" for `FillLabel`'s shape in every place.
7. `onFill`: not changed in the README, because it is the real parameter name. The code identifier is filed as finding 1227 in the review pool.
8. Figurative verbs: fixed. backdrop now says "emits", kiwi says "holds" and "has", traer says "are under", font says "has its own licences" and style says "holds". The kiwi tag sentence is deleted (see 11).
9. "Material Design 2 scale": not changed. It names Google's scale that the module implements, not this design system, and the guard passes it.
10. History: the csg coincidence sentence is deleted. The csg attribution date, traer's AS3 frame-rate note and kiwi's "dropped from nucleic/kiwi's API" stay. They describe the upstream sources the code was ported from, and the AS3 note explains what `Tick`'s argument means.
11. kiwi nested-module tag sentence: deleted.
12. kiwi "allocates hundreds of times": fixed, it now says "allocates".
13. "the plan root's `AGENTS.md`": not changed. This is the settled wording shared with the other guides.

# CN1.4 fresh-eyes review: the READMEs of ivg, markdown, mvu, seen and svg

Reviewer: one general-purpose agent, briefed with the task heading and its two bullets verbatim, the guide rules, the full text of `reviews/misreads.md`, and the diff of the five READMEs. The `+` lines of the diff were pasted complete; some long deleted passages were abbreviated with "[...]", and the reviewer was pointed at the working-tree READMEs to read them whole. It read the sources read-only.

## The reply, verbatim

I found 9 problems: 2 hit all three of ivg, seen and svg, and the other 7 are single sentences. Each finding gives the README, the quoted text, the evidence and a fix.

**1. Sentences still false against the code**

1. **ivg, seen and svg: "The build and test commands are reached from that file."** This is false. `organisation/AGENTS.md` (87 lines) contains no build or test command and links to no per-repo file. Each repo's `AGENTS.md` is 11 lines of sync text with no commands either. The sentence also uses the retired word "reach" for a document. **Fix:** delete the sentence in all three READMEs.

2. **svg: "Radial gradients are stubbed to their first stop (a `TODO` in `paintPattern` says so)."** The TODO is at `driver/gio/driver.go` line 164, inside `paintGradient`. `paintPattern` is a separate function at line 149. **Fix:** change `paintPattern` to `paintGradient`.

3. **seen: "In the root module `shader`, `light`, `face`, `transform` and `layer/zsort` have no tests either."** The list reads as complete but is not. These packages also have no `_test.go`: `animation`, `canvas`, `drag`, `intensity`, `layer`, `layer/backdrop`, `obj`, `viewport` and `zoom`. **Fix:** say "Among the untested root packages are …", or list all fourteen.

4. **seen: "The provenance is stated in this README and in comments of the `mocap` package and the `mocap` and `multipleangles` examples."** The sentence follows a claim about csg.js, but those comments credit seen.js only. `grep -i 'csg\.js\|evanw'` over the Go files finds nothing. Inside this README, the `solid` row links to `vibrantgio/csg`, not csg.js. **Fix:** "seen.js is credited in this README and in comments of the `mocap` package and the `mocap` and `multipleangles` examples; csg.js is credited nowhere."

5. **seen: "adds no dependency beyond Gio and the `golang.org/x/image` Gio already requires", and "`context/gio` nested, which adds gioui.org v0.10.2".** The library packages do import only Gio and `x/image`. But the `context/gio` go.mod also requires `vibrantgio/noise`, `zserge/lorca` and `golang.org/x/exp`, which its examples use (noisysphere, noisywavepatch, combinedsolid). **Fix:** "the `context/gio` library packages add only gioui.org v0.10.2 and `golang.org/x/image`; the module's examples also require noise, lorca and `x/exp`."

6. **mvu: "Every entry point compiles on every platform and does nothing away from macOS".** Two entry points do something there:
   - `download_stub.go`: `Download` answers `Downloaded{Err: "nothing is in the cloud on this platform"}`.
   - `scrollbars_stub.go`: `ScrollBars` returns `ScrollBarsWhenScrolling`.

   **Fix:** "…and has no native effect away from macOS, where `Download` answers that nothing is in the cloud and `ScrollBars` reports `ScrollBarsWhenScrolling`."

7. **markdown: "a name that is not in the styles folder" (minor).** `styleFor` falls back on `!Known(name)`, which covers embedded and loaded styles alike, and embedded styles are not in a folder. **Fix:** "a name that resolves to no style".

Checked and true: the ivg package table (all exported names, `render.Gradient` implementing the five `GradientConfig` methods, `Widget` centring, the examples) and the markdown import lines. Also true:
- markdown: `FromTokens` with three parameters, the fields `Wear` writes, the seam colour, the panics.
- mvu: every name in the desktop row.
- seen: the shader names, `face` and `bsp` exports, the `gio.Widget` affine, the `FitCenter` variadic, eleven Go examples, six with CoffeeScript beside them and seven without Go, the `FontFaces` package-var init, `MulB` and `Clip`.
- svg: the winding claims, `linearGradientF`/`radialGradientF`, the `.gitignore` quote, the four tested packages, the modules and the dependencies.

**2. Retired words**

8. **markdown: "reach" used for things other than code or import reachability:**
   - "the code reaches the fence's edge once scrolled" → "the code runs to the fence's edge"
   - "what every style's colours reach on the background" → "the contrast every style's colours measure on the background"
   - "the dark member that same name reaches" → "the dark member that same name resolves to"
   - "nothing reaches around it" → "nothing bypasses it"

   "no chroma type reaches its exported API" and mvu's "reached through cgo" are about type and code reachability, so they are fine.

9. **markdown: "roles of its own" and the "content role" table header.** These are bare "role" for things that include the heading scale, which is not a colour. **Fix:** "names of its own" and "content field". "Code role", "body role" and "display roles" are qualified typography roles; leave them unless the rule covers them too.

No "widget", "variant", fill or foreground misuse in any of the five.

**3. History phrases**

No finding in the diff hunks. Two borderline phrases in the whole markdown README: "One caveat survives for applications building their own shaper" (fix: "One caveat applies to applications…"), and "where the values are the chrome's today. … costs nothing now and is what lets the reading surface move later" (fix: drop "today", "now" and "later").

**4. Consumers, pins, tags, census**

No finding. The remaining "consumer" uses in mvu mean subscribers of an observable. The `vX.Y.Z` tag patterns name no version. The pins in mvu's Requirements are its own dependencies.

**5. `file:line` references**

No finding. One plain file path remains, mvu's "`desktop/doc.go`". It has no line number, so it is acceptable, though naming the package documentation alone would be enough.

**6. Plan, task or decision numbers**

No finding.

Outside the five files: `/Users/rene/code/w/vibrantgio/ivg/raster/gio/example/README.md` still names `github.com/reactivego/ivg` and old examples (PlayArrow). That is out of scope for this task but could go to the review pool.

## Disposition

1. Not changed. The sentence is the wording CN1.2 settled for every README and CN1.3 used in ten more: `organisation/AGENTS.md` sends the reader to `mdplan next PLAN.md`, whose packet carries the standing rules with the build and test commands, so the commands are reached from that file, not in it. "Reach" here means a document being reachable, the kept sense the guard's exclusion allows (`check-retired-words.sh check` prints OK).
2. Fixed: `paintGradient`.
3. Fixed: the sentence lists all fourteen untested root packages.
4. Fixed with the reviewer's wording, "csg.js is credited nowhere in the repository".
5. Fixed: the library packages add only Gio and `golang.org/x/image`; the module's examples also require noise, lorca and `golang.org/x/exp`.
6. Fixed with the reviewer's wording.
7. Fixed: "a name that resolves to no style".
8. Fixed, all four, with the reviewer's wording.
9. Fixed: "fields of its own", and the table header reads "what the content names". While there, "the same step a fence is filled with" became "the fill a fence takes" (step as a source of colour is retired).
History, borderline: fixed. "applies", and "today", "now" and "later" are dropped.
Outside the five files: filed as finding 1231.

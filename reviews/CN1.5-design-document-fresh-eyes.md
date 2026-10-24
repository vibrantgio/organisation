# CN1.5 fresh-eyes review: the design repository's DESIGN.md and conventions

One general-purpose reviewer, 2026-10-24, briefed with the task heading and
both bullets verbatim, the full new text of `design/DESIGN.md`, the diff of
every other changed file, the instruction to read each sentence against its
source, and the full text of `reviews/misreads.md`. Its reply follows
verbatim, then the disposition of each point.

## The reviewer's reply

I found 13 defects. Four are false against the code and serious: the hover rule, the accent rows, motion and reduced motion, and the subscription lifecycle. The ruled-word sweep of DESIGN.md came back clean. I edited nothing.

**Defects**

1. "Hover and press are the measured overlays … only where the platform shows them: a toolbar button does, a push button and a list row do not." This is false.
   - `PressOverlay` is measured off a push button held down (theme/tokens/platform.go:53, :404-406).
   - The push button and the ghost take both overlays: components/button/button.go:770-781, `controlface.FormFill` (components/internal/controlface/controlface.go:136-147), and design/styles.css:452-455.
   - The DOMAIN Fill entry agrees: "Transient states lay the platform's hover and press overlays over the fill" (DOMAIN.md:517).
   - platform.go:411-414 says no capture shows a push button or list row exempt from hover.
   - The only support for the claim is controlface.go:116-120, which contradicts button.go.

2. "`ControlAccent` is the accent the user chose, and four more names follow it…" `WithAccent` rebuilds seven rows besides `ControlAccent`, not four (platform.go:1003-1009). The other three are `SidebarSelection`, `SidebarSelectionUnemphasizedLabel` and `DefaultButtonFill`.
   - "On macOS `theme/system` asks AppKit for each of those names" holds only for the four AppKit names.
   - The three measured rows are skipped on macOS (theme/system/nscolors_darwin.go:55) and stay at the recorded blue unless a theme colour is pinned (platform_darwin.go:10-15, system.go:308-310).

3. "Components take durations and springs from the theme, which is what makes reduced motion free. One exception: `effects/springbutton` steps its spring at a fixed 60 steps per second…" This is wrong three ways:
   - The springs in `effects/spring` and `effects/springbutton` are hard-coded and never read the theme (effects/spring/spring.go:70-80; effects/springbutton/springbutton.go:100-104).
   - springbutton does not read `Motion` at all, so reduced motion never reaches it.
   - It takes one step per frame with dt = 1/60, not 60 steps per second. On a 120 Hz screen it runs twice as fast (springbutton.go:43-49, :108-111).

4. Accessibility: "components driven by springs read the zeros as the signal to snap." This is the contract stated in theme/tokens/motion.go:134-137. springbutton does not do it, so the sentence is false for the one spring component the guide names.

5. "It ends when the window closes (`app.DestroyEvent`) or the observable completes or errors." This is false. mvu/window.go:222-228 ignores the layers' done and err, and the loop ends only on `DestroyEvent` (window.go:199-200, :243-255).

6. "One edge inside a tier is admitted: `style` imports `font` and `textdraw`." That is two edges (organisation/scripts/check-layers.sh:110, `ALLOWED_EDGES="style->font style->textdraw"`).

7. The spine diagram `… effects → patterns → markdown` puts markdown above patterns, but they share tier 4. markdown imports neither patterns nor effects (markdown/go.mod:8-11).

8. "Forty-four fields are AppKit names. They were read off AppKit…" `FindHighlight` is an AppKit name, but its value is measured off Mail. It is pinned against Mail in theme/tokens/platform.go:171-176 and platform_test.go:263-269, and skipped by the live reader in nscolors_darwin.go:59-61. The 44/28 count itself is correct: 28 fields carry `appkit:"-"`. Also, "the alternating row" is read off AppKit's array and only confirmed against a capture (platform.go:33-34).

9. "Increased contrast follows the platform: … the platform reports the colours it paints under it." This is true only on macOS (system.go:300-303). The other platforms have no such branch.

10. "a lint fails the build on colour literals" (Non-goals) and "the lints fail the build" (Design principles). The lints are `_test.go` files that fail `go test`, which DESIGN.md itself says correctly under Typography (components/internal/lint/*_test.go).

11. "A control is operated anywhere in the area it draws, and nothing is added around it." This sentence brings the deleted pointer-target claim back. It is also false for the checkbox: the 16 dp square takes a 22 dp row as its target (density.go:139-145). The chip's dismiss mark registers a 24 dp hit rectangle. I suggest deleting the sentence with the pointer-target floor, as ruled.

12. "the `organisation` repository with the plan and the agent guide" names the plan, against "no plan named". It is borderline.

13. Minor wording:
   - "a flat tinted scrim" uses "tint" as a colour, a retired sense. "a flat scrim of one colour" would do.
   - "Nothing is generated and no foreground is computed" sits uneasily beside the `BestOn` paragraph, which chooses a foreground.
   - "its colours, control heights, text sizes and shapes are the platform's, measured off the platform's own windows" overclaims: Compact 19 is published, and Compact field 21 and checkbox row 17 are derived (density.go:21-31). DOMAIN says "measured where a capture holds them, published otherwise". The same applies to the principle "read off the platform's windows".

**Other changed files**

14. conventions.md, Token families: "`--platform-<name>`, one per semantic colour the platform names — … `--platform-sidebar-material`, `--platform-card-fill`". Both are fills the platform does not name (`appkit:"-"`).

15. conventions.md calls the dismissable dialog a "**pane**", which collides with DOMAIN's Pane, a chrome pattern (DOMAIN.md:859). dialog.html:148 uses the same word. This text predates the task but stays in a file the task corrected.

16. The new conventions say only `.dialog`, `.popover` and `.toast` cast a shadow, using `--floating-shadow-step`. Three places still contradict that:
   - readme.md:61 (from readme.go): "`--shadow-<level>` … the cue a floating transient carries (menus, dialogs, tooltips)".
   - readme.md:209: "the cue a floating transient carries over its fill".
   - design/components/cards.html:161: "a float adds `box-shadow: var(--shadow-N)`".

    Nothing in the sheet casts `--shadow-1` to `--shadow-3` on a float.

17. Ruled words left in readme.md (from readme.go, outside the named edits):
   - "reach" (readme.md:69) in the description of `--toolbar-control-shadow-*`.
   - "floor" for a height (readme.md:67, :152). The retired-words table keeps "floor" for contrast floors only.

18. In llms.txt:
   - DESIGN.md is still "the full architecture rationale" (:18, :1620). It is now a description of the system as shipped.
   - "Filled/Tonal/Ghost variants" (:511) breaks the Variant entry. Those are emphasis, not variants.
   - "floating surface on the window's own surface" reads poorly; "on the backdrop" says it plainly.

19. components/chip/chip_bench_test.go: "so CI can gate on per-frame allocation regressions (>5% threshold)". No CI exists ("There is no per-repo CI", check-layers.sh:77-79).

**Code vs Language (planned work elsewhere, not defects in the guide)**

- Typography: `DefaultTypography` uses Material sizes (57/45/36 … 11, theme/tokens/typography.go:483-503), not the platform's.
- Levels: `tokens.ElevationScale` has six levels with Level1 = 1 dp (theme/tokens/elevation.go:19-32). The export writes it as `--shadow-1..3` rgba shadows (styles.css:196-198), and readme.md:200-206 says "Six levels … The levels stop at 3". DOMAIN has five levels, and a raised thing casts no shadow.
- Floating shadow: DOMAIN and DESIGN put tooltips and toasts under the platform's shadow. `components/tooltip` and `.tooltip` draw none; toasts get it only through `patterns/notifications`.
- Density: DOMAIN says "All are measured into the reference, not derived". The code derives Compact field 21 and checkbox row 17, and takes Compact control 19 from the published value.
- Hover: controlface.go:116-120 says only a toolbar button tints, while button.go tints every variant. The code disagrees with itself here, which is the root of defect 1.
- Accent on macOS: the three measured accent rows ignore the user's accent unless a theme colour is pinned.
- theme/tokens/platform.go:22 says "Twenty-two fields are not AppKit's"; 28 carry the tag. This is a stale comment.

**Proportions:** 419 lines against 1044 is about right. A little more could go: Threading points 1-3 repeat patterns 1-2, and Design principles restate each section. Cutting both to a few lines would save about 40 lines and lose nothing.

**Verdict:** not ready. Fix defects 1-6 and 8 in DESIGN.md, and 14 and 16 in conventions and readme.go, before the commit. The ADR, history, CIELAB, `--color-`, 44 dp, plane, knob, seed, tonal, touch-first, reach and floor greps on DESIGN.md come back clean.

## Dispositions

1. Fixed, re-read from `components/button/button.go:770-781` and `controlface.FormFill`: hover and press are laid over a control's own fill, or over the surface beneath where it has none. The contradicting doc of `controlface.Fill` is pool finding 1233.
2. Fixed: seven fields follow the accent, four AppKit names and three measured fills; macOS asks AppKit for the four; `WithAccent` rebuilds all seven on Windows and Linux and wherever a theme colour is chosen; on macOS with none chosen the three fills keep their recorded values. That behaviour is pool finding 1238.
3. Fixed: duration-driven components take the theme's Motion; springbutton has its own constants, one step per frame at 1/60 s, and does not read Motion. Pool finding 1234.
4. Fixed: the spring clause is deleted from Accessibility.
5. Fixed, re-read from `mvu/window.go:222-255`: the subscription ends when the window closes; a layer completing or erroring does not end it.
6. Fixed: two edges inside tier 0.
7. Fixed: the spine ends `patterns, markdown`.
8. Fixed: forty-three read off AppKit, `FindHighlight` measured off Mail's find bar; the alternating row read off AppKit's array and confirmed against a capture.
9. Fixed: increased contrast follows the platform on macOS.
10. Fixed: the lints fail `go test`, in both places.
11. Fixed: the sentence is deleted.
12. Fixed: the organisation repository is named with the agent guide and the gate scripts.
13. Fixed all three: "a flat scrim of one colour"; "No colour is generated from another"; "measured … where a capture holds them and taken from its published guidelines otherwise", in the opening and in the principle.
14. Fixed: the colour family is one variable per colour the platform names and one per fill it draws without naming, measured.
15. Not fixed: the dismissable dialog's name is used by `dialog.html` and `patterns/dialog`; renaming it is a Language question. Pool finding 1236.
16. Fixed in `theme/export/readme.go` (both sentences), regenerated into `design/readme.md`. `cards.html:161` is a hand-authored page the mirror compares (`design/mirror/g22_test.go`), and the section it heads demonstrates the `--shadow-N` floats this task leaves to CN2.3; pool finding 1237 rather than a page edit in a task with no pixel moves.
17. Fixed in `readme.go`: "how far it is drawn around the control"; "the least width"; "least height". The variable name `--toolbar-control-shadow-reach` stays: it is the exported name.
18. Fixed the first and third: DESIGN.md "describes the system's architecture" (both places); "a floating surface on the backdrop". The second ("Filled/Tonal/Ghost variants") is outside this task's diff: pool finding 1239.
19. Fixed: the comment says the allocations are reported and names no CI.

Code vs Language: typography sizes are CO1.1, the six-level `ElevationScale` is CN2.3; the floating tooltip without a shadow is pool finding 1240; the derived Compact heights against the Density entry are pool finding 1241; the hover contradiction is 1233; the macOS accent rows are 1238; the stale platform.go count is 1235.

Proportions: Threading points 1–3, which repeated patterns 1 and 2, are folded into one paragraph; the principles stay as the one-screen summary.

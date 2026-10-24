# CN1.1 fresh-eyes review: the workbench guide, its README and the organisation README

A general-purpose reviewer was briefed with the task heading and both bullets, the full `git diff -U1` of every changed text file in workbench and organisation, and the full text of `reviews/misreads.md`. Its reply follows verbatim, then the disposition of each point.

## The reviewer's reply

I found no false claim that matters and no history phrases or plan numbers in the added text. There are five small precision problems and one borderline word, all fixable in one line each.

**False or misleading against the code**

1. **llms.txt line 266 (minor).** "LiveTheme's options choose the theme colour — see §The brand." LiveTheme has three options, not one: `WithThemeColor`, `WithA11ySource` and `WithTypography` (theme/system/system.go:228, 239, 250). The sentence implies the theme colour is all they choose. Suggested fix: "one of LiveTheme's options chooses the theme colour".

2. **llms.txt, Density table (minor).** The toolbar column (36 dp) sits under the "floor:" heading, but the toolbar control height is not a floor. density.go:107 says "toolbar control 36 36 pinned to ToolbarControlHeight". The prose above the table correctly says only the control and field heights are floors, so the table contradicts its own paragraph.

3. **llms.txt, Density, pointer-target paragraph (minor).** It still says "what it draws at Density.ControlHeight is what a pointer has to land on", then two lines later "the 20 dp it draws". Compact's ControlHeight is 19 and it draws 20 (density.go:59 and 67). The first half should say what the control draws, not "at Density.ControlHeight".

4. **llms.txt, opening paragraph (minor).** "Every repository's AGENTS.md carries a managed block pointing to … organisation/AGENTS.md". sync-agents.sh writes the block into every repo except organisation (lines 2 and 6), and organisation/README.md correctly says "Every sibling's". The guide should say the same.

5. **llms.txt, vaultview, twice (minor).** "the tree rail as its pane and the outline and backlinks as its Inspector, both filled with the chrome material behind a Separator seam" (window-structure paragraph), and "the rail and the Inspector filled with the chrome material behind a Separator seam" (vaultview section). Both regions do use the chrome material: `chromeSurface` returns `pane.Surface`, which is `SidebarMaterial` (patterns/pane/pane.go:189). But only the Inspector stands behind a Separator seam (vaultview/aside.go:467). The rail is a pane with its own rim. frame.go:214-216 says "the pane's own hairline leading, the flush column's plain seam trailing".

6. **organisation/README.md, check-no-workspace bullet (minor).** The bullet was edited, but its last sentence still reads "It also fails on a `replace` directive in any member." `scripts/retiredwords` is not a member of `go.work`. The script's header now says "any module" and does check every go.mod, scripts/retiredwords included.

Everything else I checked holds against the source:
- **Module census:** 41 `go.work` members plus 1 makes 42 go.mod files. All 42 declare Go 1.25.1. The eleven nested modules all carry prefixed tags, and organisation has no root go.mod.
- **scripts/:** twelve scripts. `design-project.txt` is read by push-design.sh:20, and check-retired-words.sh:289 runs `scripts/retiredwords`.
- **Fonts:** font/roboto/LICENSE is Apache 2.0; the robotomono, jetbrainsmono, notosansmono and notocoloremoji directories carry OFL.txt.
- **Theme and markdown calls:** `window.New(w, theme)` takes no options (window.go:56). `brand.Kept().Options()` exists (brand.go:265, 302). `markdown.FromTokens(p, typo, standsOn)` takes three parameters (style.go:475).
- **Icons:** the 15 names match components/icons/marks/, square-and-arrow-up included.
- **Density figures:** 24/27/36, 19/21/36, drawn 24/28 and 20/24 all match density.go.
- **todos:** `PaletteFrom` and addaction.go match the text.
- **vaultview:** `shell.PaneWindow`, `desktop.InsetTop`, `Flatten(UnderPageBackground, WindowBackground)` and `shell.TwoColumn`'s Inspector are all in the code.
- **iconbrowser:** uses `input.SearchField`, shows the marks above the Material grid, and is its own module.
- **sk150:** the files named, the Monitor/Presets/Device tabs, the `demo` argument, the M0–M9 argument and the serial Modbus RTU link all match.
- **Launch behaviour:** matches launch.go:35-40.
- **New guide list entries:** components, gallery/inventory and palette, imagecolor and splitter all match their package docs.
- **Stack table:** the layer order matches the go.mod dependencies (theme → components → effects → patterns).
- **Site Docs:** six tabs (sitedocs/pages.go:47). workbench has no tags.

**Retired words in added text**

7. **Borderline, llms.txt Density and Typography.** "floor" is used for a minimum height: "The control and field heights are floors", "max(floor, …)", "its 24 dp floor". The retired sense is an elevation level, and the table's kept sense is contrast floors only, so a height floor is a sense the table neither retires nor keeps. density.go uses the same word, and the old text already had "ControlHeight is a FLOOR". This is a question for the orchestrator, not a defect.
   - These are not hits: "seeding mirrorTokens' first cell" (filling a cell, not the colour sense), the sk150 Modbus "register map", "Material Design icons" (a kept sense), and "launch button" (not "launcher").

**History phrases:** none in added text. Two things that look like hits are not:
- "the newest commit on the default branch since workbench carries no tags" means "because", but it can be read as a time. "because" would be safer.
- Line 890's "The platform's window controls now stand inside your layout" is unchanged context, and "now" there means "once you take the strip".

**Plan, task or decision numbers:** none in added text. The diff removes ADR-006, B3.3, G-F3 and G0E.3. scripts/design-project.txt still contains "by E0.3", but that file is only linked, not added text.

**Conflicts with the Language (DOMAIN.md):** none. The density prose agrees with the Density entry, and the workbench text agrees with the Workbench entry.

**Missing corrections:** none of the requested items is missing. The exit grep finds nothing (exit status 1), the doubled AGENTS.md paragraph is now one, and the line counts are deleted.

**The worker's golden claim:** mostly true but understated. In both light-window.png and dark-window.png, the large differences fall in the Site Docs blurb line (x 680–949, y 320–340). Four other pixels differ by up to 5 levels, at (734,590), (755,590), (105,793) and (102,796), not the stated 1–2. That is harmless. The task's "No pixel moves" can't hold anyway, because the required apps.go text change moves the blurb.

**Verdict:** accurate and clean on history, numbers and the Language; fix the five precision points (1–5) and the README's "any member", and have the orchestrator rule on "floor" for heights.

## Disposition

1. Fixed: "One of LiveTheme's options chooses the theme colour".
2. Fixed: the table heads its first three columns "minimum", and a sentence says the toolbar control height is exact.
3. Fixed: "what it draws is what a pointer has to land on".
4. Fixed: "Every repository's AGENTS.md but the organisation repository's own carries a managed block".
5. Fixed in both places: the rail is a pane inside its own rim, and the Inspector is behind a Separator seam.
6. Fixed: "in any module".
7. Avoided, so no ruling is needed: the added text says "minimum" for a height, which is not a Language term, and "floor" stays only where it is unchanged context. density.go's own wording is code and is left alone. "since" became "because".
Golden note: accepted. The other differing pixels are antialiasing (at most 5 levels) beside the moved text. The apps.go text change is the one the task orders, and the commit body says the goldens were regenerated.

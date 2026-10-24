# CN1.2 fresh-eyes review: the theme, components, patterns and effects READMEs

One general-purpose reviewer, briefed with the task heading and its two
bullets, the diff of the four READMEs and `components/coordination/types.go`,
the guide rules, and the full text of `reviews/misreads.md`. Its reply,
verbatim, then the disposition of each point.

## The reply

1. **False: theme/README.md:26–28.** The README says "`system`, … `export`, … `brand` … import no UI toolkit." All three import `tokens`, and `tokens` imports `gioui.org/font` and `gioui.org/text` (the `tokens` import list from `go list`; `go list -deps` finds 14 gioui.org packages under each). The `theme` package also imports `tokens` and is in neither list. The statement holds only for direct imports, so the sentence needs to say "directly". The author's openpanel note is correct: `system/openpanel` pulls in no gioui.org package, so naming only tokens and typeset is right.

2. **Conflicts with the Language: theme/README.md:74.** The README says "`ElevationScale`, the shadow depth each of the six levels casts". DOMAIN.md:690 (### Level) names five levels: backdrop, chrome, content, raised, floating. `ElevationScale` (theme/tokens/elevation.go:16–23) has six fields (`Backdrop`, `Chrome`, `Level0`–`Level3`), which do not map onto those levels.

3. **False: theme/README.md:87.** The README says "`cmd/a11y-check` | Prints the OS accessibility preferences once per second". theme/cmd/a11y-check/main.go:19 subscribes to `a11y.Live(time.Second)`, which polls every second but emits only on change, so the command prints at start and on each change. The doc comment on main.go:1 repeats the error.

4. **Source contradicts itself, README follows `Live`: theme/system/system.go:73–75.** The package doc still says "Keep the interval at the intended one second; the OS caches these values and will not report a change much sooner". That contradicts `Live`'s doc at system.go:162 (100–250 ms). README lines 114–115 are correct against `Live`. The package doc was left unchanged.

5. **False, unchanged: components/README.md:134–135.** The README says "`theme/window` supplies one that tracks the OS appearance live." `system.LiveTheme` produces the live stream; `window.New` only binds a stream it is given (theme/README.md:14–17, theme/window).

6. **False, unchanged: effects/README.md:6–7.** The README says "effects is where … the ring around a focused control" is drawn. No effects package draws a focus ring: `glow` is only `Spread` and nothing in it mentions focus, and the focus ring belongs to components (components/list `FocusRing`, button).

7. **Incomplete: components/README.md "Where it sits".**
   - It lists from theme only "the `theme` and `tokens` contract". components also imports `theme/color` and `theme/typeset` (`go list -f '{{.Imports}}' ./...`).
   - `font` is imported only by components/paragraph/paragraph_test.go (`font/notocoloremoji`). go.mod requires it, so naming it is defensible, but no library code imports it.

8. **Imprecise: components/README.md:81.** The README says "`Set` is the set for a given platform". `Set` is a struct type; `New(goos string) *Set` (components/icons, line 384) builds the set for a platform.

9. **Retired words.**
   - patterns/README.md, Usage overlay snippet: `modalObs` and `modalW`. Retired row "modal | the pattern that asks a decision → dialog"; the variable holds the dialog stream.
   - Borderline, unchanged: patterns/README.md "The status role is `components/toast`'s". Bare "role" means colour role only; toast's type is `Status`.
   - Borderline, unchanged: the table row says "the header surfaces the click's purpose through `OnSort`". "Purpose" is the component property word.
   - Borderline: theme/README.md:79 "macOS with cgo has a panel". The kept sense is "open panel"; say "an open panel".
   - Borderline: effects/README.md:56 "told by its step of fill alone". The retired row "ramp, step, tone | a source of colour" may apply. patterns' card row, "a small step from the surface it is in", matches DOMAIN's Level table and is fine.

10. **History phrases.** None of the clear kind ("formerly", "now", "no longer", "the old", a removal version) are left. Borderline uses of "still":
    - effects/README.md:215, heading "Frame counting is still frame counting."
    - effects/README.md:226, "its press spring still runs at full amplitude".
    - theme/README.md:122, code comment "dark mode still live".

    All three read as "nonetheless", but they match the brief's word list.

11. **Consumers named.** No in-org consumer or reverse dependency is named. One borderline case: theme/README.md:86 "the project layout claude.ai/design reads". That is an external tool named as the export's reader.

12. **Plan or decision numbers.** None in the four READMEs or the types.go hunk. Outside the diff, components/chip/chip.go's `Chip` comment cites "ADR-003" (code comment, out of scope).

13. **Exit grep and author notes.** The CN1.2 exit grep finds nothing in the four READMEs. "Reached from `organisation/AGENTS.md`" is accurate: that file routes to `mdplan next PLAN.md`, and PLAN.md:52–53 holds the `-golden.update` ordering rule. All patterns static signatures, the shell, pane and splitter identifiers, the patterns imports, and the effects `depth`, `transition`, `blur`, `spring`, `motion` and `conductor` identifiers match the source.

## Dispositions

1. Fixed. The paragraph now says `tokens` and `typeset` import Gio themselves, the other packages import no Gio package directly, and those that use `tokens` reach Gio's text packages through it.
2. Fixed in the README: the row says "the shadow depths in dp from the backdrop up" and counts no levels. The code's six fields against the Language's five levels is filed as finding 1224.
3. Fixed: the row says the command polls once a second and prints at start and on each change. The doc comment in `cmd/a11y-check/main.go` is filed as finding 1225.
4. Not this task's file; filed as finding 1226.
5. Fixed: `theme/system.LiveTheme` supplies the live stream.
6. Fixed: the focus ring is dropped from the effects introduction.
7. Fixed: the paragraph names `color` and `typeset` among theme's packages, and says the module requires font, which the task asks the tier paragraph to name.
8. Fixed: `New` builds the set for a given platform.
9. `modalObs`/`modalW` fixed to `dialogObs`/`dialogW`; "status role" fixed to "status"; "the click's purpose" fixed to "a sort click"; "a panel" fixed to "an open panel". "Step of fill" kept: it is the Level entry's own phrase for a raised thing, not a source of colour.
10. Fixed: the heading reads "Animations count frames, not time.", "still runs" reads "runs", and the snippet comment reads "dark mode stays live".
11. Kept: claude.ai/design is the external format the export writes for, not an importer of the module.
12. Nothing to fix in the guides; the code comment is out of scope.
13. No action.

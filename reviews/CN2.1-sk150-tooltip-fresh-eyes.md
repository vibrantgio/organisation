# CN2.1 fresh-eyes review

Task: "CN2.1: Components' list moves the available room through its rows and sk150's hint cells are the library's tooltip". One general-purpose reviewer was briefed with the task's two bullets, the full diffs of components and workbench, the Tooltip and Attachment entries, the hint cell captured before and after the tooltip's delay, and the full text of reviews/misreads.md.

## The reviewer's reply, verbatim

**Verdict: meets, with one gap in the ruling and two weak Exit checks.** The offset arithmetic, the arbiter, focus detection and the deletion are correct. The tests pass: `components/list`, `components/tooltip` and `workbench/sk150` are green, and gofmt is clean on the touched packages. I did not run the guard, the layers gate or the other workbench modules.

**1. Ruling sentences and Exit clauses not met**

- **"The Tooltip entry's wrapping applies to these cells; the copy's one-line bubble goes with it."** The cells use the library tooltip, but the library does not wrap yet. `drawSurface` in `components/tooltip/tooltip.go` sets the text with `maxLines` 1, caps it at 3/4 of the available room and elides it in the middle. CO1.16 brings the wrapping later.
  - In the hovered capture the text is about 435px for 89 characters, roughly 4.9px per character.
  - The `ohph` and `ohpm` hints are 118 characters, about 577px. The cap in a 720 window is 540px.
  - So in the preset editor those two hints most likely appear shortened with an ellipsis. That breaks the Tooltip entry ("never shortened with an ellipsis"), and the deleted copy drew them whole up to 640dp. This is an estimate from the arithmetic; I rendered no preset hint.
  - This lasts until CO1.16 lands. Either accept it as an interim state or move CO1.16 ahead.
- **Exit `grep -rn 'tip\.' workbench/sk150/*.go`** returns 19 hits, all of them `tooltip.` (for example `hint_test.go:76`, `emptystate_test.go:324`). Read literally, the command does not come back empty. What it is meant to check holds: no `tips`, `newTips`, `tipDelay`, `bubble` or `tp.winW` remains. The only `winW` hits are the unrelated window-size constant declared in `main.go:37`, also used by several tests. The Exit command itself is wrong.
- **Exit "shows the library tooltip below the cell".** `hint_test.go:83` checks `surface.Min.Y < label.Max.Y`, which is the label's foot, not the cell's. The cell's switch is taller than the label, so a surface overlapping the bottom of the cell would still pass. The capture shows the right result: the tooltip top is at about y 179, below the PPT row. The assertion is just weaker than the clause.
- **Exit "a scrolled row's tooltip".** `list/surfaces_test.go` scrolls by whole rows only (`NewStateAt(6)`, so `Position.Offset` is 0), with no gap. The `y := -s.l.Position.Offset` path with a non-zero offset, and the `l.Gap` term, are never exercised. Row 10's check (line 91) also never asserts `top >= 0`.
- **"components/list places its rows' floating surfaces … so a row's tooltip reads the window's room".** The list does this only when the caller has set a `tooltip.Surfaces` (`list/surfaces.go:35`). A caller that passes only the available room still gets row tooltips placed against an area not moved by the row's offset or the scroll. The list cannot read the room itself, so this is a limit of the design, and the doc states it. sk150 sets the Surfaces in `listContent` (view.go:701), so the clause holds for sk150.
- **Commits are not made yet.** The workbench and `sk150/go.mod` pin `components v1.6.0`. Outside go.work, sk150 would build against a list without `layoutRows`, and the hint tooltips would land at the list's top instead of the row's. Check whether this round expects the components pin to move to the new commit.

**2. Retired words, invented words, wordplay**

No retired words (`ground`, `wash`, `rung`, `launcher`, `bubble`, `register`) appear. The figurative verbs:

- `components/list/surfaces.go:30`: "A row scrolled out of view takes its surfaces with it".
- `components/list/surfaces.go:46`: "The rows in view stand one after the other". `surfaces_test.go:20`, `81` and `89` also say "standing at" and "stands at" for row positions. The rule reserves "stands on" for a drawn thing on a fill.
- `workbench/sk150/view.go:698`: "the list hands them back here". `view.go:1336`: "are handed on to that Surfaces".
- `workbench/sk150/hint.go:22`: "a field's editor is handed out".
- `workbench/sk150/hint_test.go:20`: "rests the pointer". `hint_test.go:95`: "its tooltip goes", where "is removed" or "is not drawn" is the literal verb.
- `workbench/sk150/hint.go:13` calls the cell "one settings cell's tooltip trigger" and then calls it "the anchor" in the same sentence. In the Attachment table the Trigger is the interaction and the Anchor is the element, so the cell should be called the anchor.

**3. Comments that narrate history**

None. "no longer hovered" at `hint_test.go:95` describes a change of state at run time, not code history. The demo-button comment in view.go was rewritten without "now" or any mention of the arbiter it used to have.

**4. Correctness**

- **List rows:** correct.
  - After `Layout`, `-Position.Offset` is where the first visible row is placed, including the ScrollToEnd adjustment, and each following row is one size plus `l.Gap` further down. This matches `layout.List.layout` in Gio v0.10.2.
  - The leading child that is laid out but out of view is skipped, and the cross-axis alignment is Start because `State` never sets it.
  - Each row's surfaces go to the outer Surfaces exactly once.
- **Scroll function** (view.go:701–707): the surfaces are collected on `rgtx` and placed on `gtx`, which holds the room moved by `pageColumn`'s `-pad` and, in the head case, by `headH`. Correct.
- **Inset:** the row callback moves the room and the Surfaces by `side.Left` and `side.Top` before `side.Layout` pushes the same offset. Correct.
- **Grid row** (view.go:1323): the x offsets are 0, then `colW + compactGap`, and y is `(rowH - cellH)/2`, which matches Flex Middle and the SpaceEnd start. `placeSurfacesAt` both pushes `op.Offset(off)` and moves the room and Surfaces by `off`. When it forwards to an outer Surfaces, the pushed offset wraps no ops, so nothing is drawn twice.
  - It duplicates `shell`'s unexported `placeSurfaces`. That is a hoist candidate, not a bug.
- **Surfaces** are neither lost nor drawn twice: a row's own Surfaces from the list wraps the grid's per-cell Surfaces, and each is drained once. `patterns/shell/stackedpage.go` uses a bare `layout.List`, so `layoutRows` does not affect it.
- **Arbiter:** one per window from `ContentLayer`, shared by the demo button and every hint cell. Delay is not set, so the theme's DurXSlow applies.
- **Focus:** the tags are read every frame. A missing editor returns a nil interface. A missing `setClicks` entry would be a typed nil pointer, which never matches the focused tag, so it does no harm. The switches use `&switchClicks[s]`, the same clickable `switchWidget` gets.
- **Hover:** the hover tracker is updated before its pass-through area is registered over the cell's own areas, so the controls keep their events.
- **Captures:** without the pointer, no surface. With it, a one-line surface just under the PPT cell, shifted inward to x≈4 (the room's edge plus the 4dp gap), inside the window and drawn over the "PPT %" row. That is the expected placement; the one line comes from the library not wrapping yet (first point of section 1).

## Disposition

1. Wrapping: correct reading, not this task's. The packet states that the library's own elision is not this task's; the cells use the library tooltip, whose single line with middle elision CO1.16 replaces with the Tooltip entry's wrapping. No change.
2. The Exit grep: correct. Every match is a `tooltip.` identifier or the word "tooltip." in prose; none names the deleted file's `tips`, `newTips`, `tipDelay`, `wrap`'s old signature or `bubble`. Recorded in the commit body.
3. Cell foot: fixed. The test now finds the cell as the run of painted rows across the grid's first column that holds the label, and asserts that the surface starts at or below that run's foot.
4. Scroll offset: fixed. The list test now scrolls 15 px past row 6's top, so the first row in view has a non-zero offset; row 7 at 25..65 is checked below its foot and row 11 at 185..225, crossing the window's bottom edge, is checked above its top. The test fails with the list's placement removed. `l.Gap` is not exercised because `State` never sets a gap; no change. The `top >= 0` point is moot: a painted row index is never negative and the test fails first when nothing is painted.
5. A caller with no Surfaces: correct, a limit of the layering. Filed as pool item 1242.
6. Pins: the workbench builds against the components commit through the generated go.work, as every task between tagging rounds does; no tag and no pin bump. No change.
7. Figurative verbs: fixed, all eight. "takes its surfaces with it" → "are not placed"; "stand one after the other" → "are placed one after the other"; "standing at"/"stands at" → "is at"; "hands them back here" → "adds them to the Surfaces set here"; "handed on to" → "added to"; "handed out" → "known"; "rests the pointer" → "moves the pointer onto … and keeps it there"; "its tooltip goes" → "its tooltip is not drawn"; the cell is called the anchor of its tooltip, not its trigger.
8. Hoist candidate for the grid's copy of `placeSurfaces`: filed as pool item 1243.

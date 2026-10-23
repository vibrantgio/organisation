---
date: 2026-10-23
task: CM1.6
phase: CM
reviewer-model: claude-opus-5-5
reviewer-effort: unrecorded
---

# CM1.6: themer's style rows run to the lane, read cold in both appearances

## What was reviewed

The reviewers read two whole-window goldens at 1200 × 900, 1x, at the platform setting When scrolling with nothing scrolled. The goldens are `workbench/themer/testdata/golden/window-light.png` and `window-dark.png` at workbench b7cc233. Each brief carried the task's text and the Scrollbar, Scroll area, Row and Selection entries, verbatim.

Two reviewers ran. The first brief went out with a placeholder where `reviews/misreads.md` belonged. That brief could not be withdrawn. The second brief carried `reviews/misreads.md` whole, and `grep -c 'Recorded misreads'` on that file returned 1. The second reply is the record. The first reply is kept below it because it reaches the same finding.

## The reply, verbatim (second reviewer, full brief)

I found one defect against the Scrollbar entry. Row and Selection hold, and Scroll area has nothing in the frames to judge. All measurements are from the new goldens at 1x, compared with HEAD~1.

**Scrollbar: the thing shows something else.**
- **What holds:**
  - **Trailing end:** the marked row's fill now reaches the box's trailing edge.
    - Light, row "catppuccin-latte": the fill (0,100,225) runs from x 30 to x 319, and x 320 is the window (255,255,255).
    - Dark, row "catppuccin-mocha": the fill (0,89,209) runs from x 30 to x 319, and x 320 is (30,30,30).
    - In the old goldens the fill ended at x 310, so the 10 px trailing padding is gone.
  - **Rounded end:** the fill's corners at the trailing end curve in by the same 4 px as at the leading end (y 490: leading x 34→38, trailing x 312→316). The pill ends at x 320 as drawn and is not cut off by the clip.
  - **Tag:** the "either" tag ends at x 309, so the row's own inset keeps it 11 px from the end. In the old goldens it ended at x 299.
  - **Bar:** at When scrolling it takes no lane, and no thumb is drawn at rest.
  - **Clipping:** the box's surface runs flat from y 510 to y 834 at x 200, so there is no fade at the bottom edge.
  - **Code:** the Always case is checked by `TestTheStyleColumnsMarkedRowRunsToTheLane`, which requires the fill end to equal `laneMin`. It reads correctly.
- **Violation:** the sentence "A column of rows has no padding of its own" is still broken on the leading side.
  - The column still applies `op.Offset(BoxInset)` at the leading edge (styles.go, the `rows[row+1]` wrapper), so the column itself keeps a 10 dp padding there.
  - The pixels show it: the marked fill starts at x 30, which is 10 px in from the box's leading edge at x 20, but ends at x 320 with no gap at the trailing edge.
  - The result is a lopsided selection: 10 px of margin at the start and 0 px at the end, with the rounded end touching the box's edge.
  - The task only names the trailing padding. The entry has no such limit, so either the leading 10 dp goes too (and the rows' own inset handles the start), or the entry needs to be ruled to cover the trailing end only.
  - **Wording question:** "its pill" in the entry is ambiguous. The commit reads it as the tag. If it means the selection pill, then the entry asks for the pill to stay clear of the end and the fill at x 320 breaks it as well. I judged against the task's reading, but the wording should be settled.

**Scroll area: the entry holds.** The style column contains no scroll area. The code block in the preview fits inside its box and shows no bar or faded edge.

**Row: the entry holds.** Rows are 20 px apart and the marked row fills y 490 to 509. Each row holds its name and tag across the list's width. The row height did not change in this commit.

**Selection: the entry holds.** The marked row uses the selection colour, (0,100,225) in light and (0,89,209) in dark, with white text. Only one row is marked.

VERDICT: FAIL — Scrollbar: "A column of rows has no padding of its own" is broken at the leading edge. The column still offsets its rows by BoxInset, so the marked fill runs from x 30 to x 320: 10 px of column padding at the leading edge and none at the trailing edge.

## The first reply, verbatim (brief without misreads.md)

The trailing padding is gone, but in both renders the selected style's fill now runs flush against the column box's edge, which the Scrollbar entry rules out. The other three entries hold.

**Scrollbar: the thing shows something else.**
- The entry says: "A column of rows has no padding of its own: the rows run to the lane, and each row's own inset keeps its content and its pill from the end, as the platform's lists do."
- **Pixels:** on the selected row (y 490..509, in light and in dark), the fill runs from x 30 to x 319. The box spans x 20..319, so the pill has 0 px between it and the box's trailing edge. Its rounded right end fades out by x 318..319 (light, y 492: 130,153,231 → 242,243,246 → 247), so the rounding falls right on the edge.
- **Old golden:** the fill ended at x 309, 10 px in.
- **Content:** this part holds. "either" now ends at x 308, 11 px from the edge, where it was 271..298 before.
- **Cause:** the row's inset now keeps only the content from the end, not the pill. The diff in `themer/styles.go` cuts the trailing `BoxInset` out of the row as well as out of the column (`Max.X - 2*inset` became `Max.X - inset`). The row was supposed to reach the lane while the pill stayed inset inside it.
- **Asymmetry:** the pill sits 10 px from the leading edge (x 20 → 30) and 0 px from the trailing edge. The platform's lists inset the pill the same distance on both sides.
- **Leading side:** that 10 px leading inset is itself padding of the column's own (the rows start `BoxInset` in from the column's leading edge). This arguably breaks "a column of rows has no padding of its own" as well.
- **The task's own Exit is the root:** it asks that "the marked row's fill ends at the lane / at the box's edge", which contradicts the entry's "its pill" being kept "from the end". The new test was most likely written to that Exit wording, so it checks for the defect instead of catching it.
- **Rest of the entry:** at When scrolling there is no lane and no thumb at rest, which matches. The parts of the window outside the column are unchanged from the previous golden.

**Scroll area: the entry holds.** The preview's code block is unchanged from the previous golden, nothing in it is faded, and no scrolled state is shown.

**Row: the entry holds.** Rows are on a 20 px pitch (the selected row covers y 490..509), the same as before. Each row's parts span the list's width, and only the end of the selected fill and the trailing "either" tag moved.

**Selection: the entry holds.** The selected style is painted in the platform's selection colour with white text: light (0,100,225), dark (0,89,209). Neither colour changed from the previous golden.

VERDICT: FAIL — Scrollbar: the selected row's fill (x 30..319) runs flush against the box's trailing edge (x 319) at When scrolling, so the row's inset no longer keeps its pill from the end, as the entry requires. The pill is 10 px from the leading edge and 0 px from the trailing edge.

## What was done with it

- The Scrollbar finding stands, and it is not a recorded misread. [[DOMAIN#Pill]] defines the pill as "the rounded fill behind a selected row, inset from the row's ends". The Scrollbar entry keeps the pill from the row's end. CM1.6's Exit asks for the marked row's fill to end at the lane. b7cc233 follows the Exit, so the task's Exit and the entries disagree. This is filed as finding 1210 [decide].
- The leading-side padding is part of the same ruling. Under the Pill reading, the row spans the column from edge to edge, and the pill is inset from both of the row's ends. Finding 1210 covers this.
- CM1.6 stays unticked until finding 1210 is ruled. b7cc233 is pushed and is green.
- Row, Selection and Scroll area hold in both replies. There is nothing to do for them.

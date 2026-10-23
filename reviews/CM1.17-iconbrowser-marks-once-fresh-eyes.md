# CM1.17 fresh-eyes review: iconbrowser marks section

Reviewer: a general-purpose agent that did not do the work. It was briefed with the four window goldens, the Icon, Search field and Empty state entries, the task text and the recorded misreads (count of "Recorded misreads" in the file: 1).

Goldens under review, in `workbench/iconbrowser/testdata/golden`:

- `window-no-query-light.png`: 71335 pixels changed, box (13,92)-(987,700)
- `window-no-query-dark.png`: 76913 pixels changed, box (13,92)-(987,700)
- `window-some-match-light.png`: 17884 pixels changed, box (13,93)-(987,303)
- `window-some-match-dark.png`: 18914 pixels changed, box (13,93)-(988,303)

Each box starts at the marks heading. In the no-query frames the box runs to the frame bottom, because the taller mark cells move the catalogue down.

## Reply, verbatim

The task's own change holds, but the frames break the Icon entry on chevron-down, so this fails.

**Icon: the thing shows something else.** The Icon entry says: "A mark that is another mark's figure turned, chevron-down of chevron-right, keeps that figure's stroke and extent, as the platform's symbols do." I measured the inked bounding boxes in the no-query goldens (pixels more than 60 levels off the background):

| Mark | Light (w × h px) | Dark (w × h px) |
|---|---|---|
| chevron-right | 14 × 24 | 14 × 24 |
| chevron-left | 14 × 24 | 14 × 24 |
| chevron-down | 12 × 8 | 14 × 8 |
| chevron-pair | 12 × 17 | 14 × 18 |

Chevron-right turned a quarter would be about 24 × 14. Chevron-down is instead about a third of that in each direction, about the size of one half of chevron-pair. That matches its doc comment in `/Users/rene/code/w/vibrantgio/components/icons/icons.go` (lines 114–117): "one chevron pointing down, the stacked pair's lower half". The entry rules that chevron-down is chevron-right's figure, not half of the pair.

This is a fault in how the mark itself is drawn, not in CM1.17's layout. But it is the same visible symptom the task names as its cause: a chevron-down far smaller than its neighbours. Now drawn at the catalogue's size, it still reads as a speck beside chevron-left and chevron-right.

The rest of the Icon entry holds:
- **Drawn once at the catalogue's size.** There is one cell per mark, 15 marks in all, with no 16/20/24 triple. The marks are drawn at the 40 dp catalogue scale: plus is 32 × 32 and chevron-right is 24 tall, against the catalogue's Action3DRotation at 40 × 40.
- **Text colour.** The marks are in the text colour in both schemes, never the accent.
- **Names.** Every mark is named by what it depicts.

**Search field: the entry holds.**
- The order is magnifying glass, then text, then the clear mark, which shows only when something is typed.
- The field filters the collection: "folder" leaves 2 marks and 5 icons, with no highlight.
- The window background is not drawn, so whether the field is raised is a harness limit (recorded misread), not a finding.

**Empty state: the entry holds.**
- Neither query here leaves a section with no match, so no empty state is due. When "folder" filters the window, both sections keep their headings and counts, and no empty section is left behind.

**The task's text: holds, apart from the chevron-down drawing above.**
- With no query, the note reads "the set ships each mark at 16, 20 and 24 dp; chevron-down, for a row whose disclosure is open", and chevron-down's cell is on screen.
- With "folder", the note reads "the set ships each mark at 16, 20 and 24 dp" with nothing appended, and chevron-down is filtered out.
- There is no second rendering anywhere.

VERDICT: FAIL — Icon entry: chevron-down is drawn at about 12×8 px, the size of one half of chevron-pair, not chevron-right's 14×24 figure turned (about 24×14), so it does not keep "that figure's stroke and extent".

## Disposition

1. Icon, chevron-down drawn at about 12 × 8 px instead of chevron-right turned: already planned as CM1.23 (chevron-down is redrawn at chevron-right's extent). The defect is in the mark's drawing in `components/icons`. This task's layout draws the mark once at the catalogue's size, as the reviewer confirms. Nothing filed.

The Search field, Empty state and task-text answers hold. The FAIL verdict rests only on finding 1.

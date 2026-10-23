# CM1.10 fresh-eyes review: a code block's side padding scrolls with the code

The reviewer did not do the work. The packet held the changed goldens' paths with their before copies, the Scroll area and Scrollbar entries, the task's text, and the whole of `reviews/misreads.md` (one `Recorded misreads` heading).

## Reply, verbatim

The goldens match both Language entries. The only departures are the edge fade over the cut edge, which is planned (CM1.11), and the bar's measured sizes, which are also planned (CM1.27).

**What changed.** I compared all 17 before/after pairs pixel by pixel. In every golden the changes beyond 2 levels sit only inside the wide fence: 79 rows tall and 26 to 121 px wide. At rest that region is next to the trailing rim; in the scrolled golden it is on the leading side. Everywhere else the differences are at most 2 levels of 255: 275 pixels in gallery `markdown-dark`, 194 in `markdown-light`, 16 in `window-switch-dark` and 3 in `window-dark`. This matches the worker's account.

### Scroll area

- **"shows the part that fits … never reflowed or cut."** The entry holds. In every golden the fence lines run past the visible part on one line and are not wrapped (`code-overflow-*`, `measure-*`, `note-*`, `window-*`, gallery `markdown-*`).
- **"Its padding is inside it and moves with the content … so the content reaches the rim once scrolled."** The entry holds.
  - In `code-overflow-scrolled-light` the rim is at x=8 and x=411. After the change, glyph pixels reach x=10–11 (row 90: (233,234,234) and (228,229,229) at x=10–11). Before, the first ink was about 12 px further in. Partly cut glyphs ("k", "r", "}") now show at the leading rim.
  - At the far end the trailing padding is visible again, about 10 px between "navigation:" and the rim. That is right, because the padding scrolls as part of the content.
  - At rest the leading 12 px inset is kept in every golden (gallery, vaultview `window-dark`, `measure-padded-wide-bar`). Only the trailing side changed: the code now runs to the rim, at x=409–410 in `code-overflow-light` row 70.
- **"What is past its edge is clipped at the edge, never faded."**
  - The clip is at the rim. The rim pixels (220,221,221) at x=8 and x=411 are intact in both the at-rest and scrolled goldens, and no glyph pixel falls outside the fence.
  - The glyphs next to the rim are still faded at both edges, in both schemes and in every changed golden. This is the planned CM1.11 work, not a finding.

### Scrollbar

- **"A region that scrolls sideways has its bar at its bottom edge"** and **"its bar is at the area's own edge, inside the area."** The entry holds. In `code-overflow-scrolled-light` the thumb lies along the fence's bottom edge and ends at x≈408, inside the rim at 411.
- **"a thumb on a track whose size mirrors how much of the content is visible."** The entry holds. The thumb is about 4 px longer after the change, and that is the correct direction. With the padding now part of the scrolled content, the visible share goes from (W−24)/C to W/(C+24), and the second is larger whenever the content is wider than the area. The measured sizes themselves are planned (CM1.27).
- **"at rest there is no thumb."** The entry holds. None of the resting goldens draws a thumb, which is also covered by the recorded misread that a headless render draws no scrollbars at rest.
- **The lane at Always.** In `measure-padded-wide-bar` the fence shows no lane or bar of its own. That golden changed only within about 30 px of the trailing rim, so this task did not cause it. It falls under the planned bar-coverage work (CM1.27); I'm noting it here, not filing it.
- The sentences about the column, rows, search marks and the page lane are not affected by these goldens.

VERDICT: PASS

## Disposition

1. The faded glyphs at the cut edges: planned as CM1.11. Nothing is filed.
2. The thumb's measured sizes: planned as CM1.27. Nothing is filed.
3. No lane or bar on the fence in `measure-padded-wide-bar`: the bar's coverage is planned as CM1.27. That golden is a resting frame, and a resting fence draws no thumb. Nothing is filed.
4. Pixels that differ by at most 2 levels outside the fence: rasterizer noise from the changed op sequence, as the reviewer measured. Nothing is filed.

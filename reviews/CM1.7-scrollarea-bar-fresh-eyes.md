# CM1.7 fresh-eyes review: the scroll area's bar

The reviewer did not do the work. It was briefed with the task's text, the Scrollbar and Scroll area entries, and the whole of `reviews/misreads.md` (one `Recorded misreads` heading).
It reviewed these goldens: `components/scrollarea/testdata/golden/fade-light.png`, `components/gallery/testdata/golden/components-light.png` and `components-dark.png`, and old-against-new crops of the gallery's Scroll area section.

## The reviewer's reply, verbatim

The lane change holds in every image. The verdict is still FAIL, because the content fades at the area's edges, which both entries forbid. That fade is in the old renders too, so it was there before this change and is outside CM1.7.

**Scrollbar**

- "Every region that scrolls has one." Holds. Bands 2–4 of fade-light.png, which overflow, each show a thumb. The gallery area shows one in both schemes.
- "What is past a scrolling region's edge is clipped at the edge, never faded, as the platform's scroll views clip." The images show something else: the content is faded at the edge.
  - In fade-light.png, band 2 at y=80, the last mark's red channel rises from 117 to 240 across x=264–277 before the edge.
  - Band 3, on its left edge, rises from 117 to 212 across x=6–15.
  - In components-light.png at y=3560, the leading mark ramps from (129,172,255) at x=36 to (251,252,255) at x=24. Its trailing mark ramps the same way at x=528–531, against the solid mark colour (0,136,255).
  - The gallery section title says so outright: "the edge dissolves while content is hidden past it".
  - The old renders fade the same way, so this was there before the change.
- "At Always the bar has a lane of its own and the rows stop short of the lane." Holds.
  - fade-light.png: band 2's marks run y=68–95 and its thumb runs y=106–111, so the thumb sits 10 px below the content and over no mark. Bands 3 (thumb y=166–171) and 4 (thumb y=226–231) are the same.
  - Gallery, new render, both schemes: the marks end at y=3579 and the thumb runs y=3590–3595, with no overlap.
  - In the old crops the thumb lies over the bottom of the marks. In the new crops it is 10 px lower, in its own lane, and the section's bottom border moves from about y=3608 to y=3618.
- "When the content fits, the standing bar is drawn as the platform draws it then, read from the reference; a lane exists only for a bar that is drawn." Partly shown. Band 1 has no thumb. The area's outline can't be seen on the white background, so whether band 1 keeps an empty lane is not shown. Whether "no bar" matches the reference is not shown either.
- "A region that scrolls sideways has its bar at its bottom edge, the same way." Holds. Every thumb is horizontal and at the bottom.
- "A scroll area inside a document is a region that scrolls: its bar is at the area's own edge, inside the area, with a lane of its own at Always." Holds.
  - In fade-light.png the thumbs span x=2–87 at the start, x=88–173 in the middle and x=192–277 at the end. That is within the 280 px width and moves with the scroll position.
  - Each thumb ends 8 px above its 60 px band's bottom, so it is inside the band.
  - In the gallery the thumb spans x=162–405, inside the marks' span x=46–528.
- "It follows the platform's Show scroll bars setting: Always is the standing bar in a lane of its own." Holds for Always, which is the case shown.
- Not shown: the When scrolling and Automatic sentences (the fade behaviour, showing under the pointer, and the case of no thumb at rest), reading the setting off the platform, search match marks on the track, measuring the lane, track and thumb into the reference, row insets and page padding, and a field above the rows.
- Bar colour, for the record: thumb grey (109,109,109) in light and (159,159,159) in dark. It is not compared against the reference here.

**Scroll area**

- "it shows the part that fits and lets the user move the visible part to the rest, sideways or down." Holds. Bands 2–4 show start, middle and end, and the thumb position follows.
- "Nothing in it is chosen; the content is never reflowed or cut." Holds. The marks keep a constant size and pitch in every band. Band 1, which fits, shows its full ruler, ending in its last mark at x=198–199.
- "Its padding is inside it and moves with the content … so the content reaches the rim once scrolled." Holds as far as the images show. Band 1 at rest begins at x=0 with an 8 px inset. In band 4, at its end, the last mark runs to x=273 of 280. Band 3, in the middle, has content reaching x=8.
- "What is past its edge is clipped at the edge, never faded." The images show something else. These are the same fades measured under Scrollbar above, in fade-light.png and in the gallery section in both schemes. This is also from before the change.

VERDICT: FAIL — the lane change itself holds (bar in its own 10 px lane below the content, inside the area, rows not overlapped), but content past the area's edge is faded rather than clipped (measured gradients at the edges in fade-light.png and the gallery section), which breaks "What is past its edge is clipped at the edge, never faded" in both Scroll area and Scrollbar; the old renders fade too, so this was there before the change and is outside CM1.7.

## Disposition

- The lane, the bar at the area's bottom edge, and the bar inside the area: the reviewer finds that they hold. No action.
- The faded edge, under both entries: the finding is real. The fade is in the old renders, and CM1.7 does not change it. CM1.11 removes the dissolve from `markdown` only. The fade in `components/scrollarea` and the gallery section's title go to the review pool as finding 1211.
- Band 1, content that fits: the test `TestStandingBarHasALane` checks that an area whose content fits takes no lane at Always. The reference shows that a list draws nothing when its content fits. No action.
- The thumb's colour against the reference: CM1.27 covers it. No action.

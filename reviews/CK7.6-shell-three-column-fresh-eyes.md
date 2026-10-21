---
date: 2026-10-22
task: CK7.6
phase: CK
reviewer-model: claude-sonnet-4-5
reviewer-effort: unrecorded
---

# CK7.6 — the shell's three-column arrangement, read cold in both appearances

## What was reviewed

The gallery's new `patterns-shell-three-column` specimen in both
appearances: the pane down the leading edge, the list column of absolute
width beside it behind a splitter, the content column taking the rest, and
one toolbar lying across the list and the content with one share per
column.

Both schemes were cut out of the regenerated `patterns-light.png` and
`patterns-dark.png` sheets at one pixel per point and handed to a reader
who had seen neither the packet nor any source, with one instruction —
say what the picture shows and what looks wrong — and no checklist. Four
recorded misreads were briefed: hex comparisons only, since a colour judged
by eye at 1x misleads; a still holds no keyboard, so nothing is focused and
the absence of a focus ring is right; dark ink reads grey at 1x; and the
golden harness paints no window control buttons, so their absence is
deliberate.

## The reader's reply, verbatim

> I read both pictures and then took pixel measurements rather than trusting my eye at 1x.
>
> ## What the pictures show
>
> A 900x292 sheet, white (#FFFFFF) in light and #1E1E1E in dark. Inside it, a three-column layout. Down the leading edge is a floating rounded card (x=32..187, y=20..263, corner radius ~10) holding a "+" and a sidebar-toggle glyph at its top right, and three rows below: Inbox, Drafts, Archive. To its right, a list column whose toolbar carries the word "Drafts" at the leading edge and a chevron-down button at the trailing edge, with two paragraphs below ("The list column." / "Absolute width, kept when the window is resized."). A 1px vertical divider at x=400 (#E6E6E6 light / #343434 dark) running y=12..271 separates it from the content column, whose toolbar has a back chevron at the leading edge and a magnifier near the trailing edge, with three paragraphs and a small footer "48 messages — Drafts" near the bottom.
>
> The window's own bounds are only inferable: the divider and the pane's clipped shadow both stop at y=271, and the shadow is clipped at x>=24. So the window is roughly x=24.., y=12..271, and the pane is inset 8px from its leading, top and bottom edges — symmetric, which is right.
>
> ## What looks wrong
>
> 1. **In light, the three toolbar buttons have no body at all.** Scanning across the chevron at y=41 gives 255 for the full width of the disc — the fill is #FFFFFF, identical to the backdrop, and there is no border. In dark the same disc is #262626 on #1E1E1E, plainly a button. So the control reads as a button in one appearance and as a bare glyph in the other.
>
> 2. **The light button's shadow is detached and sits below the disc.** At x=372 the disc region (y=20..55) is pure 255; a #F6F6F6 plateau then starts at y=56 and fades out around y=85. The disc is invisible and its shadow is not, so what you see is a glyph hovering over a grey smudge. The dark equivalent is 1 level deep (#1D1D1D) over about 8px. The shadow geometry, not just the colour, differs between appearances.
>
> 3. **The pane's border is drawn white in light.** At x=32, x=187, y=20 and y=263 the dark picture has a 1px #3A3A3A border; the light picture has a 1px #FFFFFF line in exactly those positions, which on a white backdrop is nothing. The light pane is held together only by its #F7F7F7 fill and its shadow.
>
> 4. **Nothing marks the current sidebar row.** The list header says "Drafts" and the footer says "Drafts", but scanning the pane at y=70, 86, 108 (the gaps between rows) and across each row's full width gives a flat #F7F7F7 / #1C1C1C with no highlight, fill or indicator behind any of the three. This is selection, not focus, so the still-picture exemption doesn't cover it.
>
> 5. **Two button treatments in one toolbar strip.** The "+" and sidebar-toggle glyphs sit directly on the pane fill (#F7F7F7 / #1C1C1C at y=41 right up to the glyph strokes) with no backing of any kind, while the chevron, back and search buttons in the same horizontal band do have one.
>
> 6. **The list column's title is indented 9px past its own body text.** "Drafts" starts at x=209; "The list column." starts at x=200. The content column does not do this — body at x=413, back button disc at x=417, and that 4px is a round shape's normal overhang.
>
> 7. **The "Drafts" title is typographically indistinguishable from body text** — same glyph height (11px: y=32..42 vs y=79..89) and the same darkest ink (#2B2B2B light, #DDDDDD dark). Nothing says "column header".
>
> 8. **The window's trailing and bottom edges are invisible.** The window background is exactly the sheet backdrop in both appearances, so apart from the divider and the clipped shadow there is no evidence of where the window stops. A 20px strip below y=271 and everything right of the search button could be window or could be sheet.
>
> 9. **The trailing toolbar inset looks enormous.** The search disc ends at x=775 with 125px of empty canvas to its right, against a 13px leading inset in both columns (200 after the pane edge at 187; 413 after the divider at 400). Either the window is much narrower than the picture and nothing shows it, or that button is badly under-inset.
>
> 10. **The pane's shadow is clipped square rather than faded.** In light it goes #F2F2F2 at y=271 to #FFFFFF at y=272 in one step, and #FFFFFF at y=11 to #FBFBFB at y=12. There is a hard horizontal cut across the shadow at both ends of the window.
>
> 11. **The footer sits 4px off the window's bottom edge** (glyph descender bottom y=267, window bottom y=271) while the column's leading inset is 13px. The bottom padding is a third of the side padding.
>
> 12. **The footer is the weakest text in the light picture**: #828282 on #FFFFFF, against #999999 on #1E1E1E in dark. The dark one has considerably more headroom than the light one for the same role at the same small size.
>
> ## What looks right
>
> The divider running the full window height and passing behind the toolbar band is correct for a three-pane macOS window. The pane's 8px inset is symmetric on its three window edges. All four toolbar clusters sit on the same centreline within a pixel or two (discs centre y=38.5, "+" 37.5, toggle 37.5, title 37). The two columns' body text shares one baseline (both first lines y=79..89) and both use the same 13px leading inset. Body, title and sidebar ink are solid in both appearances (#282828-#2B2B2B on light, #DDDDDD on dark) — the washed-out look at 1x is the known misread, not a defect.

## What the author read, before handing the pictures over

Read off the regenerated sheet with PIL at one pixel per point, and off the
two Mail captures beside it:

| What | The specimen | Mail |
| --- | --- | --- |
| the seam between the list and the content, light | `#e6e6e6`, one pixel | `#e6e6e6`, one pixel |
| the same, dark | `#343434` over `#1e1e1e` | `#000000` over `#232a2e` |
| the seam under the toolbar | present at every row of the strip | present at every row of the strip |
| the pane's shadow on the list's leading edge, light | `#f4f4f4` at the pane's edge to `#ffffff` 26 px along | `#f2f2f2` to `#ffffff` 34 px along |
| the same, dark | `#1c1c1c` to `#1e1e1e` 16 px along | `#21282c` to `#232a2e` 20 px along |
| the content column's footer | stops at the seam; its first ink is 12 past the content's own edge | stops at the seam |

The light seam matches Mail to the byte. The dark seam does not, and the
difference is the library's rule rather than this arrangement's: every seam
in `patterns/shell` is the platform's separator flattened onto the fill it
stands on, and Mail is spending something darker. Pooled as 1036 rather
than changed here, because changing it changes every other seam in the
library.

## What was done about it

**Fixed in this landing — finding 6, and it is a measured defect.** The
list column's share of the toolbar led from the measured 20, and the list
column's own rows were inset 12, so the collection's name stood nine
columns clear of the rows it names. Mail does not: its mailbox name's first
painted column is x=352 and its rows' own first painted column is x=352 or
353, both 20 past the seam at x=332 — the name stands over what it names.
The specimen's list column now leads from the same 20 its share does, and
the reading is in `reference/macos/controls.md`. The gallery and sitedocs
goldens were regenerated for it. Measured in the regenerated sheet: the
title's first ink at x=209 and the rows' at x=211, both leading from column
208, the remaining two being the glyphs' own left bearings.

**Read as the specimen's framing, not as the drawing — findings 8, 9 and
10.** The specimen is drawn at the size a window would show it at, 760 dp
inside a 900 dp sheet, as every other bounded specimen on that sheet is,
and it is clipped to that box on purpose: a window cuts what its own chrome
throws at its edge, which is the hard cut across the pane's shadow the
reader read at the top and the foot. The "enormous" trailing inset is the
125 px of sheet beyond the window's own trailing edge; the search control
stands the measured 8 in from the window's edge, not from the sheet's.

**Recorded and not acted on, because each is a measured platform reading
this task does not own — findings 1, 2 and 3.** The bordered toolbar
control is a white capsule on white in the light appearance in the stored
Finder captures, its shape carried by its rim and its shadow, and its drop
shadow's own geometry is what `reference/macos/controls.md` records under
"What the toolbar control's drop shadow measures". The pane's rim is the
platform's, measured for the Pane entry and reviewed under CK7.1. All three
are the two-column arrangement's drawing, unchanged by this task, and all
three were filed against it once already.

**Correct by the shell's own contract — finding 5.** The two drawings of a
chrome mark are deliberate and they are the Pane entry's: a mark that acts
on the pane is drawn BARE at the pane's own corner, and a mark that acts on
the document is drawn BORDERED in the toolbar. One strip carrying both is
what the platform's own sidebar windows show. The reader read the rule
correctly and filed it as an inconsistency.

**The specimen's own payload — findings 4, 7, 11 and 12.** The three
sidebar rows are plain lines of prose, not operated rows, so none of them
is the selected one; the gallery's `patterns-sidebar` specimen is where the
pill is shown. The collection's name is set in the same role as the body
because that is the slot the specimen fills, where Mail sets the name
heavier with its count under it. The footer's depth and its muted ink are
the two-column specimen's own and were reviewed with it.

# CM1.11 fresh-eyes review: the cut edge is clipped

The reviewer was a general-purpose agent that did not do the work.
The brief held the task's text, the Scrollbar and Scroll area entries from DOMAIN.md, the changed goldens with their earlier versions, and the whole of reviews/misreads.md (`grep -c 'Recorded misreads'` gave 1).

## The reply, verbatim

The images back the task. In every AFTER image I checked, content past a cut edge is clipped at the edge with nothing fading it. The gallery's Scroll area title now says what the section shows.

I magnified the crops with `sips` because PIL is not installed. I checked the scrollarea edges golden, the gallery components and markdown pages in both schemes, `code-overflow-light`, `code-overflow-scrolled-light`, `measure-padded-wide-bar` and vaultview `note-dark`. I did not open the other changed markdown and vaultview goldens. Each of them changed only in a 16px strip at the edge, and the same change in the images I did open is the fade becoming a clip. I briefed myself with `organisation/reviews/misreads.md` as sent.

### Scrollbar

- **"What is past a scrolling region's edge is clipped at the edge, never faded, as the platform's scroll views clip."** The entry holds.
  - `components/scrollarea/testdata/golden/edges-light.png`: the start band is cut sharply at the right edge (x≈264–280). The middle band is cut at both edges (x 0–16 and 264–280). The end band is cut at the left edge (x 0–16). The rectangles that are cut keep the same full grey as the ones inside. In BEFORE (`scrollarea-fade-light.png`) the same rectangles shade off toward white.
  - Gallery `components-light.png` and `components-dark.png`, Scroll area section, y≈3540–3580: the blue bars stop sharply at x≈24 and x≈544. BEFORE shows a blue-to-white fade at both edges.
  - Gallery `markdown-light.png` and `markdown-dark.png`, fence right edge at x≈567–583: the comment line ("…so the cod") and "typogr" are cut mid-glyph at full contrast.
  - `markdown/.../code-overflow-light.png` is cut at the right ("not"). `code-overflow-scrolled-light.png` is cut at the left ("k inside", "Principles]]", "{}").
  - `measure-padded-wide-bar.png` is cut at the right ("sampl").
  - `workbench/vaultview/.../note-dark.png` is cut at the right ("navigatio").
- **"A region that scrolls sideways has its bar at its bottom edge, the same way."** The entry holds. In `edges-light.png` each overflowing band has its bar below it. In the gallery Scroll area the thumb sits under the area at y≈3590 in both schemes. In `code-overflow-scrolled-light.png` the thumb is along the fence's bottom edge.
- **"A scroll area inside a document is a region that scrolls: its bar is at the area's own edge, inside the area, with a lane of its own at Always."** The entry holds for the bar's position: in `edges-light.png` it is in a lane under each band, at the band's own edge. The lane width, track and thumb sizes go to CM1.27, which is already planned.
- **No bar on the fences in the gallery markdown page, vaultview note and the resting `code-overflow-light`.** Not a defect. Either it is the recorded misread (a headless render draws no scrollbars at rest), or it is CM1.12, which is already planned (the gallery's documents taking the window's Show scroll bars setting).
- **"When the content fits, the standing bar is drawn as the platform draws it then…"** `edges-light.png` band 1 (content that fits) has no bar. This band did not change between BEFORE and AFTER and is outside this task, so this is no finding; the reference values belong to CM1.27.

### Scroll area

- **"What is past its edge is clipped at the edge, never faded."** The entry holds. The evidence is the same scrollarea, gallery, markdown and vaultview crops listed under the Scrollbar entry.
- **"Its padding is inside it and moves with the content … so the content reaches the rim once scrolled."** The entry holds. In `code-overflow-scrolled-light.png` the text runs to the fence's left rim with no inset left, and at rest (`code-overflow-light.png`) the left inset is there.
- **"Nothing in it is chosen; the content is never reflowed or cut."** The entry holds. Every fence line stays on one line, and the clip only hides content past the edge without removing it: the scrolled frame shows the rest of the same lines.
- **Gallery title.** It now reads "Scroll area — content past an edge is clipped at the edge, and the bar says there is more", in both schemes. That states what the section shows, the clip and the bar. BEFORE read "the edge dissolves while content is hidden past it".

VERDICT: PASS

## Disposition

- Scrollbar, clipped and never faded: holds. Nothing to do.
- Scrollbar, the bar at the bottom edge of a region that scrolls sideways: holds. Nothing to do.
- Scrollbar, the bar at the area's own edge with a lane at Always: holds. The lane width, track and thumb sizes are planned as CM1.27.
- No bar on the resting fences in the gallery, vaultview and code-overflow-light: a recorded misread (a headless render draws no scrollbars at rest), and the gallery's documents taking the window's setting are planned as CM1.12. Nothing filed.
- Scrollbar, the standing bar when the content fits: the band is unchanged by this task, and the reference values are planned as CM1.27. Nothing filed.
- Scroll area, clipped and never faded: holds. Nothing to do.
- Scroll area, the padding inside the area: holds. Nothing to do.
- Scroll area, never reflowed or cut: holds. Nothing to do.
- The gallery title: holds. Nothing to do.

No finding went into the review pool.

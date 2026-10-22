---
date: 2026-10-22
task: CK7.8
phase: CK
reviewer-model: claude-sonnet-4-5
reviewer-effort: unrecorded
---

# CK7.8 — feeds' window on the three-column arrangement, read cold in both appearances

## What was reviewed

The whole Feeds window at 1200×800 at one pixel per point, in both
appearances, composed through the application's own layer stack
(`buildLayers` over `components/golden`'s `Layers`) — the pane down the
leading edge, the articles table as the list column beside it at the
platform's own width with the pager as its footer, the article with its tabs
in the column that takes the rest, and one toolbar across the two of them
with a share each.

The pictures were handed to a reader who had seen neither the packet nor any
source, with one question — what would a developer who uses this platform
every day complain about — and no checklist. Five recorded misreads were
briefed: the golden harness paints no window control buttons, so the empty
run at the pane's top-leading corner is where the platform's three will
stand; a lower hex is a darker colour; a still holds no keyboard, so a
selected row showing the grey pill rather than the accent one is right; dark
ink reads grey at 1x; and the column on the right is the document, not an
inspector.

## The reader's reply, verbatim

> I read both captures at full size and then sampled the pixels to be sure about colours and geometry.
>
> ## What a macOS developer would complain about
>
> 1. **The sidebar is a floating card; the other two panes are flush.** The sidebar is inset 8px from the left and 9px from the bottom (its edge runs down x=8 and across y=791), has its own ~10px corner radius, a 1px rim (#FFFFFF light / **#3A3A3A** dark) and a soft drop shadow bleeding ~8px into the article list (x=228→242 ramps #F3F3F3→#FAFAFA). The list and reader panes run hard to x=1199 and y=799. So one pane is a card and two are panes. AppKit sidebars are flush to the window edge and share the window's own radius — and a *light* 1px rim around a dark panel in dark mode reads as a 2007-era bevel.
>
> 2. **There is a pagination widget at the bottom of the article list.** Bottom-left of the middle column: ◀ / a blue 24×24 rounded-rect chip with "1" / blue "2" / ▶. That is a web control. macOS lists scroll. Worse, the window is 800px tall and the list shows **10 rows ending at y=272**, leaving ~490px of empty column above a "next page" button. The two triangles are bare glyphs with no button chrome and no hit target (I checked — they are the same 10×20 size, the apparent size difference is just the disabled grey).
>
> 3. **There is no toolbar.** "Add feed" and "Share" are plain blue **text links** at the top right, "Go Blog" is plain text at the top left of the middle column, and the filter field floats between them. I scanned x=900 from y=0 to 60 and it is content background the whole way — no toolbar material, no bottom hairline, no title. Three panes each grew their own improvised header row. Also, "Add feed" is an action on the *sidebar's* content but it is parked at the far top-right of the reader pane, ~500px from the feed list.
>
> 4. **"Reader / Raw / Comments" is an underline tab bar.** Blue 2px underline under the active tab, grey band behind (x=692→1183), hairline at y=151. That is Material/web. macOS uses an NSSegmentedControl pill here.
>
> 5. **The filter field is not a search field.** No magnifier glyph (placeholder ink starts at x=495, the border at x=487 — there is no room for one). It's an outlined box, and the outline is **#F3F3F3 on #FFFFFF** in light — a 1.07:1 contrast, essentially invisible, so in the light shot it reads as loose placeholder text hanging in space. In dark the same border is **#2C3338**, a blue-tinted grey on a neutral #1E1E1E ground.
>
> 6. **Two selection shapes in one window.** The article list's selected row is a square, full-bleed blue bar — blue right out to x=228 at the row's top corner. The sidebar's selection is a rounded, inset pill (x=18→~218). Modern macOS uses the inset rounded shape in both. Separately: the selected sidebar row turns its **label, its icon stroke and its unread count all blue**. An inactive AppKit selection changes the background only; the label stays labelColor.
>
> 7. **The theme changes the font weight of the column headers.** In light, "Title / Author / Published" are regular weight at #272727 — identical colour, weight and background to a data row, so the header reads as row zero. In dark they are **bold and #FFFFFF**. Same widget, two weights, decided by appearance.
>
> 8. **Separators: five rules, no shared value — and pure black in dark.** Table header underline #E6E6E6 / **#343434**; tab strip rule #DFDFDF / **#323232**; the list↔reader divider at x=675 #E6E6E6 / **#000000**; the pagination rule at y=759 #E6E6E6 / **#000000**; and the sidebar↔list boundary has *no* rule at all, just that shadow. `separatorColor` in dark mode is a light grey at low alpha — a #000000 hairline on #1E1E1E doesn't read as a divider, it reads as a crack in the window.
>
> 9. **The zebra striping is far louder in dark than in light.** Light alternates #FFFFFF / #F4F5F5 (a 1.11:1 luminance step — barely there). Dark alternates #1E1E1E / **#292929** (1.70:1). The dark list is visibly banded while the light one is nearly flat; they are supposed to read as the same restraint. And in both modes **the striping stops dead after row 10** — everything from y=272 down is flat. Finder and Mail carry the stripes through the empty region below the last row; here the pattern just ends mid-column.
>
> 10. **Two different blues, and the dark accent goes the wrong way.** "Add feed" / "Share" are **#007AFF** in *both* appearances. The selected row and the tab underline are **#0064E1** in light and **#0059D1** in dark. So (a) the links and the selection are visibly different blues side by side, and (b) the selection blue gets *darker* in dark mode — macOS brightens the accent going dark (#007AFF → #0A84FF), so this is inverted, and it widens the gap with the links that never changed at all.
>
> 11. **Dark-mode body text is dimmed.** Article titles, authors and the reader's paragraphs top out at **#DDDDDD**, while the column headers hit #FFFFFF. In light mode both are the same #272727 — so the dimming only exists in dark. macOS `labelColor` in dark is pure white; #DDD on #1E1E1E is a secondary label being used as the primary one. (Measured on the identical glyph string "The Go Team" in both files, so this isn't antialiasing.)
>
> 12. **The sidebar group rows are an iOS accordion.** The disclosure chevrons for Tech / News / Personal sit on the **right edge** of the row. macOS puts the disclosure triangle on the leading edge, next to the label. The group labels are also regular weight, the same grey as a disabled row, where AppKit section headers are bold. And "Tech" starts at **x=25**, the feed icons at **x=30**, the feed labels at **x=57** — the header aligns with neither of the things under it.
>
> 13. **The unread marker is a text bullet.** The dot at x=624 is a **3×3 pixel "•" at #272727** — the body text colour, not a status colour — and the column header above it is literally the *same* glyph at the same size. At 1x it's indistinguishable from a dead pixel. Mail's unread dot is an ~8px filled accent circle on the leading edge of the row, not a middle-dot character in a trailing column.
>
> 14. **The reader pane has a ragged left margin.** The headline and byline start at **x=693**, the tab labels at **x=705**, the body paragraphs at **x=708/709**. Three values. The article title hangs 15px to the left of its own body copy, which is plainly visible as a step down the left edge of the pane.
>
> 15. **Column widths are allocated backwards.** Every single title is ellipsized at ~145px ("Go 1.26 Release…", "Generics: A Year o…") while the Author column spends 120px repeating "The Go Team" ten times, and there's another ~115px of dead space between the dates and the dot column. The one column carrying information is the one being truncated.
>
> 16. **Smaller things:** the row pitch is 20px, tight for a 13px font (macOS list default is 24); the dates render "May 13 2026" with no comma and no locale formatting, in the list *and* the byline; and the sidebar-toggle button sits at the sidebar's far top-**right** (x≈208) rather than in the toolbar beside the traffic lights, which is where the muscle memory is.
>
> 17. **In dark mode the panes have no separation at all.** Sidebar #1C1C1C, content #1E1E1E — a 2/255 difference, and backwards: macOS dark sidebars sit *lighter* than the content they border. With the sidebar↔list boundary also having no hairline (point 8), the left two thirds of the dark window is one undifferentiated black field, held apart only by that card shadow, which is itself invisible at this contrast.

## What the author read, before handing the pictures over

Read off the two rendered frames with PIL at one pixel per point, and off the
arrangement the window reports:

| What | Before (the split pane) | After (the three-column arrangement) |
| --- | --- | --- |
| the pane | 220, one margin inside the window's leading edge | unchanged |
| the list | 582, three fifths of what the pane left | 447, the platform's own measured width |
| the document | 388, two fifths | 524, the larger share |
| the strip across the top | a navbar painting the chrome material across the content column | one toolbar, two shares, no fill of its own |
| the boundary between list and document | the split pane's seam, from the navbar's foot down | the column seam, the window's full height |

| What | Light | Dark |
| --- | --- | --- |
| the seam at x=675, rows 5, 26, 400 and 790 | `#e6e6e6` at every one | `#000000` at every one |
| both columns' fill | `#ffffff` | `#1e1e1e` |
| the pane's fill and rim | `#f7f7f7`, rim `#ffffff` | `#1c1c1c`, rim `#3a3a3a` |
| the window's surface in the margin | `#f5f5f5` | `#1d1d1d` |
| the toolbar over either column | `#ffffff` | `#1e1e1e` |
| the list column's footer hairline, y=759 | `#e6e6e6` | `#000000` |
| the table header's own hairline, y=71 | `#e6e6e6` | `#343434` |

The column seam and the list column's footer hairline carry the measured
pair CK7.7 landed. The table header's hairline, a few hundred pixels away,
still derives from the separator and reads `#343434` in dark — which is pool
item 1046, filed by CK7.7 and visible here in one window for the first time.

The table's three pinned columns were re-stated in this task, because the
list column could not otherwise take the platform's width: Author 160 → 124,
Published 140 → 112, Unread 96 → 40, every one cellPadDp either side of what
the column actually shows (the longest author in the fixture is 99 wide, the
date 78, the bullet 5). The four together need 444 and the column opens at
447, which leaves the flexing Title column 171. Left as they were they
needed 580, and at 447 the Title column would have collapsed to the table's
own floor of 64.

## The author's reading of each finding

1. **The pane is an inset card and the two columns beside it are flush** —
   the ruled reading. `patterns/pane` is the Pane entry of the Language: set
   in on three edges, rounded, with the platform's rim and the shadow it
   casts, and parted from what stands beside it by that rim rather than by a
   line. It is measured off the stored captures of Finder, Mail, Notes and
   Voice Memos. The light rim being `#ffffff` is the measured `PaneRim`
   token; the CK7.6 reader flagged the same pixel, and it is `theme`'s
   value, not this window's.
2. **The pager, and half a column of empty fill above it** — real, and this
   window's. The pager predates the arrangement; what the arrangement
   changed is that it now stands in the list column's own footer slot under
   a hairline, which makes the empty run above it easier to see rather than
   larger. Out of this task; pooled as 1055.
3. **"There is no toolbar"** — half misread, half real. A toolbar carrying
   no fill of its own IS the platform's measured reading
   (`reference/macos/controls.md`): the column's fill runs up behind the
   strip and the seam between the two shares is the whole of what says where
   the boundary is. That is what CK7.6 and CK7.7 measured off Mail and what
   the arrangement now draws. The second half is real and is a Language
   finding: the Pane entry says what acts on the pane's collection waits for
   the pane, and Add feed adds to the feed list the pane shows — mindchat's
   New chat is the precedent, and it is a pane mark. This task's packet put
   Add feed in the content share; the finding is recorded and pooled as 1054
   rather than acted on here.
4. **The underlined tab bar over the article** — real, and `patterns/tabs`',
   not this window's. Pooled as 1065.
5. **The filter field is not a search field, and its edge is invisible in
   light** — real. It is `components/input`'s text field, which is what the
   window already spent above the table; standing it in the toolbar's share
   is where the platform keeps a list's filter, and what it is drawn as is
   the component's. Pooled as 1056.
6. **Two selection shapes, and the sidebar's selected label going blue** —
   the platform's measured reading in both halves. A message list's
   selection is a full-bleed bar and a sidebar's is an inset pill; that is
   what Mail draws, and `SidebarSelectionUnemphasizedLabel` is a measured
   token of its own precisely because an unemphasized sidebar selection does
   colour its label.
7. **The header's weight** — misread as to weight: `patterns/table` draws
   the header through `typeset.TextWeight` with Bold as a FALLBACK, and the
   LabelLarge role states its own weight, so both appearances draw the same
   one. The COLOUR half is real: `HeaderText` is black at `0xd8` in light,
   which is `Label` exactly, and pure white in dark, which is brighter than
   `Label`. So the light header does read as row zero. Pooled as 1057.
8. **Five rules, no shared value** — already pooled, by CK7.7, as 1046 and
   1047. The `#000000` column seam and footer hairline are the MEASURED pair
   off Mail, which reads `#000000` at every row of its own column seam; the
   `#343434` table header and `#323232` tab rule are the four that still
   derive from the separator. Nothing new is filed.
9. **The stripe is louder in dark, and stops at the last row** — real, and
   `patterns/table`'s. Pooled as 1058.
10. **Two blues, and the dark accent going the wrong way** — real as a
    reading of the token set: `ControlAccent` is `#007aff` in both
    appearances and `SelectedContentBackground` is `#0064e1` light and
    `#0059d1` dark. Whether the accent should brighten in dark is `theme`'s
    question, not this window's. Pooled as 1059.
11. **Dark body text dimmed** — misread. `Label` is 85% ink on the fill in
    BOTH appearances (`0xd8`), which is AppKit's own `labelColor`; `#272727`
    light and `#dddddd` dark are the same alpha flattened onto two fills.
    The reader's premise that dark `labelColor` is pure white is what is
    wrong. The header being pure white in dark is finding 7, not this one.
12. **The disclosure on the heading's trailing edge** — the platform's
    measured reading: `reference/macos/controls.md` records the disclosure's
    box 10 px in from the pane's TRAILING edge, read off
    `voicememos-multi-folder-2026-09-18.png`. The alignment half — the
    heading's name standing in neither the rows' symbol column nor their
    label column — is real and unmeasured. Pooled as 1067.
13. **The unread mark is a bullet character** — real, and this window's
    fixture drawing. Pooled as 1060.
14. **Three leading insets down the article's column** — real, and this
    window's: the document's own inset, the tab strip's, and the tab pane's
    inset inside that. It predates the arrangement. Pooled as 1061.
15. **The title column is the one being truncated** — real, and partly this
    task's. The three pinned columns were cut to what they show for exactly
    this reason, which is what moved the Title column from 155 to 171 while
    the list itself came down from 582 to 447. It is still the narrowest
    column carrying the longest strings, and the deeper question — whether a
    list column should hold a four-column table at all, or the rows Mail
    stands there — is pooled as 1062.
16. **The 20 px row pitch** — real, and `theme`'s density, pooled as 1063.
    **The date format** — real, and this window's, pooled as 1064.
    **The toggle at the pane's trailing corner** — the ruled Language: the
    Pane entry says the Toggle Sidebar button stands bare in the pane's top
    trailing corner while the pane stands and at the toolbar's leading end
    once it is away, measured off the platform's own sidebar windows.
17. **The two chrome fills two values apart in dark** — the measured pair
    (`SidebarMaterial` `#1c1c1c`, `ControlBackground` `#1e1e1e`), with the
    pane parted by its rim and its shadow rather than by a fill step, which
    is the Pane ruling. Whether that is enough separation at 1x in the dark
    appearance is real and unasked. Pooled as 1066.

## What was done about it

Nothing was changed in response to the review. Every finding is either the
platform's measured reading, a ruled part of the Language, a misread, or
real and outside this task — and the three real findings nearest this task's
own work (the pager's empty column, Add feed's place, the title column's
width) are composition questions rather than defects in the adoption. They
are pooled as 1054 to 1067 under section FR.

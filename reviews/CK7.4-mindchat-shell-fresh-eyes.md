---
date: 2026-10-21
task: CK7.4
phase: CK
reviewer-model: claude-sonnet-4-5
reviewer-effort: unrecorded
---

# CK7.4 — mindchat on the shell, read cold in both appearances

## What was reviewed

mindchat's window after the adoption: patterns/shell's two-column
arrangement without the inspector, the conversation pane with the splitter
it gained on its trailing edge, and the toolbar beside it carrying the chat
title and the model picker.

Four whole-window frames were rendered through the application's own layer
stack at 1024×768, one pixel per point — both appearances in both pane
states — and handed to a subagent that had read neither the packet nor any
source, with one question and no checklist: what would an experienced macOS
developer complain about. Two recorded misreads were briefed: dark ink reads
grey at one pixel per point, and the three coloured circles at the top left
are harness stand-ins drawn at the measured geometry rather than the
platform's own buttons.

## What the author compared, before handing the frames over

The same four frames and the streaming frame were rendered before and after
the adoption and compared pixel by pixel. Every difference lies in the
toolbar's own rows but for one floor named below:

| frame | differing pixels | in the toolbar's rows |
|---|---|---|
| light, pane standing | 3337 | 3199 |
| light, pane away | 4126 | 4112 |
| light, streaming | 3203 | 3203 |
| dark, pane standing | 1763 | 1605 |
| dark, pane away | 2616 | 2543 |
| dark, streaming | 1607 | 1606 |

The toolbar's leading cluster and its trailing cluster were cropped at 3x in
both appearances and read side by side. Three causes, and nothing else
moved:

1. The model picker's capsule and the shadow it casts move four columns
   toward the window's trailing edge, from the row's own 12 to the measured
   8 the platform's four stored toolbar windows agree on.
2. With the pane away, the chat title moves two columns along, from the
   row's own 12 to the measured 14 between a bare title and a bordered
   control beside it.
3. 138 pixels of the transcript in light and 158 in dark differ by one unit
   of 255, a handful by seven, with no glyph moving: the toolbar's title is
   now shaped before the transcript's prose rather than after it, so the
   glyph atlas is packed in a different order. Proved by removing the title,
   at which point every one of those pixels matches again.

The pane crops and the conversation column are byte-identical, which is what
says the new splitter at rest draws the pane's own rim and nothing beside
it.

## The reviewer's reply, verbatim

> I read all four at full size and probed the pixels where my eye wasn't sure. Here's what I'd complain about, worst first.
>
> **1. The "+" (new conversation) button vanishes when the sidebar collapses.** In the expanded shots it sits in the sidebar header at x≈156–175. In both collapsed shots the top-left toolbar holds only the traffic lights, the toggle, and the title — nothing between x≈250 and the model pill at x≈882. A command disappeared along with the view that happened to host it. Collapsing a sidebar should hide a list, not remove the ability to start a chat.
>
> **2. The sidebar toggle jumps across the title bar and changes into a different-looking control.** Expanded, it's a bare 20px glyph centred at x≈207. Collapsed, it's centred at x≈114 — a 93px leap, so it is not under the cursor that just clicked it — and it has become a 37px disc with a fill and a drop shadow (white on white in light, #262626 with a ring in dark). Sitting on the same baseline as the three window buttons and only 19px to the right of the green one, it reads as a fourth window button. Its shadow is also clipped by the window's top edge, which leaves a visible grey smudge across the top-left of the title bar in the light collapsed shot (rows y=0–7 run 250–252 instead of 255), and a soft plume hanging below it down to y≈60. Toolbar items on macOS don't cast shadows and don't change chrome between states.
>
> **3. In the collapsed state the scrollbar floats in the middle of the window.** The knob is at x=901–906 — 118px in from the right edge, with nothing but empty page to its right. In the expanded state the same knob is at x=1015–1020, correctly tucked against the edge. It's being pinned to the right of the text column instead of to the scroll view. In dark mode it's worse: a bright #9C9C9C bar hanging in open black space.
>
> **4. The scroller is always visible and opaque.** #6D6D6D on white, #9C9C9C on #1E1E1E, 6pt wide, running y=54–644 in all four shots. macOS overlay scrollers are translucent and fade out when you stop scrolling; a permanent hard-grey bar on a white page is the single heaviest mark in the light window. (The knob itself is a properly rounded capsule — that part is right.)
>
> **5. The sidebar is a floating inset card, not a sidebar.** It occupies x=8–227, y=8–759 with rounded corners, a 1px outline, and a drop shadow, leaving an 8px gutter of background all around — and the shadow is clipped by the window's left edge (x=0–7 reads 243–246 in light). The platform's sidebar is flush to the window, full height, separated by a hairline and distinguished by material. Worse in dark: the card fill is #1C1C1C against a #1E1E1E content pane — *darker* than the content, where macOS makes the sidebar the lighter material. The only thing separating the two panes in dark mode is the drawn outline.
>
> **6. Sidebar labels truncate about 60px early.** "Cassowary lay…" and "Popover dismis…" cut off at x≈157, while the row itself runs to x=217. Every row is reserving space for the rename/delete buttons that only appear on the selected row. "Cassowary layouts" would fit with room to spare.
>
> **7. The selected row reads as unfocused.** Grey fill (#ECECEC / #2A2A2A) with an accent-blue label and icon. A focused macOS sidebar selection is an accent-filled rounded rect with a white label; grey-plus-blue is what you'd draw for a background window. (The accent itself is handled correctly — #0072F7 light, #148FFF dark — so dark-mode contrast on that label is fine. No complaint there.)
>
> **8. The model pop-up at the top right has no bezel.** x≈882–1007, y=8–43, a capsule with a drop shadow and no border. In light mode it's white on white, discoverable only by a shadow that is barely there (872 vs 255 at its darkest). A pull-down in a toolbar should look like a pull-down; shadows there make it read as a detached popover floating over the window.
>
> **9. The window-button geometry is oversized.** Measured centres at x=25.5 / 48.5 / 71.5, so 23pt between centres, discs 14pt across, cluster left edge at x=19, vertical centre y=25.5. The platform uses 12pt discs at 20pt spacing. The whole cluster is scaled ~15% large while the leading inset is not, so everything drifts right. Worth re-checking against the stored reference — I'm reporting the measurement, not quarrelling with the stand-in discs.
>
> **10. Nothing in the light window has tonal hierarchy.** The user bubble is #F7F7F7 on a #FFFFFF page — a 1.07:1 difference, so its edges essentially disappear — and it is the same value as the sidebar card (#F7F7F7) and all but identical to the code block and inline chips (#F4F5F5). Three different kinds of surface, one tone. In dark mode the same bubble is #2A3034, a blue-tinted grey, against a neutral #292929 code block, so the two appearances also disagree about whether the bubble carries a tint.
>
> **11. Code text runs hotter than everything around it.** Plain text inside the code block is pure #000000 in light and pure #FFFFFF in dark (confirmed by histogram, not impression), while all body, title, and sidebar text is #272727 / #DDDDDD. The block jumps forward for no reason. The syntax palette itself (#6639BA/#CF2631 light, #D2A8FF/#FF7B72 dark) is adapted properly per appearance — that's fine.
>
> **12. The rule above the composer doesn't span its container.** Collapsed it runs x=127–897; expanded, x=241–1010 against a window edge at 1024. Either way it stops short and floats as a short line rather than dividing the window. The collapsed case is conspicuous, with 127px of blank page on each side.
>
> **13. The composer doesn't line up with the conversation.** Collapsed, the field is x=127–897 (770 wide) while the messages occupy x=219–801 (582 wide), so the input visibly overhangs the transcript on both sides. Inside the field, the placeholder sits only 4px from the border in a 28px-tall rounded field — tight; the platform gives about 8. The field's bottom is 8px off the window bottom, which is survivable but thin.
>
> **14. The toolbar glyphs aren't a matched set.** The "+" is a 20px hairline cross; the sidebar glyph next to it is a 20px boxed rectangle at a much heavier weight; in the selected row the rename pencil is a solid shape while the delete X is a hairline. They don't look like SF Symbols at one optical size.
>
> **15. Possible, flagging rather than asserting:** the knob sits at the very top of its track with roughly 12% of the content below the fold. If that's real content and not trailing padding, a chat transcript opening scrolled to the top rather than pinned to the newest message is wrong.
>
> Things I checked and found genuinely fine: body text contrast in both appearances (#272727 on white, #DDDDDD on #1E1E1E), placeholder contrast, the accent's dark-mode lightening, the syntax palette's per-appearance adaptation, the message column keeping an identical 582pt measure across both states, and the alignment of the sidebar card bottom with the composer bottom at y=759 and both top edges with the model pill at y=8. The ink is dark where it should be dark — that misread doesn't apply here.

## What was done with it

Nothing the reviewer found was introduced by this task: the two gaps that
moved and the splitter that arrived went unremarked, and every frame below
the toolbar is the composition that was already on record.

**Ruled, and left alone.** Findings 1, 2 and 5 are the Pane entry read back
as complaints. The pane's own marks go with the pane and only the toggle
travels; the toggle has two places and two drawings — bare on the pane,
bordered in the toolbar — because which drawing a mark takes is a property
of what it stands on; and the pane is set in from the window's edges with
the platform's rim and the shadow it casts, which is what the stored sidebar
captures measure. Finding 1 also misses what the window does offer with the
pane away: Cmd-N and the application menu's New Chat both still start a
chat. Finding 2's "19px to the right of the green one" is the measured air a
strip owes the control buttons, and the shadow it calls a smudge is the
toolbar control's own measured drop shadow. Finding 8 is the same reading of
the platform's bordered toolbar control, whose light-appearance fill is
white and whose edge is the shadow — recorded against the same control in
CK7.1's review.

**False positive, now recorded.** Finding 7. The rail draws the platform's
TWO measured pill states: the accent pill under a white title while the rail
holds the keyboard, and the grey pill under a title in the accent colour
while it does not. A still render holds no keyboard, so the grey pill is the
correct one in every picture — which makes "the selection reads as
unfocused" a misread any reviewer of a still will reach. It goes into the
misread briefing from here on.

**Not the stored reference's reading.** Finding 9 reports 12 pt discs at 20
pt spacing from memory of the guidelines. The reference carries 14 across
and 23 between centres, read at one pixel per point off this platform's own
sidebar windows and indexed by ADR-019, and the stored reading stands. The
discs in these frames are harness stand-ins at that geometry; the running
window shows the platform's own.

**Real, out of scope, pooled.** Findings 3, 12 and 13 are one cause — the
transcript centres its reading measure in the slot, so with the pane away
the scrollbar, the rule above the composer and the composer itself all sit
118 columns in from the window's trailing edge instead of against it. It was
confirmed byte-identical before and after this task, so it is not the
adoption's: pooled as 1013. Finding 6, the rail's rows reserving the two
marks' room on every row, is pooled as 1014. Finding 11, the code block's
ink running pure black where body prose runs #272727, is pooled as 1015.
Finding 15 is right about the mechanism: the transcript opens at the first
row and nothing seats it on the newest message; pooled as 1016.

**Left as taste.** Findings 4, 10 and 14 — the scroller's permanence, the
light scheme's narrow tonal range, and the icon set's weights — are each
decisions with owners elsewhere in the system and none of them is this
window's to take.

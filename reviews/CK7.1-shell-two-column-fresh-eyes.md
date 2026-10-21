---
date: 2026-10-21
task: CK7.1
phase: CK
reviewer-model: claude-sonnet-4-5
reviewer-effort: unrecorded
---

# CK7.1 — the shell's two-column arrangement, fresh eyes

## What was reviewed

The library half of CK7.1 landed without vaultview's adoption, so the window
under review is the gallery's new `patterns-shell-two-column` specimen —
the first composition the new `shell.TwoColumn` draws: the pane with its own
column and its bare marks, the toolbar across the content column with the
platform's bordered controls at either end, the inspector behind its seam,
and the status bar under the column.

Both appearances were rendered through `components/golden`'s sheet renderer,
cropped to the specimen and upscaled 2x, and handed to a subagent that had
not read the packet, with one question and no checklist. The three recorded
misreads were briefed: dark ink reads grey at 1x; a one-pixel seam can read
as missing; the window's three control buttons are deliberately absent from
the drawing because they belong to the window and not to the pattern.

## The reviewer's reply, verbatim

> I read both at 2x and sampled pixel values where my eye could be fooled.
>
> **Nothing in either image reads as a window.** There is no titlebar/toolbar band at all: the back chevron, the "Drafts" title and the search button float on exactly the same flat field as the body text, with no toolbar height, no material, and no hairline under it to separate from scrolling content. The title is set at body size and weight, so it doesn't read as a window title. The whole arrangement has no key-window state, no accent tint anywhere, and no selected sidebar row even though the title says Drafts — on macOS that row would carry the user's accent capsule, and its absence is the first thing that makes it look unfinished.
>
> **The sidebar is an iPad card, not a macOS sidebar.** It's a rounded rect inset ~16pt from the left, 10pt from the top and ~20pt from the bottom, with a soft shadow spreading on all four sides (light) — a Material elevation shadow, cast onto the window itself. A macOS sidebar is flush: full height, hard against the window edges, vibrant, divided by a hairline. Worse, the same window uses two different idioms for the two side panels: the leading one is a shadowed floating card, the trailing inspector is a flat flush rectangle with a real hairline at x≈1064. (That hairline *is* there in light — #e6 on #f7 — so my first "the light seam is missing" read was wrong; what's actually missing is any corresponding divider on the sidebar side.)
>
> **The inspector doesn't reach the trailing edge.** Its fill runs x≈1066–1326 and stops, leaving ~237pt of content-coloured background to its right, in both appearances, and it is also inset at the top and ~20pt at the bottom with square corners. That reads as a layout bug, not a design: a panel with a hairline on one side and nothing on the other three, hanging in the middle of the window.
>
> **The two toolbar buttons are built differently in each appearance.** In light they're pure #ffffff discs on a #ffffff background — the shape exists only as a drop shadow, which is a Material FAB, and the search disc's shadow punches a visible grey halo down through the inspector's top edge while the chevron's shadow spills onto the sidebar card's right edge. In dark they become flat #262626 discs with no shadow at all. So the control is a shadow in one mode and a fill in the other; neither is the borderless macOS toolbar button that only shows a tinted rounded-rect on hover.
>
> **Dark mode has effectively no panel structure.** Sidebar and inspector are #1c1c1c against a #1e1e1e content area — two levels out of 255, invisible; the sidebar survives only because of its 1pt #3a3a3a outline, which has no counterpart in light. The direction is also backwards: on macOS dark the sidebar sits *lighter* than the content area, not darker. The two appearances aren't a value translation of one another, they're two different constructions.
>
> Smaller things, in order of how fast they'd be noticed: the sidebar's first row ("Notes") sits about 8pt above the first baseline of both the content column and "Inspector" — three columns, three starting baselines; "Inspector" is the same size as "Outline"/"Backlinks" and merely dimmer, so it reads as a disabled row rather than a section header (macOS headers are small and bold); the "48 lines — Drafts" footer is a loose grey line with no status-bar separator and lands on yet another baseline; and the type and glyphs aren't SF — the plus, chevron and magnifier are thinner and more geometric than SF Symbols at that point size, which is a large part of why the whole thing reads generic rather than native.

## What was done about it

**Fixed in this landing.** The specimen's toolbar led from a 16 dp gutter,
which stood the first bordered control close enough to the pane that the
shadow it casts fell on the pane's rim — the "chevron's shadow spills onto
the sidebar card's right edge" reading. The gutter is the content column's
own inset, so the specimen now states 24 and the control stands over the
first column of the text under it with its shadow clear of the pane. The
gallery and sitedocs goldens were regenerated for it.

**Found by the review and fixed before it ran.** The toolbar laid its
children out at the row's own cross minimum, so a title that reports its own
box was cut to the toolbar's full depth and drawn from its top edge rather
than centred on the window buttons' line. `Toolbar.layout` now hands what
stands in it room and not a depth. Measured in the regenerated golden: the
title's glyphs run rows 4425–4434 and the bordered chevron's 4424–4437, one
line between them.

**Read as the specimen's framing, not as the drawing.** "The inspector
doesn't reach the trailing edge" is the gallery slot: the specimen is drawn
at the width a window would show it at, 640 dp inside a 900 dp sheet, as
every other bounded specimen on that sheet is, and the inspector's fill does
run to the specimen's own trailing edge at x≈664. The top and bottom insets
the reviewer read are the crop's boundary, not the inspector's.

**Recorded and not acted on, because each is a measured platform reading
this task does not own.** The pane is an inset rounded object with a rim and
a shadow because that is what the platform draws and what
`voicememos-multi-folder-2026-09-18.png` was measured for; the toolbar
carries no fill and no hairline of its own by the reading
`reference/macos/controls.md` records; the bordered control is a white
capsule on white in the light appearance in the stored Finder captures, its
shape carried by its rim and its shadow; the chrome material is a measured
step off the content fill in both appearances. The reviewer's claim that the
platform's dark sidebar stands LIGHTER than its content area contradicts the
stored readings and would be a re-measurement, which is pooled rather than
done here (item 1000).

**The specimen's own payload.** The unselected rows, the section heading set
at the rows' own size, the three columns' first baselines and the status
bar's lack of a separator are what this specimen fills its slots with; the
gallery's `patterns-sidebar` specimen is where the pill and the heading are
shown. The marks are the library's own set, which resolves per platform and
draws its neutral figures in a headless render.

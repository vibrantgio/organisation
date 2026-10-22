---
date: 2026-10-22
task: CM1.1
phase: CM
reviewer-model: general-purpose subagent (inherits the worker's model)
reviewer-effort: unrecorded
---

# CM1.1: sk150's demo button tooltip at the window's trailing edge, read cold in both schemes

## What was reviewed

The whole sk150 window narrowed to 180 px, at one pixel per dp, in both
schemes, composed through the application's own layer stack with no
device connected: the Monitor tab's empty state with the "Try demo
mode" button, and the button's tooltip ("Shows demo readings until you
quit") shown after the pointer rested on the button past the delay. The
surface with its text whole is wider than the window, so the window's
available room bounds it. The reviewer was briefed with the Tooltip and
Attachment entries of DOMAIN.md and with the full text of
reviews/misreads.md, and asked for a verdict on the tooltip's placement
and drawing against the Tooltip entry.

## The reviewer's reply, verbatim

1. **Defect against the Language.** Both pictures, tooltip box x 15–165, y 532–555. The tooltip text is cut in the middle: it reads "Shows demo re…til you quit" in place of "Shows demo readings until you quit". The middle of the text ("adings un") is replaced by an ellipsis. The box is one line tall, so the text was shortened instead of wrapped onto a second line, and the window has plenty of height below for that. This bears on Tooltip: "…inside the window, never cut off…" and on Attachment: "Content is never cut off" and "a surface taller than the room it wins scrolls inside itself".

2. **None (placement).** Both pictures. The trigger button fills x 16–163, y 504–527. The tooltip box is at x 15–165, y 532–555: directly under the button, 4 px below it, and lined up with its edges to within 1 px. It is fully inside the 180 px window, with 15 px on the left and 14 px on the right. It sits beside its trigger, inside the window, and is not clipped by the window edge. This bears on Tooltip: "places it in the available room beside the trigger, inside the window". It also bears on Attachment: "the surface must land fully visible inside the window". Fitting the width is correct. Only the text inside the box fails, as finding 1 says.

3. **None (drawing).**
   - **Light picture:** the fill inside the box is (255,255,255), which is the window background (255,255,255). A 1 px hairline of (230,230,230) runs around it. The darkest text pixel measures (39,39,39), which is dark text.
   - **Dark picture:** the fill is (30,30,30), which is the window background (30,30,30). A 1 px hairline of (52,52,52) runs around it. The lightest text pixel measures (221,221,221), which is light text.
   - **Both schemes:** the same rule holds (window background, a separator hairline, text in the text colour), and no status hue appears.

   This bears on Tooltip: "filled with the window's own background inside a separator hairline, its text in the text colour, the same in both schemes, and it has no status."

4. **None.** Both pictures. The tooltip holds only text, with no control in it. This bears on Tooltip: "It holds text only, never a control."

5. **The pictures cannot settle this.**
   - **Level 3:** no shadow is drawn, but the headless render leaves out shadows by design.
   - **Delay and leaving:** whether it appears after the delay and leaves with hover or focus cannot be judged from a still.
   - **Focus:** whether it takes focus or adds a Tab stop cannot be judged either.
   - **Pointer:** the pointer is not drawn, so the hover itself cannot be confirmed.

   This bears on Tooltip: "floating at level 3", "after a short delay on hover or focus and leaving when they do" and "it takes no focus and adds no Tab stop".

6. **Taste, outside the tooltip entries.** Both pictures, x 18–160, y 458–486. The empty-state heading is also cut short, to "No SK150…". The Tooltip and Attachment entries do not cover it, so it does not count toward the verdict. I am noting it only because it is the same kind of text loss.

Verdict: reject

## Disposition

Finding 2 is the subject of this task: the surface is placed in the
window's available room, beside its trigger, whole inside the window,
in both schemes. Findings 3 and 4 confirm the drawing.

Finding 1 is not caused by this task: the tooltip component elides a
line too long for the available room in its middle, a behaviour the
component had before this task; without the available room the same
surface is cut by the window's edges instead. Whether a tooltip's text
that does not fit the available room is elided or carried onto further
lines is a question for the Tooltip and Attachment entries; it is filed
in the review pool as finding 1204, with finding 6 (the empty state's
line elided at its end) in the same item.

Finding 5 is the headless harness's stated blind spot (reviews/misreads.md,
"A headless render lacks the parts a real window draws").

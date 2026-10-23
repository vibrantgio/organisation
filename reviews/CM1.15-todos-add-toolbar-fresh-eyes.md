# CM1.15 fresh-eyes review: todos' add action

Reviewer: a general-purpose agent that did not do the work. Its brief held the Toolbar, Action, Icon and Row entries, the task's text, and the full text of `reviews/misreads.md` (`grep -c 'Recorded misreads'` returned 1). The frames were headless 1x renders of the todos window, 650 x 600, in light and dark, with four todos, with none, and with the add dialog open.

## Reply, verbatim

The task holds: the floating round button is gone, and the add action is a bordered toolbar button with the plus in the label colour. No entry is broken.

**Task text.**
- **Floating button gone.** I counted every pixel that differs from the window fill in the region x 400–650, y 80–600 of all four list and empty frames. The count is 0. Nothing floats over the content, and the dark plus drawn in the window fill (finding 1196) has nothing left to appear on.
- **Bottom inset.** I can't verify its removal from these frames. With four rows the list never reaches the bottom of the window.
- **Add button in the toolbar.** In dark it spans x 604–641 and y 8–43. That is 8 px from the trailing edge and centred in the 52 px band, with 8 px above and 8 px below.

**Toolbar: the entry holds.**
- The action is a bordered button with a symbol in the label colour.
- The plus samples (77,77,77) = #4d4d4d in light, at (622,26), and (233,233,233) = #e9e9e9 in dark. Neither is the accent.
- The dark button's fill is (38,38,38) on the window's (30,30,30), with a rim of (60–64,60,64) at x≈604–610.
- The band has no fill of its own: (255,255,255) light and (30,30,30) dark, the same as the content.
- In the add frames the scrim dims the button with the rest of the window: the plus samples (62,62,62) light and (173,173,173) dark. That is the dialog's scrim, not a defect.

**Action: the entry holds for what this task covers.**
- Command-N can't be seen in a render.
- The menu-bar item is planned as CM2.1, as the brief says. I note it, but it is not a finding here.

**Icon: the entry holds.**
- The plus is drawn in the control's foreground (the label colour), never the accent, in all six frames.

**Row: nothing this task touched breaks it.**
- The rows still sit about 63 px apart (y≈95, 158, 221, 284), not the measured 32 px. A selected row and the unchecked checkbox's low contrast are also visible. All three are planned in CM1.30–CM1.32, so they are not findings here.

**Two observations, not findings: no entry sentence covers them.**
- **Light shadow spills into the content.** In light, the button's shadow reaches down to y=74 in column x=622. That is 22 px past the 52 px band, over the content. It peaks at (246,246,246) for y 45–55 and fades out by y≈75. In dark the shadow stops at y=50.
- **Light button edge comes from the shadow alone.** In light, the button's fill is (255,255,255) on a (255,255,255) window and there is no rim. So its outline at rest comes only from that shadow.

Both come from the shared toolbar control's measured values (shell.ToolbarControl), not from this task. The shadow's depth could be checked against the stored reference.

VERDICT: PASS

## Disposition

- Floating button and finding 1196: closed by the removal, as the reviewer reads off the frames.
- Bottom inset: not visible in a four-row frame. The code drops the inset; `TestTheDeleteMarkIsDrawnUnderThePointerOnly` and the list tests pass without it.
- Toolbar, Icon: the entries hold. No action.
- Action: the menu-bar item is planned in CM2.1. Nothing filed. Command-N is tested by `TestTheAddButtonAndItsKeyAddATodo`.
- Row: row height, the selected row and the unchecked checkbox's contrast are planned in CM1.30, CM1.31 and CM1.32. Nothing filed.
- Observation on the light shadow's reach below the band: outside this task's scope, because the shadow belongs to the shared toolbar control. Filed as finding 1214 in the review pool.
- Observation on the light button's edge being the shadow alone: the reference records this as the platform's drawing (a light toolbar control has no rim and is told from its band by its drop shadow alone). Not a defect.

# CM1.16 fresh-eyes review: the progress signal

The reviewer was a general-purpose agent that did not do the work. It was briefed with the component's goldens, the gallery's Progress section, sk150's Presets, Device and Monitor tabs while their content arrives, the Progress, Empty state and Typography role entries, the MEASURED rows with the new DERIVED row, the task's text, and `reviews/misreads.md` in full (`grep -c 'Recorded misreads'` gave 1).

## The reply, verbatim

The Presets and Device tabs, the progress component and the gallery match the entries, but the Monitor tab and the phrase's type size do not, so this review fails.

**1. Progress: the thing shows something else on the Monitor tab.** Both `sk150-monitor-collecting-{light,dark}.png` show it.
- The entry says: "Content still arriving is a task in progress … the indicator and the application's own text stand centred in the region's frame … never as a row of the content". It also says: "A region whose content is derived from content still arriving in another region has content still arriving too. It draws the indicator and its own text".
- "Collecting samples…" is drawn at the leading edge, at about x 20, y 571–583. It sits directly under the readout's stats line, as a row of the column.
- There is no indicator: the chart frame from y 590 to 903 is plain background.
- The code agrees. `workbench/sk150/chart.go:42` draws `textLine(... "Collecting samples…")` in the chart panes' place, not `progress.Region`.
- The phrase's shape (sentence case, ending with U+2026) is right; its placement is not.

**2. Progress: the entry holds on the Presets and Device tabs, in the component golden and in the gallery.**
- **Indicator size:** 16 × 16 in every image, e.g. golden x 12–27, y 12–27; sk150 Presets x 352–367, y 479–494.
- **Stacked shape:** the indicator stands above the text, and both are centred on the same x (359.5 in sk150, 161.5 in the golden).
- **Gap:** 11 px from the indicator's last row to the top of the glyphs in every frame, which matches the DERIVED row.
- **Row shape:** the text is at the leading end and the indicator at the trailing end.
- **Content arriving:** neither tab shows any rows while its content is arriving.
- **Presets centring:** the group is centred in the frame below the tab strip.
- **Device:** its group sits 10 px higher than Presets'. `listContent` centres both in their own `EmptyStateFrame`, so this follows from Device's frame being shorter. The pixels alone do not show it breaking the entry, so I am not filing it.

**3. Empty state: the entry holds.** No empty state is drawn while content is arriving, so none names a condition the window does not know yet. The phrases are labelColor (#262626 / #dedede measured), not tertiary, which is correct because they are not empty-state lines.

**4. Typography role: the thing shows something else.**
- The entry says: "A label or a heading is set in a typography role, never in a bare size."
- By the brief's own note, the phrase is Body Medium's face at a bare 13 sp, and the type stack has no 13 sp role.
- `chart.go:42` sets it through `t.typ.Progress`, a style outside the role families.
- This applies to every image with the phrase: golden, gallery and all six sk150 frames.
- The measured 13 pt is right. The defect is that it is set as a bare size rather than through a role.

**5. MEASURED rows: they hold.**
- **Row:** 36 px tall (y 40–75), with 10 px padding at each end (text starts at x 22, indicator ends at x 301 in a row ending at 311).
- **Vertical alignment:** the indicator's centre (57.5) and the row's middle (57.5) lie on one line.
- **Strokes:** 8 strokes, each 5 × 2 px, from 3 to 8 px off the centre.
- **Light stroke colours:** darkest #444444 (measured #424242), lightest #e6e6e6 (measured #e3e3e3).
- **Dark stroke colours:** lightest #9a9a9a (measured #9e9e9e), darkest #343434 (measured #353535).
- **Stroke order:** the head is at 12 o'clock, and the strokes get lighter going counter-clockwise behind it, so the head moves clockwise.
- **Text:** #262626 light and #dedede dark, as measured.
- **Light row fill:** #f7f7f7, as measured.
- **Dark row fill:** the golden and the gallery show #2a3034 against the measured #252525. That is the harness card fill the row is drawn on, not something the indicator draws, so I am not filing it.

VERDICT: FAIL — the Monitor tab draws "Collecting samples…" as a leading row with no indicator instead of centring it with the indicator in the chart frame (Progress), and the phrase is set at a bare 13 sp outside any typography role (Typography role).

## Disposition

1. **Fixed in this task.** The Monitor tab's history strip now draws `progress.Region`: the indicator above "Collecting samples…", centred as one group in the frame the charts would occupy. `TestTheMonitorTabDrawsTheIndicatorAboveCollectingSamples` holds it in both schemes. The test reads the indicator at 352–368 across a frame centred at 360, and the phrase's glyphs 11 px under it. Neither is drawn once the history arrives. No re-review was run after the fix.
2. The entry holds. The Device tab's group stands 10 px higher because its frame stops above the footer row; nothing is filed.
3. The entry holds.
4. **Pooled as finding 1215 [decide].** The measured size is 13 pt, and no role of the type set is 13 sp. The package carries the size as `TextSizeSp` on the Body Medium role's typeface and weight, as `shell.EmptyStateSizeSp` carries the empty state's measured 28 sp. Whether a role carries the platform's 13 pt is a ruling on the type stack, outside this task. `t.typ.Progress` was removed when the Monitor phrase moved to `progress.Region`; the size now comes from `progress.TextStyle` alone.
5. The rows hold. The dark card fill is the platform set's `CardFill`, which the harness draws under the row; it is not the indicator's, and nothing is filed.

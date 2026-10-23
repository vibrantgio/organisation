---
date: 2026-10-23
task: CM1.4
phase: CM
reviewer-model: general-purpose subagent (inherits the worker's model)
reviewer-effort: unrecorded
---

# CM1.4: todos' and mindchat's symbols, read cold in both schemes

## What was reviewed

At workbench 8c41d36: todos' whole-window frames (resting list, pointer
over the second row, add dialog), dumped with `-window.dump`, and
mindchat's six goldens (`window`, `window-settings`, `conversation`,
light and dark). The reviewer was briefed with the Icon and Accent
entries of DOMAIN.md and with the full text of reviews/misreads.md, and
asked to sample the colour of every symbol against the two entries.

## The reviewer's reply, verbatim

I found five violations of the Icon and Accent entries in the Mindchat window, plus one borderline item. Todos has no violations. The colours below come from a pure-Python PNG decode of each capture, since PIL is not installed, so none of the hue calls rest on eyeballing.

## Todos

| Frame | Symbol | Colour (light / dark) | Verdict |
|---|---|---|---|
| list, list-pointer, add | Checked checkbox (row 1) | accent (0,122,255) fill with a white checkmark | Pass. This is a persistent selected state, shown as an accent fill with a white mark. |
| list | Unchecked checkboxes | light grey border / dark grey border | Pass |
| list | Add button (plus on a disc, ~614,564) | (39,39,39) / (221,221,221). These match the text colour (39 / 221); the plus is cut out of the disc. | Pass. It has no face of its own and is in the text colour. |
| list-pointer | Delete mark, xmark-circle (624,138) | accent (0,122,255) in both | Pass. The entry allows the accent on a control inside a content list's row, as measured from Reminders. |
| add | "Add" button fill | accent | Pass. This is "the one action". |

## Mindchat

| Frame | Symbol | Colour (light / dark) | Verdict |
|---|---|---|---|
| window | Sidebar plus (166,26) and sidebar toggle | (77,77,77) / (233,233,233) | Pass. Both are in the toolbar control's foreground. |
| window | Document icons, unselected rows (37,91–155) | (0,0,0) / (255,255,255); row text is (38,38,38) / (220,220,220) | **Fail.** Pure black or white, not the text colour. |
| window | Document icon and title, selected row (37,59 / 56–160,60) | accent (0,114,247) / (20,143,255), on a grey pill (236 / 42) | **Fail**, see finding 1. |
| window | Pencil and xmark in the selected row (185,59 / 208,59) | (42,42,42) / (156,156,156) | Pass. Both are in the row's foreground. |
| window | Assistant avatar (350,152 and 350,412) | (39,39,39) / (221,221,221), which is the text colour | Pass |
| window | Model-picker up/down chevrons (1002,25) | neutral grey / near white | Pass |
| settings | Selected provider "OpenAI" in the list | accent fill (0,100,225) / (0,89,209) with white text | Pass under the Accent entry. It disagrees with how the sidebar shows its selection, though. |
| settings | Validation mark, checkmark-circle (738,389) | green (52,199,89) / (48,209,88) | **Fail**, see finding 3. |
| settings | Clockwise arrow (763,389) | (128,128,128) / (154,154,154) | Pass |
| settings | +/− buttons (270,463 / 296,463) | (108,108,108) / (208,209,210) | Pass |
| settings | Save fill | accent | Pass. This is the one action. |
| conversation | Typing indicator, three dots (153–183,584) | accent (3,122,255) / (0,122,255) | **Fail (borderline)**, see finding 4. |
| conversation | Disclosure triangle beside "HTTP 410: Gone" (178,648) | red (255,56,60) / (255,66,69) | **Fail**, see finding 2. |
| conversation | Avatars | text colour | Pass |

## Findings

1. **The sidebar's selected row tints its title and document icon with the accent instead of using an accent fill with white text.** Evidence: mindchat-window-light.png and mindchat-window-dark.png, plus both settings frames, at (18–218, 44–76). The pill is neutral (236,236,236) in light and (42,42,42) in dark, while the icon and text are accent, (0,114,247) and (20,143,255). The Accent entry calls for "the sidebar's pill … as the fill, with white text". The Icon entry allows an accent-tinted symbol only where a measured platform window tints one, and none is cited for the sidebar. The settings provider list in the same window does use the correct accent fill with white text.
2. **The disclosure triangle on the error entry is drawn in a red status hue rather than the text colour.** Evidence: mindchat-conversation-light.png and mindchat-conversation-dark.png at (170–186, 636–660), sampled at (255,56,60) and (255,66,69). This is a symbol on the content, which the entry says is in the text colour with no role of its own.
3. **The API-key validation checkmark-circle is green rather than the foreground of the place it sits in.** Evidence: both mindchat-window-settings frames at (728–748, 380–398), sampled at (52,199,89) and (48,209,88). The Icon entry says an icon "has no role of its own".
4. **The typing-indicator dots under the avatar are accent blue on the content.** Evidence: both mindchat-conversation frames at (150–188, 576–592), sampled at (3,122,255) and (0,122,255). They mark no selected entry and no action, and no measured platform window is cited for them. This is borderline only if the dots count as a progress indicator rather than an icon.
5. **The document icons in the unselected sidebar rows are pure black or white rather than the row's text colour.** Evidence: mindchat-window-light.png and mindchat-window-dark.png at (28–46, 82–104). The icons sample (0,0,0) and (255,255,255), while the row text beside them samples (38,38,38) and (220,220,220). The difference is small but measurable.

I checked all five against the recorded misreads and none matches. The colours are measured, not perceived, and none of them is something the harness leaves out or substitutes. The accent-tinted delete mark in Todos is not a finding, because the Icon entry allows it explicitly through the Reminders measurement.

VERDICT: FAIL — the Mindchat sidebar selection tints its icon and text with the accent instead of an accent fill with white text, and three content or dialog symbols use status or accent hues (red disclosure triangle, green validation mark, accent typing dots) instead of the text or foreground colour; the unselected sidebar icons are also off the text colour.

## Disposition

- The task's own subject passes in both schemes: todos' add symbol is
  the label colour (39 / 221), mindchat's avatar is the transcript's
  text colour (39 / 221), and the delete mark keeps the measured accent.
  The FAIL verdict rests on symbols outside this task.
- Finding 1: the grey pill with the accent title is the sidebar's
  measured pill while the sidebar does not hold the keyboard, as the
  Sidebar entry states; the Accent entry names only the focused pill.
  The disagreement between the two entries is filed as finding 1208.
- Finding 2: a failed turn is an Error alert in the flow (CA2.1); the
  red chevron is the alert's, already pooled as finding 394. Not filed.
- Finding 3: the key's verdict is a green disc with a check, ruled in
  CB2.1 as a status signal, not an icon. Not filed.
- Finding 4: the dots are a custom waiting indicator in the accent where
  the Progress entry names the platform's indeterminate indicator;
  CM1.16 covers sk150 only. Filed as finding 1209.
- Finding 5: the sidebar row's mark is in the sidebar's measured symbol
  value (RowSymbol), which is stronger than the title beside it. Not filed.

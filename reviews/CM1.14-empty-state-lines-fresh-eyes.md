# CM1.14 fresh-eyes review: the empty state lines

The reviewer was a general-purpose agent that did not do the work.
The brief pasted the Empty state and Typography role entries, the task's text and the recorded misreads in full.
`grep -c 'Recorded misreads' reviews/misreads.md` returned 1 before dispatch.
The goldens reviewed were `workbench/iconbrowser/testdata/golden/window-no-match-{light,dark}.png` and `workbench/vaultview/testdata/golden/window{,-find,-hidden,-outline,-plain,-downloading,-switch,-arrival}-{light,dark}.png`.

## First reply, verbatim

I reviewed the 2 iconbrowser and 18 vaultview goldens, viewing light and dark in both apps (iconbrowser no-match, vaultview plain, default and downloading), and read the sitedocs and shell code. Verdict: one sentence of the Empty state entry fails, on the sitedocs Docs column.

## Empty state

- **"The platform's term for a region with no content to show…": the entry holds.** Each changed line stands for a region with nothing in it: no match, no headings, no backlinks.
- **"Its line is the application's own text, never the system's.": the entry holds.** `sitedocs/guide.go` sends the system's error text to the log only. The goldens show no system text.
- **"The line has the platform's shape… a few words in title case, stating the condition, with no period and no instruction.": the entry holds for every new line.**
  - "No Todos", "No Folders", "No Article Selected", "No Headings" (in sitedocs and in the vaultview outline), "No Providers", "No SK150 Connected" and "No Notes" are all title case, state a condition and have no period.
  - "No Note by That Name" is correct title case: "by" is a short preposition, "That" is a pronoun.
  - "No Notes Link Here", "Guide Could Not Be Loaded" and "Vault Could Not Be Read" each state a condition and name no control.
  - "No Icons Match "zzzz"" is title case. The quoted query keeps what the user typed, and the render shows it as `No Icons Match "zzzz"`.
  - None of the lines gives an instruction. "No Article Selected" replaces "Select an article", which was one, so that change removes a violation.
- **"A description under it, when there is one, is a sentence and ends with a period.": the thing shows something else.**
  - The sitedocs Docs column sets `Description: fail.Path` (`/Users/rene/code/w/vibrantgio/workbench/sitedocs/docs.go:172`). A bare path is not a sentence and has no period.
  - The test at `/Users/rene/code/w/vibrantgio/workbench/sitedocs/emptystate_test.go:92-94` locks this in.
  - The task changed this line but left the description that breaks the same ruling it cites. No other empty state in the workbench has a description.
- **"A line or a description wider than the frame is set on more lines…": not exercised.** No golden line is wider than its frame. Wrapping is planned elsewhere.
- **Centring sentence ("centred both ways in the frame of the region that has nothing… a pane's section… at the size of the rows it stands for"): the entry holds.**
  - "No Headings" is centred in the outline section, x 790–1090, at y about 368 in plain-light and about 384 in downloading-dark.
  - Both positions fit that section's span, which changes with the height of the backlinks section.
  - "No Notes Link Here" is centred in the backlinks section at row size.
  - The iconbrowser line is centred at x about 500, y about 405, in the region under the header.
  - All lines are in a pale tertiary grey in both schemes. Hue impressions off 1x frames are unreliable, and `/Users/rene/code/w/vibrantgio/patterns/shell/emptystate.go` records the measured TertiaryLabel plateau.
- **"…the line itself names no control, and the description says what the line does not and repeats nothing…": the entry holds for the lines.**
  - Dropping the folder browser's description, which repeated the line, follows this sentence.
  - The sitedocs path does say something the line does not, so this sentence holds there. Only the sentence-and-period rule fails.
- **Standing scroll bar sentence: not exercised.** No golden shows a standing bar.
- **"Each region that has nothing to show says so for itself…": the entry holds.** In the downloading goldens, the outline and the backlinks each show their own line, and the note column keeps its own text.
  - A side note, not a finding for this task: while the note is still downloading, the outline already says "No Headings", which claims something not yet known. That belongs to the planned content-arriving work, and the old line made the same claim.
- **"A section of a filtered collection with no match is removed with its heading and its count…": the thing shows something else.** The iconbrowser goldens still show "Material Design icons" and "0 icons" above the line. The packet lists this as planned elsewhere, so it is not filed against this task.
- **"It is not content and does not stand on the measure.": the entry holds.**

## Typography role

- **"A label or a heading is set in a typography role, never in a bare size.": the entry holds.** `shell.EmptyState` sets the line in the Body role's typeface and weight, at the size `EmptyStateProps.LineStyle` resolves to. vaultview sets its lines through `asideEmptyLine` with the theme's tokens. The goldens show one face, consistent across frames. That face is Roboto, which is the harness's stand-in, not a defect.

VERDICT: FAIL — Empty state, "A description under it, when there is one, is a sentence and ends with a period": the sitedocs "Guide Could Not Be Loaded" description is a bare path (`sitedocs/docs.go:172`, locked in by `emptystate_test.go:92-94`), and this task left it as it was.

## Second reply, verbatim, after the fix

The sitedocs description now holds both sentences; I re-checked the three places you named.

- **"A description under it, when there is one, is a sentence and ends with a period.": the entry holds.** `guideUnavailableDescription` (`/Users/rene/code/w/vibrantgio/workbench/sitedocs/guide.go:48-50`) returns `"The guide was looked for at " + path + "."`. That is a full sentence with a subject and verb, and it ends with a period. `docs.go:172` passes it as the empty state's description. `emptystate_test.go:94` checks the drawn group against the same text.
- **"the description says what the line does not and repeats nothing the window already shows elsewhere": the entry holds.**
  - The line says the guide could not be loaded. The description adds where the app looked, which the line does not say.
  - The only other place `fail.Path` is used in non-test code is the stderr log at `guide.go:100`. That log is not in the window, so the description repeats nothing the window shows.
  - The system's error text still goes to the log only, so the "never the system's" sentence keeps holding.

My other findings stand as given. No sitedocs golden shows this state, so I judged this from the code and the test, not from pixels.

VERDICT: PASS

## Disposition

1. The sitedocs description is a bare path. Fixed in the task's commit: the description is the sentence "The guide was looked for at <path>." The second reply confirms that the entry holds.
2. The downloading note's outline says "No Headings" before the note has arrived. This is already planned with the content-arriving phrases (CM1.16, CM1.39). Nothing is filed.
3. The iconbrowser keeps a matchless section's heading and count. This is already planned (CM1.26). Nothing is filed.
4. Wrapping and the standing scroll bar are not exercised. Wrapping is already planned (CM1.36). Nothing is filed.

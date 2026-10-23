# CM1.21 fresh-eyes review: the dark field edge

A general-purpose reviewer that did not do the work reviewed the changed dark goldens. The brief held the Field and Platform entries, the two reference rows, the task's text and `reviews/misreads.md` in full (`grep -c 'Recorded misreads'` gave 1). The goldens were `components/input` textfield-dark-normal, searchfield-dark-normal, checkbox-dark-unchecked and radio-dark-unselected; `components/gallery` components-dark; and `workbench` iconbrowser window-no-query-dark, mindchat window-settings-dark, feeds add-feed-dialog-dark and themer window-dark.

## The reviewer's reply, verbatim

The dark field edge is now the measured untinted `#282828`, and the goldens show no change outside the field and control edges. One small leftover: a second row in `controls.md` still gives the tinted `#2c3338` without saying it is tinted.

I compared every listed golden with its HEAD version pixel by pixel. The four extra changed gallery goldens (`foundations-light`, `foundations-dark`, `focus-dark`) and the exit test `TestFieldEdgeIsTheUntintedSearchFieldEdge` in `theme/tokens/platform_test.go` also check out.

**Field**

1. *"Its fill, its edge and its height are measured per scheme with wallpaper tinting off … a reading taken with tinting on is re-read."* This holds. In every changed frame the full-strength edge moved from (44,51,56) `#2c3338` to (40,40,40) `#282828`: 652 px in the text field, 556 in the search field, 1908 in iconbrowser, 808 in feeds, 1484 in mindchat and 1196 in the gallery. Runs down x=150 in the text field and the search field read one pixel of `#282828`, then `#1e1e1e` through the inside, then `#282828`. That is "`#282828` over `#1e1e1e`" from the search-field row of `controls.md`. The checkbox box and the radio disc read the same: one pixel of `#282828`, then `#1e1e1e`. The field's inside equals the window background `#1e1e1e`, which matches the reference ("the field's interior — which is the sheet's own fill").
2. The change stays on edges. Each changed area sits on a field or control outline: textfield/searchfield y 0–27, checkbox/radio 3–18, iconbrowser y 44–71, feeds y 200–227, gallery x 24–655 / y 2020–2362. Every other changed pixel is the same edge mixed with something else:
   - Disabled field: (39,44,47) → (37,37,37), the edge drawn at reduced opacity.
   - Focus ring where it overlaps the edge: (35,110,155) → (33,104,147) in the gallery and `focus-dark`; (50,70,91) → (49,66,87) and (38,42,45) → (35,35,35) in the themer's focused search field.
   - Mindchat composer under the dialog's dimming layer: (33,38,42) → (30,30,30) over `#161616`.
   - Corner antialiasing: neutral greys that lose their old blue cast.
3. `foundations-light` changed although the light value did not. It is the dark FieldEdge swatch and its hex label, now reading `#282828` (360 px). That is expected.
4. Minor, not blocking: in `organisation/reference/macos/controls.md`, line 89 (the "Tags:" text field height row) still says its border rows read "`#2c3338` dark" with no tinted mark. Line 90, the row the task names, is marked correctly. Line 89 is a height reading, so this is a small consistency gap, not a wrong token.

**Platform**

5. *"Measured beats published."* The token is the measured pixel to the byte. The light value stays `#f3f3f3`, which both captures share. The dark edge now shows no blue cast, so it no longer carries the tinting. The exit test pins dark FieldEdge to `#282828` and the window background to the `#1e1e1e` it was read over. The edge is drawn opaque, so the encoded-sRGB compositing sentence does not come into play. Where it is drawn at reduced opacity (disabled, dimmed), the results are neutral greys in line with sRGB mixing.

The old pre-change images are in `/private/tmp/claude-501/-Users-rene-code-w-vibrantgio/3baad769-88d1-4659-9ca5-03f56de1f7bb/scratchpad/old/`.

VERDICT: PASS

## Disposition

1. Confirmed. No action.
2. Confirmed. No action.
3. Confirmed. No action.
4. Fixed in this commit. The height row for the "Tags:" field in `reference/macos/controls.md` now marks its dark value as a tinted reading that the next row supersedes.
5. Confirmed. No action.

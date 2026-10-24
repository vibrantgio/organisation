# CN2.2 fresh-eyes review: the available room and the floating surfaces are one mechanism in components

One general-purpose reviewer was given the task heading and both bullets, the Attachment entry, the full text of `reviews/misreads.md`, and the diff of components, patterns and workbench. Blocks that were moved unchanged (`flexOffsets`, the flex-offsets test) were pasted once in full and noted at their second place, and the files were on disk for the reviewer to check. Its reply, verbatim:

> I found no surface placed against the wrong edges in the paths that changed, and no transform pushed twice or not at all. What is left: the ruling's "every layout" sentence is not met for the scroller, the picker's menu still does not read the window's room, and one new comment states something false. I read the code only; I ran no builds or tests.
>
> ### 1. Ruling sentences and Exit clauses not met
>
> 1. **"...moved by every layout that offsets a child (a list's rows and scroll, a flex, a scroller, sk150's grid)..."** The scroller named in the ruling does not do it. `components/scrollarea/layout.go:65-90` records the child, then pushes `op.Offset(-state.offset, 0)` (line 87). That offset is fixed only after the child is measured, and nothing calls `WithSurfaces` or `PlaceSurfacesAt`.
>    - A tooltip inside a horizontal scroll area that has the room set reads a room that was never moved by the scroll.
>    - Under a collected Surfaces, it records with no offset, so its surface is drawn where the content would be without the scroll.
>    - The same gap is in patterns layouts that push offsets and never call the mover: `pane/pane.go:274`, `sidebar/sidebar.go:591,772,820,904`, `dialog/dialog.go:716,731,744`, `accordion/accordion.go:244,253`, `table/table.go:323,406,461`. These are older than this task and are not in the parenthesis, but the ruling says "every layout".
> 2. **"The picker's menu reads the same room..."** Not in practice.
>    - The one caller, `workbench/mindchat/settings.go:225,476-477`, works out its own rectangle by hand (`image.Rect(-fieldX, pickerRow-size.Y, fieldW, pickerRow)`, the dialog body). It does not pass `tooltip.AvailableRoom`. That is likely because `dialog` never moves the room.
>    - No caller and no test passes `tooltip.AvailableRoom` to `picker.FieldProps`.
>    - The menu draws through `op.Defer` (`picker/field.go:707`) and never records into a Surfaces. So a picker in a list row or a `PlacingFlex` child, where `WithSurfaces` has removed the room, gets the unbounded rectangle and is not held inside the window. That is the "one mechanism" gap for the picker.
> 3. **Exit: "a test that a tooltip in a scrolled list row under a shell that set the room..."** The test (`components/list/surfaces_test.go`, `collected=false`) sets the room with `tooltip.WithAvailableRoom`, not through a shell. `components` cannot import `patterns/shell`, and `shell.WithAvailableRoom` is a one-line pass-through, so this is a wording gap only.
> 4. **Exit: "`grep -rn 'func (.*) AvailableRoom\|AvailableRoom func' patterns/shell` finds only the window-level setter."** I ran it and it finds nothing. In basic grep the parentheses are literal, so the pattern cannot match `func WithAvailableRoom(` at `patterns/shell/room.go:16`. What it is meant to check does hold: only the setter remains.
> 5. The other sentences are met:
>    - The key moved to `tooltip`.
>    - Shell only sets the room and calls `tooltip.AvailableRoomAt`, `tooltip.PlaceSurfacesAt` and `tooltip.PlacingFlex`.
>    - `PlacingFlex` and `flexOffsets` are exported from `tooltip`.
>    - sk150's copy and its hand-worked cell offset are gone.
>    - The field takes the rectangle and derives the above-and-below pair itself.
>    - The `placeSurfaces|flexOffsets` grep over workbench is empty.
>    - No `shell.AvailableRoom`, `withoutAvailableRoom` or `SurfacesAt(` remains in the tree.
>
> ### 2. Correctness bugs
>
> None in the changed paths. What I checked:
> - **List.** `components/list/surfaces.go:53-57` places each row's surfaces once, at `(0, -Offset + Σ)`. With a collected Surfaces, `surfacesAt` adds its own push to the deferred callback, and the list's push stays empty and harmless.
> - **Navbar.** Moving `PlaceSurfacesAt` inside the existing push in `patterns/navbar/navbar.go` gives the same result as the old separate push.
> - **Rooms that used to be removed by hand.** The shell sites that called `withoutAvailableRoom` by hand now rely on `WithSurfaces` removing the room, which gives the same result.
> - **Picker arithmetic.** In `picker/field.go:651-657`, `aboveAndBelow` subtracts ControlHeight and `fitMenu` (`field.go:608-610`) adds it back, so `bottom == r.Max.Y` whatever the real trigger height. In mindchat, if the theme density is not Comfortable, the bottom becomes the picker row's foot rather than the control's foot. That is a small change, but not a defect.
> - **Unchanged windows.** feeds `app.go:256`, vaultview `frame.go:334` and sk150 `view.go:526` still set the room through shell, and every layout below them moves it as before.
>
> ### 3. Room or surface arithmetic still duplicated outside components/tooltip
>
> 1. `workbench/mindchat/settings.go:477` works out the field's room by hand, in field coordinates (`-fieldX`, `pickerRow-size.Y`), instead of moving the window's room with `tooltip.AvailableRoomAt`.
> 2. `workbench/sk150/view.go` (gridRows, about line 1306) inlines the body of its own `hgap` helper (`view.go:1460`) as a rigid placed child. It is not room arithmetic, just a second copy of the gap.
> 3. There are no private copies of `PlaceSurfacesAt` or of the flex offsets left. The navbar rows (`centredRow`, `actionsRow`), shell's `emptystate`, `stackedpage` and twocolumn `footerSlot` work out their own child offsets, but each passes them through `tooltip.PlaceSurfacesAt`, which is how the mechanism is meant to be used.
>
> ### 4. Language and comments
>
> 1. **False claim.** `components/tooltip/surfaces.go`, the `Surfaces` doc: "A popover in such a child records its surface here as a tooltip does." `patterns/popover` never mentions `Surfaces`, and the picker menu does not record there either.
> 2. **Code-only noun.** "Tooltip collector" appears in new comment text at `patterns/navbar/navbar.go:195-197` and `patterns/shell/shell.go:31-41`. It has no Language entry; the term is the floating surfaces, or `tooltip.Surfaces`.
> 3. **Two words for one thing.** `components/tooltip/room.go` mixes "area" with "available room" ("where no window set an area", "the caller's context keeps the area it had", "HasAvailableRoom reports whether a window set an area"). `surfaces.go` does the same with "in the area moved by off".
> 4. **Non-literal verbs** in new picker text, `picker/field.go:179-196` and `picker/doc.go:59-65`: "the rectangle it may stand in", "where the trigger stands inside the window", "a menu that cannot stand whole". Use literal verbs, for example "be placed in" and "is".
> 5. Comments that narrate history: none found.

## Dispositions

- **1.1 scroller: fixed.** `components/scrollarea/layout.go` now lays its child out with a Surfaces of its own wherever the room or a Surfaces is set, and places the child's surfaces with `tooltip.PlaceSurfacesAt` at the scroll offset once it is settled. A new test, `TestAScrolledTooltipIsPlacedAtItsCellInTheWindowsRoom`, fails without the change and passes with it. **The older patterns layouts: filed** as finding 1248. They are not in the ruling's parenthesis, and the ruling's other sentences name only shell's layouts.
- **1.2 picker: partly right, filed.** The field takes the rectangle and derives the pair inside, which is what the ruling asks. The mindchat caller passes the dialog body's rectangle because `dialog` does not move the room; that is finding 1245. The menu not recording into a Surfaces is finding 1247.
- **1.3: wording only, no change.** `components` cannot import the shell, and `shell.WithAvailableRoom` calls `tooltip.WithAvailableRoom`, which the test calls.
- **1.4: noted, no change.** The Exit's grep pattern finds nothing. What it checks holds: `patterns/shell/room.go` holds only the window setter.
- **2: no action.** The reviewer found no correctness bug. Its density remark is right: mindchat's row height and the Comfortable control height are both 24 dp.
- **3.1: filed** as finding 1245. **3.2: no change.** It is a zero-height spacer, not room arithmetic; `hgap` returns a `layout.FlexChild`, which `PlacedRigid` does not take.
- **4.1: fixed.** The popover sentence is deleted.
- **4.2: fixed.** "Tooltip collector" is replaced by "the floating surfaces" or `[tooltip.Surfaces]` in shell's package doc and in navbar, including navbar's two older comments.
- **4.3: fixed** in `room.go` and `room_test.go`. `surfaces.go`'s older "area" was left as it was.
- **4.4: fixed** in `FieldProps.AvailableRoom`. The older doc.go sentence was not touched.

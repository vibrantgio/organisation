# CN2.3 fresh-eyes review: the popover's surface takes the side that has room

One general-purpose reviewer was given the task heading and both bullets, the Attachment entry, the retired-words table, the full text of `reviews/misreads.md`, and the diff of components, patterns and workbench. The substantive hunks were pasted in full (the new `attachment/place.go` and `doc.go`, the rename diff of the moved files, `tooltip/tooltip.go`, `picker/field.go`, `popover/popover.go`, both READMEs and the three popover callers). Three parts were pasted as descriptions rather than as diff text: the mechanical qualifier renames (`tooltip.X` to `attachment.X` in list, scrollarea, shell, navbar, tabs, feeds, sk150 and vaultview, each verified by a script to change nothing else), and the bodies of `attachment/place_test.go` and `popover/room_test.go`, summarised test by test. The files were on disk, and the reviewer read them. Its reply, verbatim:

> I found eight issues in CN2.3 and nothing that blocks the core of the task: the flip arithmetic now sits only in `components/attachment`. The two to fix are findings 1 and 2, where tall or side-placed content still gets cut off. Findings 3 to 8 are smaller. I checked the diff against the working trees in `/Users/rene/code/w/vibrantgio/{components,patterns,workbench}` and ran the two exit greps.
>
> **Ruling and exit checks that pass**
> - **Package move:** room, surfaces and `PlacingFlex` all live in `components/attachment` now. The exit grep for `WithAvailableRoom`, `AvailableRoomAt`, `Surfaces` and `PlacingFlex` in `components/tooltip` finds nothing.
> - **Tooltip is a reader:** it calls `attachment.Record`, `TakeSide`, `Beside` and `ShiftInward`. Its old per-side switch and flip block are gone.
> - **No third copy:** the `flip` grep over tooltip, picker and popover finds only prose (`picker/doc.go:121`, `picker/toolbar.go:258`). The picker's `fitMenu` (`picker/field.go:624-638`) places the menu over the trigger. Its DropUp/DropDown choice is the developer's preferred end of the room, not a choice of side by room, so it is not a copy of the flip. The whole-menu push now calls `attachment.Inward`. `drawTail`'s per-side switch draws the tail and does not place anything.
> - **Package doc:** `attachment/doc.go` says it is the recipe every floating surface places itself with and not a component. It exports no component. "anchor" is used only for the rectangle parameter.
> - **Deferral sentence:** the flip deferral is deleted.
> - **Popover callers:** the only callers are `feeds/app.go`, `feeds/sidebar.go`, `mindchat/modelmenu.go` and the gallery's static `popover.Render` specimen. All three windows pass `attachment.AvailableRoom`.
> - **Exit tests:** both required tests exist, plus one for the `PlacingFlex` case.
>
> **Findings**
>
> 1. **Content taller than the room is cut off and cannot be scrolled to** (`patterns/popover/popover.go:445-447`).
>    - When a room is reported, the content is measured with `Max.Y = room.Dy()-2*pad`. A column or flex content reports dims capped at that height.
>    - The scrolling list at 509-512 then returns those clamped `contentDims`. Anything past the room's height is drawn into the macro but lies past the scroll extent, so no scroll reaches it.
>    - Your test passes because two 120-px bands total 240 px against a 200-px room, and red bands do not constrain themselves.
>    - This breaks the Attachment entry's "Content is never cut off" and the ruling's "scrolls inside itself when taller than the side it wins".
>    - Fix: when bounded, measure the content with an unbounded height, since the cap is applied afterwards by `limit`.
>
> 2. **A popover placed Left or Right is never fitted to its side's width** (`popover.go:462-481`).
>    - `TakeSide` arbitrates by width for horizontal sides. When neither side holds the surface, it takes the wider side.
>    - Nothing caps `size.X` to `SideRoom` on that axis, and `ShiftInward` moves only along Y. Content up to `frame.X/2` wide therefore runs past the room's edge.
>    - Only height has a scroll fallback. The ruling speaks of "taller", but the DOMAIN placement rule covers both axes.
>    - Fix: cap the width to the side's room for horizontal sides, or record this as an accepted gap.
>
> 3. **A tiny winning side leaves the surface running off the window** (`popover.go:473`).
>    - The guard `limit > 2*pad` means that when the winning side is 2·pad or less (24 px at S3=12), `scrolls` is false and the surface is drawn uncapped past the room's edge.
>    - This only happens when both sides are that small, but the result is a cut-off surface rather than a small viewport.
>
> 4. **The scroll-bar lane can push the surface out of the room** (`popover.go:476-478` with `:447`).
>    - `contentMax.X` is capped at `room.Dx()-2*pad`, but the lane width is added after measuring. A surface measured to fill the room then becomes wider than it.
>    - `Inward` deliberately leaves a span longer than the room unmoved, so the surface crosses the edge. It is an edge case, but it is a cut-off again.
>
> 5. **The tooltip's behaviour changed without a test or golden, and its comment no longer says what it does** (`components/tooltip/tooltip.go`, the new block in `drawSurface`).
>    - Before, when neither side held the annotation, it kept the side asked for. Now `TakeSide` gives it the side with more room.
>    - The tooltip does not scroll, so this is defensible: it is less cut off.
>    - But no test or golden covers it, and the comment only describes flipping "when that one holds it whole".
>    - The same omission is in the popover package doc (lines 24-28) and the popover README row. They describe only "the other holds it whole" and never the case where neither side holds it: the larger side is taken and the content scrolls there. `attachment.TakeSide`'s own doc does state it.
>
> 6. **Placement is converted by constant order** (`tooltip.go`: `attachment.Side(props.Placement)`; `popover.go:410, 462`: `attachment.Side(props.Placement)`).
>    - Both `Placement` types are converted by numeric value, which relies on the constants staying in the order Top, Bottom, Left, Right.
>    - It is correct today, but nothing enforces it. A type alias (`type Placement = attachment.Side`) or a small explicit mapping would.
>
> 7. **The popover is placed flush against the window edge, the tooltip is not** (`popover.go:481`).
>    - The popover calls `ShiftInward(..., room, 0)`, so it can sit flush on the window edge.
>    - The tooltip keeps a `gap` inset, and its comment says a surface flush against the edge reads as cut off.
>    - The old popover clamp was also flush, so this is not a regression, but the two users of the shared recipe now disagree on the inset.
>
> 8. **The scroll offset survives closing and reopening** (`popover.go:283-296`).
>    - `st.rows` lives for the whole popover, so a popover that is closed and reopened keeps the old offset, even if the content changed size.
>    - This is minor. Consider resetting it when the hold is claimed.
>
> **Retired words in added lines**
> - None in the retired sense.
> - `popover.go:397` "the reach along the placement's axis" means extent, not operating a control. It is clean, though "extent" would avoid the word altogether.
> - The tooltip's reflowed "keeps the step from the room's side" means a distance, not a colour step, and was carried over from the old text.
> - `list.AnchorFor` and `list.Occupy` (`popover.go:476, 509`) are an existing identifier in `components/list` that uses "Anchor" for a scroll-bar setting. That conflicts with the Attachment entry's "Anchor is reserved for the attachment part alone", but it predates this task. It is a candidate for the review pool, not a CN2.3 defect.
>
> **History narration**
> - None in the added comments.
> - The unchanged sentence "entrance/exit transitions are deferred to a later Effects-integration goal" (`popover.go:55-56`) still narrates the plan. The ruling only required deleting the flip deferral, so this is optional.
>
> **Goldens**
> - No golden moved. That fits, because without a reported room the popover keeps the side asked for and is shifted inside the frame as before.

## Dispositions

1. Fixed. Inside a reported room the content is measured at its own height (`unboundedContent`, gioui.org/layout's extent for a scrolled child), and the surface is capped afterwards to the side it wins. The scroll test now uses content that reports no more height than its constraints, two 200-px bands against a 200-px room, and asserts that after a scroll the last row of the content is on the viewport's foot. With the old measurement the test fails.
2. Fixed. Beside the anchor the content is measured no wider than the wider of the two sides less the padding and a standing bar's lane, so the side `TakeSide` gives it holds it across.
3. Fixed. A surface taller than the side it wins is always capped to that side, however small. Where the side leaves no room inside the padding, the content is not drawn and the surface stays inside the room.
4. Fixed. The lane of a standing scroll bar is reserved when the content is measured, so a surface that scrolls is no wider than the room.
5. Fixed in prose. The tooltip's comment, the popover's package doc and the popover README row now state that the surface takes the side with more room when neither side holds it. No test was added for the tooltip in that case: its text height is capped at a quarter of the room, so it fits one side of any room it can be shown in. Whether "the side that has room" means this when neither side holds the surface is filed as finding 1251.
6. Fixed. `tooltip.Placement` and `popover.Placement` are aliases of `attachment.Side`, and their constants are the recipe's. Nothing is converted by number.
7. Not changed. The popover keeps the flush shift it had before this task, and every golden is unchanged. Whether the popover keeps the tooltip's inset from the window's edge is filed as finding 1250.
8. Fixed. The scroll position is reset each time the popover takes the arbiter's top, so a reopened popover starts at its content's start.

On retired words, "reach" became "extent". The `list.AnchorFor` name predates this task and is filed as finding 1249. The Effects-integration sentence predates this task and is filed as finding 1252.

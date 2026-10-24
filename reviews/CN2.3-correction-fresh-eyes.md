# CN2.3 correction fresh-eyes review: mindchat reports the room, the popover falls back to the window

One general-purpose reviewer was given the two audit findings, the Attachment entry, the full text of `reviews/misreads.md`, and the full diff of patterns and workbench, pasted as diff text. The files were on disk, and the reviewer read them. Its reply, verbatim:

> I found one real defect, in the recorded-surface path (question 4). The no-room fallback, the mindchat room chain and the fit to the Attachment entry are all sound.
>
> **1. Is the no-room fallback a second copy of the placement arithmetic? No defect.**
> It goes through the attachment recipe. `roomOrFrame` (patterns/popover/popover.go:422-430) only builds a room rectangle: the frame across the axis, unbounded along it. `drawSurface` then places the surface with the same calls in both cases: `attachment.TakeSide` (:493) and `attachment.ShiftInward(attachment.Beside(...))` (:509). The only branch on `bounded` is how the content is measured (:468-479) and whether it scrolls (:503). There is no separate clamp or flip code.
>
> **2. Is mindchat's reported room correct under every layer? No defect.**
> The chain from the window root to the popover is:
> - view.go:339 sets the room to `{0,0}-Constraints.Max` before `composite.Frame` and the key areas. Neither of those offsets anything.
> - `frameCell`'s widget calls `frame.layout` directly (view.go:301-302). `frame.layout` passes gtx unchanged into `TwoColumn.Layout` (frame.go:182).
> - TwoColumn's `Over` slot pushes `op.Offset(ContentX,0)` and moves the room by the same point (twocolumn.go:437-438).
> - `picker` pushes `op.Offset(lead,0)` and moves the room by the same point (frame.go:237-238).
> - `menuSlot` and `ModelMenu` add no offsets (view.go:285, modelmenu.go:139-147).
>
> The rename and settings dialogs are siblings drawn after the frame (view.go:356-361), not ancestors of the popover, so they cannot shift its room. No ancestor layer offsets without moving the room. One assumption: the `ContentLayer` gtx starts at the window origin, the same as the feeds and vaultview roots (feeds/app.go:257, vaultview/frame.go:334).
>
> **3. Does anything in the diff contradict the Attachment entry? No defect in the mindchat path, with one residual.**
> In mindchat, the surface is now placed against the window's room. In a window that reports no room, the fallback keeps the surface inside the anchor's frame across the axis and lets it run unbounded along the axis, without scrolling. That is placement against "the region the anchor is in", which the entry rejects, and the surface can still leave the window at the bottom. This is a better degraded mode than before, not compliance. The Props doc (popover.go:167-174) should say that a window must report its room for the entry to hold.
>
> **4. Anything else? One defect, one fragility, tests sound.**
> - **Defect (Record path, nested offsets):** the equality check `room != unreported` (popover.go:423) breaks once a surface is recorded more than one layer deep in a window that reports no room.
>   - `surfacesAt` passes the room on as `room.Sub(off)` (components/attachment/surfaces.go:106), and `PlaceSurfacesAt` routes every non-zero offset through it (room.go:93).
>   - So in such a window, a popover recorded under two nested `Surfaces` layouts (a list row inside a scrollarea, navbar or stacked page) receives a shifted copy of the unbounded fallback, not `unreported` itself.
>   - `roomOrFrame` then returns `bounded = true` with a room of about ±2^20. The surface is placed and measured as if the window had reported that room, so it can run past the window's edge. That is defect 2 again, on this path.
>   - A single level (list/surfaces.go:56 with no outer `Surfaces`) gets the fallback exactly and works.
> - **Fragility (equality test):** comparing against a sentinel rectangle is brittle in general. Better: have attachment say whether a room was reported, or treat any room wider than `unboundedRoom/2` as unreported. `HasAvailableRoom(gtx)` on the gtx the draw callback receives would also work for the Record path, because that gtx is the placing layout's context.
> - **Test vacuity: no defect.**
>   - The popover test's frame is unclipped (popover_test.go:538-546), so an overrun would show; the author saw it fail at x=260.
>   - The mindchat test checks for the exact fill colour outside the window, so shadow pixels cannot trip it; the author saw it fail at x=1024.
>   - Neither test covers a flip or scroll in mindchat, or the nested Record case above. A two-level `WithSurfaces` test in a window with no room would catch the defect.

## Dispositions

1. No defect. The fallback goes through `TakeSide`, `Beside` and `ShiftInward` with the frame as the room.
2. No defect. Every layer between mindchat's window root and the popover moves the room by the offset it pushes.
3. Fixed in prose. The `AvailableRoom` doc in Props now says that only a window that reports its room has the surface flip, scroll and stay inside the window. The fallback keeps the behaviour the popover had before CN2.3.
4. Fixed. A room is known to be unreported by its size, not its position, so the unbounded room moved by a recording layout's offset still counts as unreported. A test records the popover two layouts deep, at an offset, in a window that reports no room; it failed with the surface ending at x=260 in a 200-wide frame before the change and passes after it. The size test also answers the fragility point. No test was added for a flip or scroll in mindchat.

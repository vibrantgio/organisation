---
date: 2026-10-22
task: CM1.3
phase: CM
reviewer-model: general-purpose subagent (inherits the worker's model)
reviewer-effort: unrecorded
---

# CM1.3: the workbench's application icons and launch buttons, read cold in both schemes

## What was reviewed

The workbench window's two goldens, `workbench/testdata/golden/light-window.png`
and `dark-window.png`, recorded at workbench a48a3e6: the hero over the
grid of nine application groups, none running. The reviewer was briefed
with the Icon, Fill, Accent and Theme colour entries of DOMAIN.md and
with the full text of reviews/misreads.md, and asked to sample the
colour each icon is drawn in, the launch button's fill and title, and
anything else on the window against the three entries.

## The reviewer's reply, verbatim

The change passes. I sampled the pixels of both goldens with a small Go program run with GOWORK=off; the program is in `/private/tmp/claude-501/-Users-rene-code-w-vibrantgio/3baad769-88d1-4659-9ca5-03f56de1f7bb/scratchpad/px/main.go`.

**1. Application icons**
- **Light:** all nine icons are solid #272727 with antialiased edges. The app name beside each one is the same #272727. That is `Label` (black at alpha 0xd8) laid over the #ffffff window plane, which gives 255×39/255 = 39 = #27. A few pixels read #282828, which is rasteriser rounding and not a different colour.
- **Dark:** all nine icons are solid #dddddd, and so are the app names. That is `Label` (white at alpha 0xd8) laid over #1e1e1e, which gives 216 + 30×39/255 ≈ 221 = #dd.
- **Text colour:** every icon is in the text colour, as the Icon entry asks: "On the content, an application's icon … [is] in the text colour." In `view.go`, `appIcon` uses `textColour`, the same function as the name.
- **Accent:** no icon is drawn in the accent. Outside the nine launch buttons, neither render has a single chromatic pixel (max−min channel above 12). There are 0 in light and 0 in dark.

**2. Launch button**
- **Both schemes:** the fill is a flat #157efb (2314 pixels per button) and the title is #ffffff.
- **Tokens:** these are `DefaultButtonFill` (#157efb, the value measured in both appearances) and `AlternateSelectedControlText` (#ffffff).
- **Code path:** `buttonColors` takes its default (Filled) branch with no colour override, so it uses `p.DefaultButtonFill` and `p.AlternateSelectedControlText`. It does not use `ControlAccent` (#007aff).
- **Theme colour:** this matches the Fill entry's row "the theme colour | the Filled button — the one action a surface is about". The token follows the user's accent through `WithAccent`. Seeing the same blue in both renders is the frozen-emission case listed in misreads, not a hardcoded accent.

**3. Anything else**
- **No violations found.** The tiles and page are the plain window plane (#ffffff light, #1e1e1e dark). The name and blurb use neutral label colours. Nothing on the window is drawn in the accent apart from the launch button's fill.
- **Accent on the status line (not visible in these renders):** the status line draws "running" in `ControlAccent`. These renders show no running app, so I could not see it. It is tinted text, not a selection fill with white text, so it does not obviously fit "Accent: how a persistent state is shown: the platform's selection colour … as the fill, with white text". That is a possible point for the Accent entry to look at later, not something visible in this change.
- **Taste:** nine Filled buttons share one page. Each one is the single action of its own group, so the rule "the one action a surface is about" holds per group. Whether a grid of nine blue buttons reads too strongly is a matter of taste, not a violation.

Verdict: pass. Icons are in the text colour (#272727 / #dddddd) and the launch button is #157efb with a white title in both schemes, so the change meets the Icon, Fill and Accent entries.

## Disposition

- Icons and launch button: pass, as the task states.
- The status line's "running" in the accent: filed as finding 1207.
- Nine Filled buttons on one page: taste, as the reviewer says; each is
  the one action of its own group. Not filed.

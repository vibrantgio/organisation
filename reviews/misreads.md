# Recorded misreads

This file lists the non-defects that fresh-eyes reviewers of Vibrant Gio renders file again and again, and every review packet briefs its reviewer with it.

## A headless render lacks the parts a real window draws

The reviewer sees no window chrome, no traffic lights, no vibrancy, no focus ring and no scrollbars at rest.
The frames are 1x goldens.
A headless harness draws none of these parts, so their absence is not a defect.
Rebut by naming the harness and its stated blind spots.

Caveat: this briefing hides titlebar-integration defects from every review.
A task whose ruling involves the native titlebar strip is verified at the window-configuration level, never by renders.
That covers full-size content, the window buttons and the claim on the strip.

Established 2026-08-21.

## A headless harness draws stand-ins for the window controls

The reviewer sees flat window buttons with no rim, square window corners, or plain discs in demo hex colours.
The reviewer also notes that the discs ignore the scrim.
The harness paints these stand-ins at the right geometry for testing.
The real AppKit buttons, rounded corners, shadow and vibrancy appear only in a live window.
Rebut by naming the harness and its stated blind spots.

Established 2026-08-27 and 2026-08-30.

## A headless render shows platform substitutions

The reviewer sees Roboto instead of SF, Material icons instead of SF Symbols, sentence-case headings, and raised surfaces without shadows.
The harness substitutes these for the platform's own assets.
They are not defects of the component under review.
Rebut by naming the substitution.

## A frozen-scheme render looks like a hardcoded accent

The reviewer sees one accent in every frame and concludes that the app ignores the OS accent setting.
A headless harness freezes one theme emission, so every frame shows the same accent.
The apps take the OS accent from the live theme stream.
Rebut by pointing at the theme wiring, not at the pixels.

Established 2026-08-27.

## Small dark foreground on a light fill looks grey at 1x

The reviewer calls 13sp labels near-illegible grey.
Stems of about 1px lose more to antialiasing dark on light than light on dark.
The pairings measure 11:1 to 15:1.
Three independent reviewers made this call on the gallery colour swatches, and the pixels measured fine each time.
Rebut by measurement.

Established 2026-08-19.

## A hex comparison of two dark greys runs the wrong way

The reviewer reads #1c1c1c as lighter than #1e1e1e and calls the dark sidebar's direction backwards.
#1c1c1c is darker: its channels are 28 against 30.
Rebut by comparing the channel values.

Established 2026-10-21.

## Dark chrome darker than the content looks like a bug

The reviewer sees a dark sidebar at #151515 beside content at #181818 and calls it the opposite of platform convention.
The chrome is the window's backdrop, the darkest region, in both schemes.
This is measured off the platform in Voice Memos, Settings and a reference chat app.
Rebut by citing the level order.
A window whose chrome is lighter than its content is the real bug.

Established 2026-08-30.

## An adjacent saturated band shifts the hue of a thin accent line

The reviewer reports one tab underline as red or pink while the others are purple.
A pixel check shows all underlines in the same accent purple, (114,58,212).
A purple banner directly under the tab strip induces the perceived shift.
Rebut with a pixel measurement of the underline colour.

Established 2026-08-23.

## A small glyph square looks more saturated than the wide button beside it

The reviewer claims the icon-only button keeps a saturated fill in both themes and outweighs the wide button next to it.
A pixel probe shows both fills identical, {114,58,212} in light and {208,196,255} in dark.
Both buttons draw their fills through the same `buttonColors`, so they cannot diverge.
Rebut with a pixel probe of both fills.

Established 2026-08-30.

## Reviewer hue impressions off 1x frames are unreliable

The reviewer reports hues that the pixels do not contain.
One reviewer reported a red ramp as teal to purple.
Geometry judged off the same frames is reliable.
Rebut any hue claim with pixel sampling.

Established 2026-08-27.

## The window buttons follow folklore geometry

The reviewer asserts a 12pt diameter and a 20pt pitch for the traffic lights.
The stored reference measures 14pt and 23pt on this OS.
`reference/macos` and ADR-019 call the 12/20 values folklore that matches neither measured band.
Rebut by citing ADR-019.

Established 2026-08-27.

## A live window capture shows black square corners

The reviewer sees square black window corners in a `screencapture -l` capture.
The capture fills the rounded-corner cutouts with opaque #000000.
In a live capture the traffic lights are the real AppKit buttons.
A claim that their colours or geometry are wrong argues against memory, not pixels.
Rebut by naming the capture tool and citing ADR-019's 14/23 measurement.

Established 2026-08-28.

## A card's advisory tag gets checked against the wrong referent

The reviewer audits the themer card's `faint` tag against how vivid the card's swatch strip looks.
The reviewer then calls the tag incoherent.
The tag describes the base's own foregrounds on the base's own fills, not the swatch strip.
Rebut by stating the referent.
The finding that the card does not show its referent is recorded as an owner call.

Established 2026-08-20.

## A pointer target cannot be judged from a still

The reviewer reports that the chip's dismiss mark has no target.
`registerDismissTarget` registers a 24dp per-axis hit rectangle after the body's.
Rebut by citing the registration.

Established at BN2.2 and recurred at BN3.2.

## A cap height measured off a capture runs about 1px high

The reviewer measures an 11px cap height by scanning thresholded, antialiased pixels.
The face's own metric, OS/2 sCapHeight, is 9.95px at LabelLarge.
A size claim within about 1px of a font metric is not a divergence.
Rebut by citing the font metric.

Established 2026-09-02.

## The themer's colour field renders as an empty box

The reviewer reports a large trailing gap or a missing field on the Theme colour row.
The render harness draws the published colour field, the hex TextField, as an empty box.
Every themer review brief includes this line.

Established 2026-09-17.

## An offscreen review frame shows pinned radii or the placeholder palette

The reviewer reports square corners or the wrong palette on an offscreen frame.
These come from a broken harness, not from the component.
An offscreen review frame renders the real `tokens.Radius`, never the goldens' pinned `RadiusScale{}`.
It applies the theme palette synchronously, because a clock-driven `SetColors` never lands headless.
Rebut by fixing the harness and rendering the frame again.

Established 2026-08-27.

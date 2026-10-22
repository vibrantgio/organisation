# Images in vaultview's notes — exploration

This document explores how vaultview draws a picture a note refers
to: a PNG, a JPEG, a GIF or an SVG in the vault. It is candidate
material. Nothing here is planned until a task in PLAN.md links an
entry for it. Read off the code on 2026-10-22.

## 1. What exists

- `markdown` parses `![alt](path)` as an [[DOMAIN#Image]] block when
  the image is a paragraph's only content. An image mixed into a
  sentence becomes its alt text at parse time.
- `markdown` fetches nothing. `markdown.ImageProvider` returns the
  decoded pixels for a destination, and `markdown.WidgetImageProvider`
  returns a live `layout.Widget` instead, for vector formats. With no
  provider, or when the provider returns an error, the block is drawn
  as its alt text.
- `markdown/svgimage` is a provider for SVG over an `fs.FS`, with a
  raster provider as its fallback.
- `markdown/obsidian` turns `![[target]]` into a hyperlink span with
  the scheme `wikiembed:`, not into an image block. Vaultview resolves
  `wikiembed:` the same way as `wiki:`, as a link to a note.
- Vaultview sets no image provider. A note with `![alt](photo.png)`
  shows the text "alt"; a note with `![[photo.png]]` shows a link
  that resolves to nothing.

## 2. What Obsidian does, which a vault is written for

- An embed `![[photo.png]]` finds the file by name anywhere in the
  vault, as a wikilink finds a note. A path `![[folder/photo.png]]`
  is read from the vault root.
- `![[photo.png|300]]` draws the picture 300 points wide;
  `![[photo.png|300x200]]` draws it in that box. `![alt](photo.png)`
  draws it at its own size, scaled down to the measure.
- A remote `![](https://…/photo.png)` is fetched and drawn.
- A missing file is drawn as the embed text in the link colour.
- Formats: png, jpg, jpeg, gif, bmp, svg, webp, avif.

## 3. The Language questions

- [[DOMAIN#Image]] says a picture is content, read and never
  operated. It does not say how wide a picture is drawn, whether it
  is drawn at its own size or the measure, or what a picture wider
  than the measure does. The [[DOMAIN#Measure]] entry governs text.
- Embed is not a Language word. A note that shows another file's
  content in place needs a name; the vault's attachment folder is
  Obsidian's word and clashes with [[DOMAIN#Attachment]].
- A picture that cannot be read: a failed read is its own case under
  [[DOMAIN#Empty state]] for a region. For one block inside a
  document, the entry says nothing. The alt text is the markdown
  answer; the application's own sentence is the Language's.
- A remote picture: fetching over the network from a note is a
  policy question for the owner (privacy, offline vaults).

## 4. What a provider for vaultview is

- The provider is built once per vault from the vault's `Index`: a
  destination is resolved as a wikilink target is (`resolveFile`
  finds a file by name or path), so `![[photo.png]]`, `![](photo.png)`
  and `![](attachments/photo.png)` all land on the file.
- `markdown/obsidian` emits an image block for `![[file.png]]` when
  the target's extension is a picture's, keeping the `wikiembed:`
  span for notes. The width and box after `|` are parsed into the
  block.
- Decoding is read off the file through `image/png`, `image/jpeg`,
  `image/gif` and `golang.org/x/image/webp`; SVG through
  `svgimage`. Decoding runs off the layout thread; the block keeps
  its place at the width it will have (from the size in the source,
  or the image's header) while the pixels arrive, so the note does
  not jump.
- Decoded pixels are cached by path and modification time, bounded
  by bytes, and dropped when the vault is switched.
- A picture is drawn at its own pixel size divided by the display's
  density (a 600 px image is 300 dp at 2x) and scaled down to the
  measure's width, keeping its aspect. `@2x` in the name halves it.
- A picture that cannot be read is drawn as the application's own
  sentence in the block's place (the file name and that it could not
  be read), not as the alt text, and the cause is logged.

## 5. What it costs

- `markdown`: the width and box on an image block; an image mixed
  into a sentence stays alt text.
- `markdown/obsidian`: embed to image block for picture extensions.
- `markdown/svgimage` or a new `markdown/rasterimage`: the raster
  provider over an `fs.FS`, decode, cache, density.
- Vaultview: the provider over the vault's index; a model field for
  pictures arriving; goldens with a stored test picture in both
  schemes; a test that a missing picture draws the sentence.
- The reference: how Notes or Obsidian on the platform place a
  picture in running text (its margin above and below, its rounding
  if any) is not measured. One capture reads it.

## 6. Order, one task per run

1. Language: Image entry extended with width, the measure and the
   failed read; a word for an embed; the remote policy ruled.
2. `markdown`: image block width and box; `obsidian` embeds to image
   blocks.
3. The raster provider package with decode, cache and density.
4. Vaultview's provider over the index, pictures arriving off the
   layout thread, the failed-read sentence.
5. Reference capture of a picture in platform running text, then the
   margins drawn from it.

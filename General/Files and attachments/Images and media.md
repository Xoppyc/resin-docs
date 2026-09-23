# Images and Media

Resin supports image embedding, clipboard pasting, live resize handles, and a dedicated lightbox viewer for inspecting images in detail.

---

## Pasting and Importing Images

- **Pasting screenshots**: Capture a screenshot with `Win+Shift+S` (Windows) or `Cmd+Shift+4` (macOS) and press `Ctrl+V` inside any note. Resin saves the file into your `attachments/` folder as `attachments/Pasted image YYYYMMDDHHmmss.png` and inserts the `![[...]]` embed tag at your cursor position.
- **Drag-and-drop**: Drag image files directly from your system file manager into the editor. Resin copies the file into `attachments/` and inserts the embed link at the drop target.

---

## Embed Syntax

Resin supports both internal wikilink embeds and standard markdown image syntax:

- `![[diagram.png]]`: Embeds the image at its natural size.
- `![[diagram.png|400]]`: Scales the image width to 400 pixels while maintaining its aspect ratio.
- `![[diagram.png|400|<]]`: Sets the width to 400 pixels and aligns the image to the left.
- `![[diagram.png|400|>]]`: Sets the width to 400 pixels and aligns the image to the right.
- `![Alt Text](attachments/diagram.png)`: Standard markdown format.

Alignment shorthand symbols:

- `<` aligns left
- `>` aligns right
- `=` aligns center

---

## Interactive Editor Controls

When hovering over an image in Live Preview:

- **Resize handle**: Drag the right edge handle to resize the image visually. Resin automatically writes the updated width back into the markdown source.
- **Floating toolbar**: Displays buttons to open the lightbox viewer, copy the image to clipboard, or edit the embed syntax.

---

## Media Lightbox Viewer

Clicking an image or selecting the zoom icon opens the lightbox viewer:

- Use the mouse wheel to zoom in and out.
- Click and drag to pan across high-resolution images.
- Press `Esc` or click outside the image frame to exit the viewer.

---

## Supported Formats

Resin renders standard image formats including PNG, JPEG, WebP, GIF, and SVG.

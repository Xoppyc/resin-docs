# Images and Media

Resin supports image embedding, clipboard pasting, interactive resize handles, and a dedicated Media Viewer workspace view.

![[pasting-image-into-note.png|Pasting an image directly into the editor]]

## Pasting and Importing Images

- **Pasting screenshots**: Capture a screenshot with `Win+Shift+S` (Windows) or `Cmd+Shift+4` (macOS) and press `Ctrl+V` inside any note. Resin saves the file into your `attachments/` folder as `attachments/Pasted image YYYYMMDDHHmmss.png` and inserts the `![[...]]` embed at your cursor.
- **Drag and drop**: Drag image files from your desktop or file manager into the editor. Resin copies the file into `attachments/` and inserts the embed link at the drop target.

## Embed Syntax

Resin supports both internal wikilinks and standard Markdown image syntax:

- `![[diagram.png]]`: Embeds the image at its natural size.
- `![[diagram.png|400]]`: Scales width to 400 pixels while preserving aspect ratio.
- `![[diagram.png|400|<]]`: Scales to 400 pixels and aligns left.
- `![[diagram.png|400|>]]`: Scales to 400 pixels and aligns right.
- `![Alt Text](attachments/diagram.png)`: Standard Markdown format.

Alignment symbols:
- `<` aligns left
- `>` aligns right
- `=` aligns center

## Interactive Controls

When hovering over an image in Live Preview:

![[image-resize-handle.png|Interactive resize handle and alignment toolbar]]

- **Resize handle**: Drag the right edge handle to resize the image visually. Resin writes the updated width back into the Markdown source automatically.
- **Floating toolbar**: Quick buttons to open the Media Viewer, copy the image to your clipboard, change alignment, or edit the embed syntax.

## Media Viewer

Double-clicking an image or clicking the zoom button in the hover toolbar opens the image in Resin's dedicated **Media Viewer** workspace tab:

![[media-viewer-tab.png|Media Viewer tab with pan, zoom, rotation, and vault filmstrip]]

- **Pan and Zoom**: Zoom in/out with the scroll wheel or toolbar buttons, and click-drag to pan across high-resolution images.
- **Transform**: Rotate 90 degrees or flip horizontally and vertically.
- **File details**: Displays image resolution and file size.
- **Vault filmstrip**: Browse and cycle through every image in your vault using the bottom thumbnail strip or arrow keys.

## Supported Formats

Resin renders standard web image formats: PNG, JPEG, WebP, GIF, and SVG.



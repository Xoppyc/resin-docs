# Inspecting with Developer Tools

Because Resin is built with web technologies inside Tauri, you have access to full Chromium Developer Tools for inspecting HTML elements, debugging CSS styles, and examining console messages.

---

## Opening DevTools

Use any of the following shortcuts anywhere in Resin:

- `Ctrl+Shift+I` (Windows and Linux)
- `Cmd+Option+I` (macOS)
- `F12`

You can also rebind this shortcut in **Settings → Hotkeys** under the command `app:toggle-devtools`.

---

## Finding Element Classes for CSS Snippets

1. Open DevTools (`Ctrl+Shift+I`).
2. Click the element inspect cursor in the top-left corner of the DevTools panel (or press `Ctrl+Shift+C`).
3. Click any element in Resin—a tab, ribbon button, callout, or sidebar row.
4. The **Elements** panel highlights the corresponding HTML node and reveals its class names and attributes.

---

## Key Editor Classes (CodeMirror 6)

When customizing the editor with CSS snippets, target these common classes:

- `.cm-editor`: The outer editor container.
- `.cm-scroller`: The scrolling viewport of the document.
- `.cm-content`: The main text content area where lines are rendered.
- `.cm-line`: Individual text lines inside the editor.
- `.cm-activeLine`: The line currently holding your text cursor.
- `.cm-cursor`: The blinking cursor element.
- `.cm-selectionBackground`: The highlight behind selected text.

---

## Workspace Component Classes

- `.tab-bar`: The container holding tab headers in each split pane.
- `.tab`: An individual tab header (`.tab.mod-active` indicates the selected tab).
- `.ribbon`: The vertical toolbar on the left edge.
- `.status-bar`: The bottom contextual information bar.
- `.modal` and `.prompt`: Dialog overlays, confirmation modals, and the command palette.

---

## Testing CSS Live

Select any element in the DOM tree and edit properties directly in the **Styles** tab on the right side of DevTools. Changes apply instantly on screen. Once you have the design looking right, copy the rules into a `.css` file inside `.resin/snippets/` to make them permanent.

---

## Debugging Plugins in Console

The **Console** tab displays runtime log output, errors, and warnings. If you are developing a plugin, statements written with `console.log()` appear here in real time.

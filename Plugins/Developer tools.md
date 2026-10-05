# Built-in Developer Tools

Because Resin is built on modern web standards wrapped in Tauri, you have access to full desktop web inspection tools.

---

## Opening DevTools

Press either of the following shortcuts anywhere in Resin:

- **`Ctrl+Shift+I`** (Windows / Linux)
- **`Cmd+Option+I`** (macOS)
- **`F12`**

You can also rebind this shortcut in **Settings → Hotkeys** under the command `app:toggle-devtools`.

---

## What You Can Do with DevTools

### 1. Find Element Class Names for CSS Snippets
Click the inspect cursor icon in the top-left corner of the DevTools panel, then click on any element in Resin (a tab, ribbon icon, callout, or sidebar item). The Elements panel immediately highlights the HTML element and shows you its exact class names.

### 2. Live Test CSS Properties
Select an element in the DOM tree, go to the **Styles** tab on the right, and add or change CSS rules directly. You'll see the UI change in real time. Once you find a look you love, copy the CSS and paste it into your `.resin/snippets/my-tweak.css` file.

### 3. Inspect CSS Variables
Inspect the `<html>` or `<body>` tag to see all active CSS custom properties (`--background-primary`, `--accent-color`, `--text-normal`). This makes it easy to see which variables your theme or snippet can override.

### 4. Debug Plugins via Console
Check the **Console** tab for logs, warnings, or errors when developing custom plugins or testing code block renderers.


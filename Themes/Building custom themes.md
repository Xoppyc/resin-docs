# Building Custom Themes

A theme in Resin is a folder containing two files: a `manifest.json` describing your theme and a `theme.css` with your styles.

---

## Where Themes Live

Every theme lives inside your vault's hidden `.resin` folder:

```
<Your Vault>/
  └── .resin/
      └── themes/
          └── my-cool-theme/
              ├── manifest.json
              └── theme.css
```

---

## The `manifest.json` File

I noticed almost nobody reads changelogs (I get it, release notes get long!), so people didn't know what shape `manifest.json` needs to have.

Here is the exact schema Resin expects:

```json
{
  "id": "my-cool-theme",
  "name": "My Cool Theme",
  "version": "1.0.0",
  "author": "Your Name",
  "description": "A clean dark aesthetic for Resin",
  "minAppVersion": "0.2.5"
}
```

### Fields Explained:

- **`id`**: Unique kebab-case identifier (must match your folder name). Don't use spaces or special characters here.
- **`name`**: The human-readable name that shows up in Resin's Settings menu.
- **`version`**: Semantic version string (e.g. `1.0.0`).
- **`author`**: Your name or GitHub handle so people know who made it.
- **`description`**: A quick one-sentence summary of the look and feel.
- **`minAppVersion`**: The earliest version of Resin your theme supports (usually `0.2.5` or `0.3.0`).

---

## The `theme.css` File

Your `theme.css` is where the magic happens.

The easiest and cleanest way to write a theme is to override Resin's design tokens in `:root` (or `.theme-dark` / `.theme-light`). That restyles the sidebar, editor, buttons, dialogs, and tabs all at once without having to write hundreds of individual CSS rules.

### Starter Template

Here is a minimal, fully working theme template you can copy and tweak:

```css
/* ==========================================================================
   My Cool Theme for Resin
   ========================================================================== */

.theme-dark {
  /* Core Backgrounds */
  --background-primary: #1e1e2e;
  --background-secondary: #181825;
  --background-secondary-alt: #11111b;
  --sidebar-bg: #181825;

  /* Interactive States */
  --background-modifier-hover: rgba(255, 255, 255, 0.06);
  --background-modifier-active-hover: rgba(255, 255, 255, 0.1);
  --background-modifier-border: #313244;
  --border-subtle: #313244;
  --border-strong: #45475a;

  /* Typography Colors */
  --text-normal: #cdd6f4;
  --text-muted: #a6adc8;
  --text-faint: #6c7086;

  /* Accent Color (Buttons, Active Tabs, Wikilinks) */
  --accent-color: #cba6f7;
  --accent-color-hover: #b4befe;
  --accent-color-subtle: rgba(203, 166, 247, 0.15);

  /* Code & Syntax */
  --code-background: #11111b;
  --color-mono: #f5e0dc;

  /* Status Colors */
  --color-green: #a6e3a1;
  --color-red: #f38ba8;
  --color-orange: #fab387;
  --color-yellow: #f9e2af;
  --color-blue: #89b4fa;
}

.theme-light {
  --background-primary: #eff1f5;
  --background-secondary: #e6e9ef;
  --background-secondary-alt: #dce0e8;
  --sidebar-bg: #e6e9ef;
  --background-modifier-border: #ccd0da;
  --border-subtle: #ccd0da;
  --text-normal: #4c4f69;
  --text-muted: #6c6f85;
  --text-faint: #9ca0b0;
  --accent-color: #8839ef;
  --accent-color-hover: #7287fd;
}
```

---

## Going Crazy: Targeting Components Directly

Overriding variables gets you 80% of the way there. But if you want to completely reshape the layout—giving tabs pill shapes, changing how the status bar looks, or customizing callout borders—you can target Resin's class names directly.

### Main Elements to Target:

- **Tabs**: `.tab`, `.tab-bar`, `.tab.mod-active`, `.tab-close-button`
- **Navigation Ribbon**: `.ribbon`, `.ribbon-group`, `.ribbon-action`
- **File Explorer**: `.file-explorer`, `.tree-item`, `.tree-item-self`, `.folder-caret`
- **Note Editor**: `.note-editor`, `.note-editor-title-input`, `.cm-scroller`, `.cm-content`
- **Status Bar**: `.status-bar`, `.status-bar-item`
- **Callouts**: `.cm-callout`, `.cm-callout-title`, `.cm-callout-icon`
- **Context Menus**: `.menu-item`, `.menu-separator`, `.menu-content`

---

## Complete CSS Variables Reference

Instead of hunting through source code, use our complete reference tables for every variable in Resin:

- **[[Foundations|Foundations Reference]]**: Surfaces, borders, text, accents, status colors, radiuses, and shadows.
- **[[Components|Components Reference]]**: Tabs, ribbon, buttons, modals, status bar, and context menus.
- **[[Markdown and editor|Markdown & Editor Reference]]**: Headings, code blocks, callouts, tables, wikilinks, and checkboxes.

---

## How to Test and Apply Your Theme

1. Put your folder in `<Vault>/.resin/themes/<theme-id>/`.
2. Open Resin **Settings** (`Ctrl+,`) $\rightarrow$ **Appearance**.
3. Under the **Themes** dropdown, pick your theme. Resin reloads the active stylesheet on the fly.
4. Press `Ctrl+Shift+I` to open DevTools while you edit your `theme.css`. You can change properties live in the browser tools, see how they look, and then paste them into your file.

I hope Resin fits your workflow, but if you want to make it look like a cyberpunk terminal or a cozy paper notebook, you have all the tools to do it.

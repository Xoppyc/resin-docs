# Building Custom Themes

A theme in Resin is a folder containing two files: a `manifest.json` describing your theme and a `theme.css` with your styles.

![[theme-settings-picker.png|Selecting a custom theme from Appearance Settings]]

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

## The `manifest.json` File

Here is the exact schema Resin expects:

```json
{
  "id": "my-cool-theme",
  "name": "My Cool Theme",
  "version": "1.0.0",
  "author": "Your Name",
  "description": "A clean dark aesthetic for Resin",
  "minAppVersion": "0.4.0"
}
```

### Fields Explained:

- **`id`**: Unique kebab-case identifier (must match your folder name).
- **`name`**: Display name shown in Settings.
- **`version`**: Semantic version string (e.g. `1.0.0`).
- **`author`**: Your name or handle.
- **`description`**: One-sentence summary.
- **`minAppVersion`**: The earliest version of Resin supported.

## The `theme.css` File

The cleanest way to write a theme is to override Resin's design tokens in `.theme-dark` and `.theme-light`. This restyles the sidebar, editor, buttons, dialogs, and tabs simultaneously.

![[theme-devtools-inspection.png|Inspecting theme CSS variables with DevTools]]

### Starter Template

```css
.theme-dark {
  /* Surfaces */
  --background-primary: #1e1e2e;
  --background-secondary: #181825;
  --background-secondary-alt: #11111b;
  --sidebar-bg: #181825;

  /* Borders & Highlights */
  --background-modifier-hover: rgba(255, 255, 255, 0.06);
  --background-modifier-border: #313244;
  --border-subtle: #313244;
  --border-strong: #45475a;

  /* Typography */
  --text-normal: #cdd6f4;
  --text-muted: #a6adc8;
  --text-faint: #6c7086;

  /* Accent */
  --accent-color: #cba6f7;
  --accent-color-hover: #b4befe;
  --accent-color-subtle: rgba(203, 166, 247, 0.15);
}

.theme-light {
  --background-primary: #eff1f5;
  --background-secondary: #e6e9ef;
  --background-secondary-alt: #dce0e8;
  --sidebar-bg: #e6e9ef;
  --border-subtle: #ccd0da;
  --text-normal: #4c4f69;
  --text-muted: #6c6f85;
  --accent-color: #8839ef;
  --accent-color-hover: #7287fd;
}
```

## Targeting Components

You can also target component classes directly:
- **Tabs**: `.tab`, `.tab-bar`, `.tab.mod-active`, `.tab-close-button`
- **Ribbon**: `.ribbon`, `.ribbon-action`
- **File Explorer**: `.file-explorer`, `.tree-item`, `.folder-caret`
- **Editor**: `.note-editor`, `.cm-scroller`, `.cm-content`
- **Status Bar**: `.status-bar`, `.status-bar-item`
- **Callouts**: `.cm-callout`, `.cm-callout-title`

## Variable References

- **[[Foundations]]**: Surfaces, borders, text, accents, and shadows.
- **[[Components]]**: Tabs, ribbon, buttons, modals, and status bar.
- **[[Markdown and editor]]**: Headings, code blocks, callouts, tables, and links.


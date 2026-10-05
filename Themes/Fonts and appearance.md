# Typography and Appearance

Resin gives you full control over system typography and editor layout, including a native specimen font picker, base font sizes, line heights, and theme modes.

---

## The Three Font Domains

Resin separates typography into three distinct CSS variable domains so UI elements, editor text, and technical blocks stay visually distinct:

1. **Interface Font (`--font-ui`)**: Applied across the workspace chrome, including the ribbon, tabs, file explorer tree, status bar, and dialog modals.
2. **Text Font (`--font-editor`)**: Applied to the body text of your notes in both Live Preview and Reading views.
3. **Monospace Font (`--font-mono`)**: Applied to fenced code blocks, inline code spans, LaTeX math syntax, and tables.

---

## Choosing Fonts with the Specimen Picker

1. Open **Settings** (`Ctrl+,` or `Cmd+,`).
2. Navigate to the **Appearance** tab.
3. In the **Typography** section, click the font card next to Interface, Text, or Monospace font.
4. Resin scans your operating system and displays a live specimen card for each locally installed font.
5. Click any font to apply it immediately across your workspace.

To revert to Resin's default system font stack, click the **Reset** button next to any font field.

---

## Font Size and Line Height

Under **Settings → Appearance**:

- **Font Size**: Adjusts base editor text size (default: 16px). Headings and block elements scale proportionally based on this value.
- **Line Height**: Controls the vertical distance between lines of body text (default: 1.5). Increasing line height creates more breathing room for long-form reading.

---

## Base Themes and Accent Color

Resin supports both Light and Dark modes:

- Toggle between Light and Dark mode under **Settings → Appearance → Base Theme**, or select **Sync with System** to match your OS dark mode preference.
- Choose a custom **Accent Color** using the color swatches or hex input. The accent color controls link colors, active tab indicators, caret colors, and primary action buttons.

---

## Custom Font Fallbacks via CSS Snippets

If you want to define specific fallback font families or font feature settings (like OpenType ligatures), create a snippet in `.resin/snippets/fonts.css`:

```css
:root {
  --font-editor:
    "Inter", -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
  --font-mono: "JetBrains Mono", "Cascadia Code", "Fira Code", monospace;
}

/* Enable code ligatures */
.cm-content,
code,
pre {
  font-feature-settings:
    "liga" 1,
    "calt" 1;
}
```

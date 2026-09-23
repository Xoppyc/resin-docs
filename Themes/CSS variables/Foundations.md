# CSS Variables: Foundations

This reference lists all foundational design tokens in Resin: surfaces, borders, text, accents, status colors, radiuses, and shadows.

---

## 1. Surfaces & Backgrounds

Resin uses a 4-layer surface architecture. Overriding these gives your theme consistent depth across the entire window:

| Variable | Default (Dark) | What It Controls |
| :--- | :--- | :--- |
| `--background-primary` | `#1e1e2e` / `var(--base-00)` | Main editor surface and note body background |
| `--background-primary-alt` | `var(--base-05)` | Alternate workspace background, cards, and panels |
| `--background-secondary` | `var(--base-05)` | Shell surfaces, sidebar, and tab bar |
| `--background-secondary-alt` | `var(--base-10)` | Recessed UI elements and inactive panels |
| `--background-surface` | `var(--base-10)` | Dropdown menus and popover cards |
| `--background-surface-elevated` | `var(--base-20)` | Modals, dialogs, and floating pickers |
| `--sidebar-bg` | `var(--background-primary)` | File Explorer and left/right dock panels |
| `--tab-bar-bg` | `var(--background-secondary)` | Top horizontal workspace tab strip |

---

## 2. Interactive State Modifiers

These handle hovers, active clicks, and selections without needing hardcoded colors:

| Variable | Description |
| :--- | :--- |
| `--background-modifier-hover` | Subtle highlight when hovering items, buttons, or list rows |
| `--background-modifier-active` | Pressed / active click highlight |
| `--background-modifier-selected` | Selected tree items in File Explorer or active menu rows |
| `--background-modifier-focus` | Subtle glow when an input or pane has keyboard focus |
| `--background-modifier-border` | Subtle divider lines between panes and headers |

---

## 3. Borders

| Variable | Default | What It Controls |
| :--- | :--- | :--- |
| `--border-width` | `1px` | Standard border thickness across the app |
| `--border-subtle` | `var(--base-20)` | Subtle dividing lines between sidebar, tabs, and panels |
| `--border-strong` | `var(--base-30)` | High-contrast borders for modals and popovers |
| `--border-focus` | `var(--accent-color)` | Highlight border when an input or control is focused |
| `--border-error` | `var(--color-red)` | Validation errors on inputs or fields |

---

## 4. Typography & Text Colors

| Variable | Default (Dark) | What It Controls |
| :--- | :--- | :--- |
| `--text-normal` | `#cdd6f4` | Primary body text in notes, titles, and menus |
| `--text-muted` | `#a6adc8` | Secondary labels, timestamps, and muted headers |
| `--text-faint` | `#6c7086` | Placeholders, inactive icons, and line numbers |
| `--text-accent` | `var(--accent-color)` | Interactive link labels and accent highlights |
| `--text-accent-hover` | Lighter accent | Hover state on links and clickable labels |
| `--text-on-accent` | `#ffffff` / `#000000` | Text rendered on top of filled accent buttons |

---

## 5. The Accent Color

The accent color drives focus rings, active tab indicators, checkboxes, and buttons:

| Variable | Default | What It Controls |
| :--- | :--- | :--- |
| `--accent-color` | `#e47906` | Main brand accent (user-configurable in Settings) |
| `--accent-color-hover` | Lighter accent | Hover state for accent buttons and tabs |
| `--accent-color-subtle` | `rgba(accent, 0.15)` | Subtle accent backgrounds, text selections, and pills |

---

## 6. Semantic & Status Palette

Used by callouts, badges, status items, and tag pills:

| Variable | Hex Example | Common Usages |
| :--- | :--- | :--- |
| `--color-red` | `#f38ba8` | Errors, danger callouts, delete actions |
| `--color-orange` | `#fab387` | Warnings, attention callouts, high priority |
| `--color-yellow` | `#f9e2af` | Help, FAQ, caution |
| `--color-green` | `#a6e3a1` | Success, done tasks, check callouts |
| `--color-blue` | `#89b4fa` | Info, notes, external links |
| `--color-purple` | `#cba6f7` | Summaries, abstract callouts |
| `--color-pink` | `#f5c2e7` | Favorites, bookmarks, ideas |

---

## 7. Radiuses & Shadows

### Border Radiuses:
- `--radius-xs`: `2px` (tiny tags, badges)
- `--radius-sm`: `4px` (buttons, tree items, list rows)
- `--radius-md`: `6px` (tabs, inputs, callouts)
- `--radius-lg`: `8px` (cards, dialogs)
- `--radius-xl`: `12px` (large modals, startup screen)
- `--radius-round`: `9999px` (circular avatar pills, toggle switches)

### Elevation Shadows:
- `--shadow-sm`: Subtle card elevation
- `--shadow-md`: Context menus and dropdown popovers
- `--shadow-lg`: Floating dialogs and modals


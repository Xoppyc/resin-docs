# CSS Variables: Components

This reference lists the component-specific CSS variables available in Resin.

---

## 1. Workspace Tabs

Tabs live at the top of split panes in the workspace.

| Variable                  | Default                            | What It Controls                                |
| :------------------------ | :--------------------------------- | :---------------------------------------------- |
| `--tab-bar-bg`            | `var(--background-secondary)`      | Background of the tab bar container             |
| `--tab-bg-active`         | `var(--background-primary)`        | Background of the currently active/selected tab |
| `--tab-bg-inactive`       | `var(--background-secondary)`      | Background of inactive tabs                     |
| `--tab-bg-hover`          | `var(--background-modifier-hover)` | Hover highlight on inactive tabs                |
| `--tab-text-color`        | `var(--text-muted)`                | Text color on inactive tabs                     |
| `--tab-text-color-active` | `var(--text-normal)`               | Text color on the active tab                    |
| `--tab-radius`            | `var(--radius-md)`                 | Corner radius of tabs                           |
| `--tab-height`            | `36px`                             | Height of the tab strip                         |

---

## 2. Navigation Ribbon

The vertical ribbon on the left edge of the window.

| Variable               | Default                       | What It Controls                          |
| :--------------------- | :---------------------------- | :---------------------------------------- |
| `--ribbon-width`       | `44px`                        | Width of the vertical ribbon bar          |
| `--ribbon-background`  | `var(--background-secondary)` | Background color of the ribbon            |
| `--ribbon-icon-color`  | `var(--text-muted)`           | Color of ribbon action icons              |
| `--ribbon-icon-hover`  | `var(--text-normal)`          | Hover color of ribbon action icons        |
| `--ribbon-icon-active` | `var(--accent-color)`         | Color when a ribbon action is active/open |

---

## 3. Buttons (`.resin-button` / `<Button>`)

Buttons used across modals, toolbars, settings panels, and plugin views.

| Variable                    | Default                            | What It Controls           |
| :-------------------------- | :--------------------------------- | :------------------------- |
| `--button-background`       | `var(--background-secondary)`      | Default button background  |
| `--button-background-hover` | `var(--background-modifier-hover)` | Hover background color     |
| `--button-color`            | `var(--text-normal)`               | Button text and icon color |
| `--button-color-hover`      | `var(--text-normal)`               | Hover text color           |
| `--button-border`           | `1px solid var(--border-subtle)`   | Default button border      |
| `--button-radius`           | `var(--radius-md, 6px)`            | Corner radius of buttons   |
| `--button-padding`          | `6px 14px`                         | Padding inside buttons     |
| `--button-padding-sm`       | `4px 8px`                          | Small button padding       |
| `--button-padding-lg`       | `8px 18px`                         | Large button padding       |

### Universal Modifier Classes

Plugin developers and custom views can use these classes on any standard `<button>` tag:

- `.resin-button`: Base styled button with subtle background and border.
- `.resin-button.mod-cta` (or `button.mod-cta`): Primary solid accent button using `var(--accent-color)`.
- `.resin-button.mod-warning` (or `button.mod-warning`): Destructive button using error colors.
- `.resin-button.mod-ghost`: Transparent background button with hover state.
- `.mod-sm`: Compact padding (4px 8px) and 12px text.
- `.mod-lg`: Generous padding (8px 18px) and 14px text.

---

## 4. Modals & Dialogs

Floating popups, confirm dialogs, and settings overlays.

| Variable               | Default                              | What It Controls                   |
| :--------------------- | :----------------------------------- | :--------------------------------- |
| `--modal-bg`           | `var(--background-surface-elevated)` | Modal container background         |
| `--modal-border-color` | `var(--border-strong)`               | Border color around the modal card |
| `--modal-border-width` | `var(--border-width)`                | Border thickness                   |
| `--modal-radius`       | `var(--radius-xl)`                   | Corner radius of floating modals   |
| `--modal-padding`      | `var(--size-3-1)`                    | Internal padding around content    |
| `--modal-shadow`       | `var(--shadow-lg)`                   | Drop shadow for elevation          |

---

## 5. Status Bar

The information strip docked at the bottom of the window.

| Variable                    | Default                             | What It Controls                               |
| :-------------------------- | :---------------------------------- | :--------------------------------------------- |
| `--status-bar-bg`           | `var(--background-secondary)`       | Background color of the status bar             |
| `--status-bar-border-color` | `var(--background-modifier-border)` | Top border divider                             |
| `--status-bar-text-color`   | `var(--text-muted)`                 | Color of word count, line/col, and mode labels |
| `--status-bar-height`       | `28px`                              | Height of the status bar                       |
| `--status-bar-font-size`    | `11px`                              | Font size for status items                     |

---

## 6. Context Menus & Popovers

Right-click menus and dropdown selectors.

| Variable               | Default                            | What It Controls                     |
| :--------------------- | :--------------------------------- | :----------------------------------- |
| `--menu-bg`            | `var(--background-surface)`        | Dropdown popup container background  |
| `--menu-border-color`  | `var(--border-strong)`             | Border around dropdown menus         |
| `--menu-radius`        | `var(--radius-md)`                 | Corner radius of the menu container  |
| `--menu-item-hover`    | `var(--background-modifier-hover)` | Row background on hover              |
| `--menu-item-text`     | `var(--text-normal)`               | Text color for menu actions          |
| `--menu-item-disabled` | `var(--text-faint)`                | Text color for disabled menu actions |

---

## 7. Checkboxes (`.resin-checkbox` / `<Checkbox>`)

Custom multi-state checkbox widgets used in the editor, task sidebar, and settings dialogs.

| Variable                   | Default                    | What It Controls                        |
| :------------------------- | :------------------------- | :-------------------------------------- |
| `--task-checkbox-size`     | `16px`                     | Checkbox width and height               |
| `--task-checkbox-radius`   | `4px`                      | Corner radius of the checkbox box       |
| `--task-glyph-size`        | `12px`                     | Checkbox inner glyph dimensions         |
| `--task-glyph-todo`        | `none`                     | SVG glyph for todo state (`[ ]`)        |
| `--task-glyph-inprogress`  | Radial progress ring SVG   | SVG glyph for in-progress state (`[/]`) |
| `--task-glyph-done`        | Checkmark SVG              | SVG glyph for completed state (`[x]`)   |
| `--task-glyph-canceled`    | Horizontal dash SVG        | SVG glyph for canceled state (`[-]`)    |
| `--task-bg-todo`           | `transparent`              | Background color when uncompleted       |
| `--task-bg-inprogress`     | Accent with `0.12` opacity | Background color when in progress       |
| `--task-bg-done`           | `var(--accent-color)`      | Background color when done              |
| `--task-bg-canceled`       | `transparent`              | Background color when canceled          |
| `--task-border-todo`       | `var(--border-strong)`     | Border color for uncompleted checkbox   |
| `--task-border-inprogress` | `var(--accent-color)`      | Border color for in-progress checkbox   |
| `--task-border-done`       | `var(--accent-color)`      | Border color for completed checkbox     |
| `--task-border-canceled`   | `var(--text-muted)`        | Border color for canceled checkbox      |

Plugin developers can render `<Checkbox state={rawChar} onChange={...} />` in React or `<span class="resin-checkbox" data-task="x"></span>` in plain HTML.

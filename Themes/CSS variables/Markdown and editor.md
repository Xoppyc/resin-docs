# CSS Variables: Markdown & Editor

This reference lists the CSS tokens driving typography and rendered elements inside the note editor.

---

## 1. Markdown Headings

Headings support individual color tokens so theme authors can easily create rainbow or hierarchical palettes:

| Variable     | Default (Dark)       | What It Controls         |
| :----------- | :------------------- | :----------------------- |
| `--h1-color` | `var(--text-normal)` | `# Heading 1` color      |
| `--h2-color` | `var(--text-normal)` | `## Heading 2` color     |
| `--h3-color` | `var(--text-normal)` | `### Heading 3` color    |
| `--h4-color` | `var(--text-normal)` | `#### Heading 4` color   |
| `--h5-color` | `var(--text-muted)`  | `##### Heading 5` color  |
| `--h6-color` | `var(--text-muted)`  | `###### Heading 6` color |
| `--h1-size`  | `2.0em`              | Heading 1 font scale     |
| `--h2-size`  | `1.6em`              | Heading 2 font scale     |
| `--h3-size`  | `1.3em`              | Heading 3 font scale     |
| `--h4-size`  | `1.15em`             | Heading 4 font scale     |

---

## 2. Code & Fenced Code Blocks

Controls inline backticks (`` `code` ``) and fenced blocks (` ```js `):

| Variable            | Default              | What It Controls                              |
| :------------------ | :------------------- | :-------------------------------------------- |
| `--code-background` | `var(--base-10)`     | Background behind inline code and code blocks |
| `--code-color`      | `var(--text-normal)` | Text color inside code blocks                 |
| `--code-radius`     | `var(--radius-sm)`   | Corner radius of code fences and inline pills |
| `--font-mono`       | `monospace`          | Font family used for all code rendering       |

---

## 3. Callouts

Callout blocks (`> [!type]`) use dynamic variables based on their type:

| Variable                 | Default                            | What It Controls                         |
| :----------------------- | :--------------------------------- | :--------------------------------------- |
| `--callout-color`        | Dynamic by type                    | Primary theme color for border and title |
| `--callout-background`   | `rgba(var(--callout-color), 0.08)` | Semi-transparent background fill         |
| `--callout-border-width` | `3px`                              | Thickness of the left accent bar         |
| `--callout-radius`       | `var(--radius-md)`                 | Corner radius of the callout container   |
| `--callout-padding`      | `12px 16px`                        | Inner padding around callout text        |

---

## 4. Tables

GFM markdown tables rendered in the editor:

| Variable               | Default                             | What It Controls                        |
| :--------------------- | :---------------------------------- | :-------------------------------------- |
| `--table-border-color` | `var(--background-modifier-border)` | Grid cell border divider lines          |
| `--table-header-bg`    | `var(--background-secondary)`       | Background of table header (`<th>`) row |
| `--table-row-hover-bg` | `var(--background-modifier-hover)`  | Subtle row highlight on hover           |
| `--table-cell-padding` | `6px 12px`                          | Spacing inside table cells              |

---

## 5. Wikilinks & Embeds

Interactive link pills (`[[Note]]`) and media embeds (`![[image.png]]`):

| Variable                  | Default               | What It Controls                                        |
| :------------------------ | :-------------------- | :------------------------------------------------------ |
| `--link-color`            | `var(--accent-color)` | Text and underline color of resolved wikilinks          |
| `--link-color-hover`      | Lighter accent        | Hover highlight on wikilinks                            |
| `--link-unresolved-color` | `var(--text-faint)`   | Text color for links pointing to non-existent notes     |
| `--link-pill-bg`          | `rgba(accent, 0.1)`   | Subtle pill background behind wikilinks in Live Preview |
| `--link-pill-radius`      | `var(--radius-sm)`    | Corner radius of interactive wikilink chips             |

---

## 6. Tasks and Multi-State Checkboxes

Checklists (`- [ ]`, `- [/]`, `- [x]`, `- [-]`) with customizable SVG glyphs:

| Variable                     | Default                      | What It Controls                         |
| :--------------------------- | :--------------------------- | :--------------------------------------- |
| `--task-glyph-todo`          | `none`                       | SVG icon URL for empty `[ ]` tasks       |
| `--task-glyph-inprogress`    | SVG clock/arc                | SVG icon URL for in-progress `[/]` tasks |
| `--task-glyph-done`          | SVG checkmark                | SVG icon URL for completed `[x]` tasks   |
| `--task-glyph-canceled`      | SVG horizontal dash          | SVG icon URL for canceled `[-]` tasks    |
| `--task-bg-todo`             | `transparent`                | Background for uncompleted `[ ]` tasks   |
| `--task-bg-inprogress`       | `oklch(accent / 0.12)`       | Background for in-progress `[/]` tasks   |
| `--task-bg-done`             | `var(--accent-color)`        | Background for completed `[x]` tasks     |
| `--task-bg-canceled`         | `transparent`                | Background for canceled `[-]` tasks      |
| `--task-border-todo`         | `var(--border-strong, #666)` | Border color for uncompleted tasks       |
| `--task-border-inprogress`   | `var(--accent-color)`        | Border color for in-progress tasks       |
| `--task-border-done`         | `var(--accent-color)`        | Border color for completed tasks         |
| `--task-border-canceled`     | `var(--text-muted, #888)`    | Border color for canceled tasks          |
| `--task-checkbox-size`       | `16px`                       | Checkbox box dimensions                  |
| `--task-checkbox-radius`     | `4px`                        | Corner radius of checkbox box            |
| `--task-glyph-size`          | `12px`                       | Sizing of embedded SVG glyph             |
| `--task-checked-decoration`  | `line-through`               | Text decoration for completed lines      |
| `--task-checked-color`       | `var(--text-muted)`          | Text color for completed lines           |
| `--task-checked-opacity`     | `0.65`                       | Text opacity for completed lines         |
| `--task-canceled-decoration` | `line-through`               | Text decoration for canceled lines       |
| `--task-canceled-color`      | `var(--text-muted)`          | Text color for canceled lines            |
| `--task-canceled-opacity`    | `0.6`                        | Text opacity for canceled lines          |

# Tasks

The Tasks plugin aggregates task checkboxes across your entire vault into a dedicated sidebar panel and lets you embed live task queries directly in your notes.

![[tasks-sidebar-panel.png|Tasks sidebar panel with status filter pills and search]]

## Open the Tasks View

- Click the checklist icon in the left ribbon.
- Or open the Command Palette (`Ctrl+P`) and run **Tasks: Open tasks sidebar**.

## Filtering Tasks

- **Status pills**: Filter by **All**, **To Do** (`[ ]`), **In Progress** (`[/]`), or **Done** (`[x]`).
- **Filter toggle**: Toggle between **Tasks Only** (items with metadata or tags) and **All Items** (all vault checklists).
- **Search bar**: Type to filter tasks by title, note name, project, or tag.

## Embedded Task Queries

Embed live-updating task queries in any note using a ```` ```tasks ```` code block:

![[tasks-embedded-query-block.png|Rendered tasks codeblock query embedded in a note]]

### Query Syntax

| Parameter | Values | Example |
| :--- | :--- | :--- |
| `status` | `empty`, `pending`, `done`, `canceled` | `status: pending` |
| `only` | `tasks`, `all` | `only: tasks` |
| `project` | `<name>` | `project: Resin` |
| `tag` | `<#tag>` | `tag: #urgent` |
| `sort by` | `due`, `priority`, `created` | `sort by: priority` |

### Query Examples

````markdown
```tasks
status: pending
project: Resin
sort by: priority
```
````

Checkboxes in rendered queries are interactive. Clicking a checkbox updates its state and writes the modification directly to the source note on disk. Clicking the task text opens the source note and scrolls to the target line.


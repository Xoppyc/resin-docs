# Tasks

The Tasks plugin scans your entire vault for markdown task checkboxes and organizes them in a dedicated sidebar view and through interactive embedded note queries.

---

## Opening the Tasks View

- Click the checklist icon in the left ribbon.
- Or open the Command Palette (`Ctrl+P` / `Cmd+P`) and run **Tasks: Open tasks sidebar**.

The Tasks view opens in the sidebar, displaying a live count badge of all pending and completed tasks across your vault.

---

## Filtering and Searching

At the top of the Tasks view, you have quick status filters and a search bar:

- **Filter Toggle**: Switch between **Tasks Only** (items with metadata or tags) and **All Items** (including plain shopping lists).
- **Filter Pills**: Switch between **All**, **To Do** (`[ ]`), **In Progress** (`[/]`), and **Done** (`[x]`).
- **Live Search**: Type in the search box to filter tasks by title, note name, project, or tag.

---

## Embedded Task Queries (` ```tasks ` Codeblock)

You can embed a live-updating task board or query directly into any note using fenced ` ```tasks ` code blocks. Resin resolves these queries in 0 milliseconds directly from the in-memory `MetadataCache`.

### Query Syntax Reference

Each line inside the codeblock specifies a filter parameter in `key: value` format:

| Key                | Values                                 | Description                                                | Example             |
| :----------------- | :------------------------------------- | :--------------------------------------------------------- | :------------------ |
| `status`           | `empty`, `pending`, `done`, `canceled` | Filters tasks by their state                               | `status: pending`   |
| `only`             | `tasks`, `all`                         | Set to `tasks` to ignore plain checklists without metadata | `only: tasks`       |
| `project`          | `<name>`                               | Matches project name (case-insensitive substring)          | `project: Resin`    |
| `tag`              | `<#tag>`                               | Matches a specific `#tag`                                  | `tag: #urgent`      |
| `sort by` / `sort` | `due`, `priority`, `created`           | Sorts the resulting task list                              | `sort by: priority` |

### Concrete Examples

#### 1. In-Progress Project Tasks

````markdown
```tasks
status: pending
project: Resin
sort by: priority
```
````

#### 2. All Urgent Items Across Vault

````markdown
```tasks
status: empty
tag: #urgent
sort by: due
```
````

#### 3. Completed Archive for Review

````markdown
```tasks
status: done
sort by: created
```
````

Every checkbox inside a rendered query is fully interactive. Clicking a task in the query cycles its state and writes the modification back to the source note on disk in the background.

---

## Navigating to Notes

- Click on any task description or the file header in the sidebar or query block to open the note in your active workspace tab and jump straight to that task's line.

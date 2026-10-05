# Tasks and Checkboxes

In Resin, checklists support multi-state workflows and natural-language metadata that gets indexed across your vault.

![[task-checkbox-states.png|The four checkbox states in Live Preview]]

## Checkbox States

Standard markdown provides `- [ ]` and `- [x]`. Resin adds in-progress and canceled states:

| Syntax | State | Meaning |
| :--- | :--- | :--- |
| `- [ ]` | `empty` | To Do / Unstarted |
| `- [/]` | `pending` | In Progress |
| `- [x]` | `done` | Completed (struck through) |
| `- [-]` | `canceled` | Dropped / Canceled |

### Shortcuts and Interactions

- **Click**: Toggles between `- [ ]` and `- [x]`.
- **Mod+Enter** (`Ctrl+Enter` / `Cmd+Enter`): Cycles through all four states: `[ ]` -> `[/]` -> `[x]` -> `[-]` -> `[ ]`.
- **Enter**: Continues the task list on the next line. Pressing Enter on an empty checkbox converts the line back to plain text.

## Task Metadata

Adding metadata tokens turns any checklist line into a structured task indexed by `app.metadataCache`. In Live Preview, these tokens render as interactive chips:

![[task-metadata-chips.png|Rendered metadata chips and ghost autocomplete]]

| Field | Natural Syntax | Emoji Shorthand | Example |
| :--- | :--- | :--- | :--- |
| **Due date** | `due: <date>` | `📅 <date>` | `due tomorrow`, `📅 2026-10-15` |
| **Scheduled** | `scheduled: <date>` | `⏳ <date>` | `scheduled: Friday` |
| **Priority** | `priority: <p>` | `🔺`, `⏫`, `🔼`, `🔽`, `⏬` | `priority: high`, `p:1` |
| **Recurrence** | `every <interval>` | `🔁 <interval>` | `every weekday`, `repeat: daily` |
| **Estimate** | `estimate: <time>` | `⏱️ <time>` | `estimate: 45m`, `⏱️ 2h` |
| **Project** | `+Project` or `[[Project]]` | — | `+Resin`, `[[Launch]]` |
| **Tags** | `#tag` | — | `#release`, `#dev` |

## Ghost Suggestions

When typing inside a task line, Resin shows subtle ghost completions:
- Type `due ` -> suggests `tomorrow`
- Type `priority ` -> suggests `high`
- Type `every ` -> suggests `weekday`

Press `Tab` to accept the suggestion, or `Esc` to dismiss it.

## Querying Tasks

Embed live queries in any note using the ```` ```tasks ```` code block. Refer to [[Tasks]] for query syntax and filtering options.


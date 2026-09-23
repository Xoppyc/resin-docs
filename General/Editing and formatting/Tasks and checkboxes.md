# Tasks & Multi-State Checkboxes

In Resin, checklists are fast, clean, and support multi-state workflows. A checklist item is just a checkbox until it earns metadata. Once metadata is added, it is indexed across your entire vault.

---

## The Four Checkbox States

Standard markdown only supports `- [ ]` (empty) and `- [x]` (done). Resin expands this with in-progress and canceled states:

| Markdown Syntax | State      | Appearance           | Meaning                                        |
| :-------------- | :--------- | :------------------- | :--------------------------------------------- |
| `- [ ]`         | `empty`    | Clean rounded square | Unstarted / To Do                              |
| `- [/]`         | `pending`  | Radial progress ring | In Progress / Working on it                    |
| `- [x]`         | `done`     | Filled checkmark     | Completed (title is struck through and dimmed) |
| `- [-]`         | `canceled` | Horizontal dash      | Dropped / On Hold                              |

### Clicking & Keyboard Behavior

- **Single Click**: Toggles directly between uncompleted (`[ ]`) and completed (`[x]`).
- **Mod+Enter** (`Ctrl+Enter` on Windows/Linux, `Cmd+Enter` on macOS): Cycles through all four states: `[ ]` $\rightarrow$ `[/]` $\rightarrow$ `[x]` $\rightarrow$ `[-]` $\rightarrow$ `[ ]`.
- **Enter**: Continues the list on the next line.
- **Enter on empty checkbox**: Clears the checkbox and returns to plain paragraph text.

---

## Supported Task Metadata

When a checkbox contains any of the following tokens, Resin elevates it into a structured task indexed by `app.metadataCache` and SQLite.

In Live Preview, these tokens automatically fold into subtle, readable chips. Clicking a chip or placing your cursor inside it unfolds the raw text for editing.

| Metadata Type         | Natural Language Syntax                                          | Emoji Shorthand              | Examples                                                                                  |
| :-------------------- | :--------------------------------------------------------------- | :--------------------------- | :---------------------------------------------------------------------------------------- |
| **Due Date**          | `due: <date>` or `due <date>`                                    | `📅 <date>`                  | `due tomorrow`, `due Mon`, `due monday June 2nd`, `due in 3 days`, `📅 2026-09-30 at 5pm` |
| **Scheduled / Start** | `scheduled: <date>` or `start: <date>`                           | `⏳ <date>`                  | `scheduled: Friday`, `start: next week`, `⏳ 2026-10-01`                                  |
| **Priority**          | `priority: highest\|high\|medium\|low\|lowest` or `p:1` to `p:4` | `🔺`, `⏫`, `🔼`, `🔽`, `⏬` | `priority: high`, `p:1` (highest), `🔺 urgent review`                                     |
| **Recurrence**        | `repeat: <interval>` or `every <interval>`                       | `🔁 <interval>`              | `every weekday`, `repeat: daily`, `🔁 every Monday`                                       |
| **Time Estimate**     | `estimate: <duration>`                                           | `⏱️ <duration>`              | `estimate: 30m`, `estimate: 2h`, `⏱️ 45m`                                                 |
| **Project**           | `+ProjectName` or `[[ProjectName]]`                              | —                            | `+Resin`, `[[Client Work]]`                                                               |
| **Tags**              | `#tag`                                                           | —                            | `#feature`, `#errand`, `#bug`                                                             |
| **Creation Date**     | `created: <date>`                                                | `➕ <date>`                  | `➕ 2026-09-22` (stamped when created)                                                    |
| **Completion Date**   | `completed: <date>`                                              | `✅ <date>`                  | `✅ 2026-09-22 18:30` (stamped when checked off)                                          |

### Supported Date Expressions

Resin's date parser supports full natural language:

- **Relative words**: `today`, `tomorrow`, `yesterday`, `tonight`
- **Weekdays**: `Mon`, `monday`, `Tuesday`, `next Friday`, `this Wednesday`
- **Month and day**: `June 2nd`, `June 2`, `2nd June`, `Oct 15th, 2026`
- **Offsets**: `in 3 days`, `in 2 weeks`, `in 4 hours`
- **ISO & Standard**: `2026-09-22`, `09/22/2026`, `22.09.2026`
- **Optional time**: Append `at 5pm`, `at 17:30`, or `14:00` to any date.

---

## Ghost Auto-Suggestions

When typing a task line, Resin provides non-intrusive ghost auto-suggestions:

- Type `due ` $\rightarrow$ displays ghost suggestion `tomorrow`.
- Type `due m` $\rightarrow$ displays ghost suggestion `onday`.
- Type `priority ` $\rightarrow$ displays ghost suggestion `high`.
- Type `every ` $\rightarrow$ displays ghost suggestion `weekday`.
- Type `estimate ` $\rightarrow$ displays ghost suggestion `30m`.

Press **Tab** to accept the suggestion. Press **Escape** to dismiss it.

---

## Querying Tasks with Code Blocks

You can embed live task queries directly inside any note using the ` ```tasks ` codeblock. See [[Tasks]] for the complete query reference and examples.

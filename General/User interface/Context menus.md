# Context Menus

Resin provides context menus across the workspace. Right-clicking an element displays actions relevant to the current item, whether you are managing open tabs, organizing files in the sidebar, or editing notes.

---

## Tab Header Context Menu

Right-clicking any tab header opens options for split placement and tab lifecycle:

- **Split Right**: Opens the current note in a new vertical split pane to the right.
- **Split Down**: Opens the current note in a new horizontal split pane below.
- **Duplicate Tab**: Opens a second tab with the same file in the active pane.
- **Close**: Closes the selected tab (equivalent to `Ctrl+W`).
- **Close Others**: Closes every tab in the current split group except the selected one.
- **Close Tabs to the Right**: Closes all tabs located to the right of the selected tab.

---

## File Explorer Context Menu

Right-clicking a file or folder in the sidebar opens file system actions:

- **New note**: Creates a new markdown file inside the clicked folder.
- **New folder**: Creates a new directory inside the clicked folder.
- **Rename**: Highlights the name for inline editing (or press `F2`).
- **Delete**: Prompts to delete the file or directory from your vault (or press `Delete`).
- **Reveal in File Explorer**: Opens your operating system's native file explorer (Windows Explorer, Finder, or Linux file manager) at the location of the selected file.

---

## Editor Context Menu

Right-clicking inside the CodeMirror editor provides text and formatting actions:

- Standard clipboard operations: **Cut**, **Copy**, and **Paste**.
- Markdown formatting helpers for bold, italic, code, and links.
- Block helpers for inserting tables, callouts, and code fences.

---

## Plugin Extensions

Plugins can register their own context menu items using the context menu registry. Menu items are scoped to target components such as tab headers, tree items, or editor text selections.

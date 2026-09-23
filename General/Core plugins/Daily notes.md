# Daily Notes

The Daily Notes core plugin creates a date-stamped markdown file for today with a single keystroke. It helps you keep running work logs, journal entries, and task lists without manually organizing folders.

---

## Opening Today's Daily Note

To open or create today's daily note:

- Press `Alt+D` anywhere in Resin.
- Click the calendar icon in the left ribbon.
- Open the [[Command palette]] (`Ctrl+P`) and choose **Open today's daily note**.

If today's note does not exist yet, Resin creates it automatically and focuses the editor cursor on the first line.

---

## Configuring Daily Notes

Open **Settings** (`Ctrl+,`) and select **Daily Notes** in the sidebar.

- **Date format**: The naming format for your files (e.g. `yyyy-MM-dd` produces `2026-09-22.md`). Standard date tokens are supported.
- **New file location**: The destination folder where daily notes are stored (e.g. `Daily/` or `Journal/`). If the folder does not exist, Resin creates it automatically.
- **Template file**: An optional path to a note in your vault used as the starting structure for every new daily note.

---

## Using Daily Templates

If you have a recurring daily structure, you can point the plugin to a template note:

```markdown
# {{date:yyyy-MM-dd}}

## Priorities

- [ ]

## Notes & Discoveries

-

## Log

-
```

When you trigger today's daily note for the first time, Resin populates the new file with your template content.

---

## Linking Daily Notes

Daily notes integrate naturally with the rest of your vault through wikilinks:

```markdown
# 2026-09-22

- Reviewed [[Workspace and tabs]] layout behavior
- Updated documentation for the [[Command palette]]
```

Because Resin indexes wikilinks, opening any project note shows backlinks to every daily note that referenced it.

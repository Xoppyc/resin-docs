# Daily Notes

The Daily Notes core plugin creates or opens a date-stamped markdown file for today with a single keystroke.

![[daily-note-active.png|Today's daily note open in the workspace]]

## Open Today's Note

- Press `Alt+D` anywhere in Resin.
- Click the calendar icon in the left ribbon.
- Open the [[Command palette]] (`Ctrl+P`) and run **Open today's daily note**.

If today's note does not exist yet, Resin creates it and focuses the editor on the first line.

## Configuration

Configure options in **Settings** (`Ctrl+,`) -> **Daily Notes**:

![[daily-notes-settings.png|Daily notes date format and folder configuration in Settings]]

- **Date format**: Filename format pattern (default `yyyy-MM-dd` -> `2026-10-05.md`).
- **New file location**: Target vault folder (e.g. `Daily/` or `Journal/`).
- **Template file**: Optional path to a note used as the initial template for new daily notes.

## Templates

Create a template note in your vault to pre-populate headings and checklists:

```markdown
# {{date:yyyy-MM-dd}}

## Priorities
- [ ] 

## Notes & Log
- 
```

When today's note is created, Resin resolves the variables and inserts the template content.

## Linking Daily Notes

Daily notes connect with projects through wikilinks:

```markdown
- Met with team regarding [[Project Roadmap]]
- Fixed issue noted in [[Workspace and tabs]]
```

Because Resin indexes wikilinks, connections between daily logs and project notes are tracked across your vault and visualized in Graph View.


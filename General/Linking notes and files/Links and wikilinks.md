# Wikilinks & Navigation

Wikilinks connect your thoughts together, creating an interconnected knowledge network.

---

## 1. Creating Links

Type `[[` anywhere in a note. Resin instantly opens the suggestion popup:

- Start typing the title of another note.
- Press `Tab` or `Enter` to pick the completion. Resin inserts the closed link: `[[Project Roadmap]]`.
- If the note doesn't exist yet, complete the link anyway. It becomes an unresolved link (styled in `--link-unresolved-color`).

---

## 2. Linking to Specific Headings

You can link directly to a section inside a note by typing `#`:

- `[[Project Roadmap#Milestones]]`: Links directly to the "Milestones" heading inside `Project Roadmap`.
- `[[#Summary]]`: Links to a heading within the current note you are editing.

When you type `#`, Resin’s autocomplete automatically scans the target note and lists every heading from `H1` to `H6`.

---

## 3. Custom Display Text (Aliases)

Use the pipe character (`|`) to change how the link looks in your text:

- `[[Project Roadmap|our plan]]` renders as "our plan", but clicking it navigates to `Project Roadmap.md`.

---

## 4. Navigating Links

- **`Ctrl+Click`** (or `Cmd+Click` on macOS): Follows the link and opens the note in the active editor.
- **Unresolved Link Click**: Clicking an unresolved link automatically creates the new note on disk, adds it to the file tree, and opens it immediately for writing.
- **Backlinks**: When notes link to each other, Resin's in-memory `MetadataCache` tracks every reference, letting you see who links to what note across your vault.


# Links and Wikilinks

Wikilinks connect notes together, building an interconnected knowledge graph across your vault.

![[wikilink-autocomplete-popup.png|Wikilink autocomplete popup triggered by double brackets]]

## Create a Link

Type `[[` anywhere in a note to trigger the link suggestion popup:

- Type part of a note's title to filter the vault list.
- Press `Enter` or `Tab` to insert the completed link: `[[Project Roadmap]]`.
- If the note does not exist yet, type its name anyway. Resin formats it as an unresolved link (`--link-unresolved-color`).

## Link to a Heading

Target specific sections inside a note using the hash (`#`) symbol:

```markdown
[[Project Roadmap#Milestones]]
[[#Local Section]]
```

![[wikilink-heading-selector.png|Targeting a heading with hash syntax inside a wikilink]]

Typing `#` after the note name opens heading autocomplete, listing every heading (`H1` to `H6`) found in the target note.

## Custom Display Text (Aliases)

Use a vertical pipe (`|`) to set custom label text:

```markdown
[[Project Roadmap|our plan]]
```

Displays "our plan" in the note, but navigates to `Project Roadmap.md` when clicked.

## Navigating Links

- **Follow link**: `Ctrl+Click` (or `Cmd+Click` on macOS) opens the linked note in the active editor.
- **Create missing note**: Clicking an unresolved link creates the markdown file on disk immediately and opens it for editing.
- **Backlinks**: Resin's `MetadataCache` tracks every reference in memory, indexing which notes reference each other across the entire vault.



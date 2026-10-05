# Welcome to Resin

Resin is a local-first desktop markdown workspace for notes and tasks.

![[resin-workspace-overview.png|Overview of the Resin workspace showing an open note, file explorer sidebar, and left ribbon]]

---

## Your Files Belong to You

Resin stores your notes as plain `.md` files in a regular folder on your computer (your **Vault**). There is no proprietary database and no cloud lock-in.

- You can open your vault folder in VS Code, Obsidian, or the terminal anytime.
- Backing up your vault is as simple as copying the folder or initializing a Git repository.
- If you ever stop using Resin, your files and attachments remain completely intact.

---

## Speed and Architecture

- **Immediate in-memory updates**: Creating, moving, or renaming a note updates the UI instantly in memory before background disk writes complete.
- **Tauri 2 engine**: The backend runs lightweight Rust with SQLite for indexing, avoiding the memory footprint of traditional Electron apps.
- **Live Preview**: Markdown formatting tokens (like `#`, `**`, or `*`) collapse when your cursor moves away, keeping the editor clean while leaving raw text directly editable.

---

## Vault Structure

Resin stores configuration in a single `.resin/` folder at the root of your vault:

![[vault-file-structure.png|Directory tree showing a Resin vault with the .resin configuration folder]]

```text
<Your Vault>/
  ├── .resin/
  │   ├── settings.json       # App and editor preferences
  │   ├── workspace.json      # Open tabs and split layouts
  │   ├── snippets/           # Custom CSS snippets
  │   ├── themes/             # Installed themes
  │   └── plugins/            # Installed community plugins
  ├── attachments/            # Pasted and dropped images
  └── Your Notes.md
```

Everything inside `.resin/` is plain JSON and CSS, so your workspace settings can be committed to Git or synced across computers.

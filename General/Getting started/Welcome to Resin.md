# Welcome to Resin

Resin is a desktop knowledge workspace. At its core, it is two things joined together: an uncompromising markdown editor, and an extensible personal workspace.

---

## 1. Your Files Belong to You

Resin doesn't store your notes in a closed database or sync them to a hidden server. Everything you write lives as plain `.md` files in a regular folder on your computer (your **Vault**).

- You can open your vault folder in VS Code, Sublime Text, or the terminal anytime.
- If you stop using Resin tomorrow, your notes, images, and attachments remain yours. Nothing is held hostage.
- Backing up your vault is as simple as copying a folder or putting it in Git.

---

## 2. Speed and Feel

I put a lot of work into making sure Resin feels responsive:

- **Speedy Architecture**: When you create, rename, or move a file, the file explorer updates in memory synchronously before disk I/O even finishes in the background. No loading spinners because I hate em.
- **Tauri 2 Native Engine**: The backend runs lightweight native Rust with SQLite for full-text indexing, using a fraction of the memory that traditional Electron apps consume.
- **Markdown Live Preview**: Delimiters for bold, italic, headings, and code cleanly collapse when your cursor isn't touching them, giving you a clean visual preview while keeping raw markdown editable under the hood.

---

## 3. Where Things Live

Inside your vault folder, Resin maintains a single hidden folder: `.resin/`:

```
<Your Vault>/
  ├── .resin/
  │   ├── settings.json       # Your application and editor preferences
  │   ├── workspace.json      # Saved tab splits and open notes
  │   ├── snippets/           # Your custom CSS snippets
  │   ├── themes/             # Installed custom themes
  │   └── plugins/            # Installed plugins and their data
  ├── attachments/            # Dropped and pasted images
  └── Your Notes.md
```

Everything inside `.resin/` is plain JSON and CSS, making your entire workspace portable.

---

I hope Resin fits you and gives your thoughts a comfortable home. Let's get started by creating your first note.

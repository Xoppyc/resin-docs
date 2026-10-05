# Vault and Virtual File System

The Vault subsystem (`this.app.vault`) manages all files and folders in the workspace, pairing an in-memory Virtual File System (VFS) with asynchronous disk persistence.

![[vault-vfs-file-tree.png|Virtual file system tree and file metadata in Resin]]

## Nodes: `TFile` and `TFolder`

- `TAbstractFile`: Base class with `path`, `name`, and `parent`.
- `TFile`: File leaf node with `basename`, `extension`, and `stat` (`ctime`, `mtime`, `size`).
- `TFolder`: Folder container node with `children: TAbstractFile[]`.

## Reading and Writing Files

```ts
// Read note content
const content = await this.app.vault.read("Notes/Idea.md");

// Write note content (emits 'file:modified' event)
await this.app.vault.write("Notes/Idea.md", "# Updated Note\n\nContent.");

// Create a new note
const newFile = await this.app.vault.create("Notes/New.md", "# New Note\n");

// Create a folder
await this.app.vault.createFolder("Projects/2026");
```

## Renaming and Deleting

```ts
// Rename or move a note (automatically updates referencing wikilinks!)
await this.app.vault.rename("Notes/Old.md", "Notes/New.md");

// Send to system trash / recycle bin
await this.app.vault.trash("Notes/Old.md");
```

## DataAdapter

For low-level I/O outside the note cache (such as binary assets or plugin storage):

```ts
const adapter = this.app.vault.adapter;

const exists = await adapter.exists("data.bin");
const text = await adapter.read("config.json");
await adapter.write("config.json", JSON.stringify({ key: "val" }));
```


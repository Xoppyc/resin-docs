# Decoupled Context Menus

Resin features a decoupled, scope-based context menu system. Plugins can contribute action items, separators, and nested flyout submenus to any context menu in the app.

---

## 1. Context Menu Item Types

```ts
// 1. Standard Action Item
const actionItem: MenuAction = {
  type: "action",
  label: "Copy Note Title",
  icon: "CopyIcon",
  shortcut: "mod+c",
  onSelect: () => console.log("Copied!"),
};

// 2. Separator Line
const separator: MenuSeparator = {
  type: "separator",
};

// 3. Nested Flyout Submenu
const submenu: MenuSubmenu = {
  type: "submenu",
  label: "Export As...",
  icon: "ExportIcon",
  items: [
    { label: "PDF Document", onSelect: () => exportPdf() },
    { label: "HTML Web Page", onSelect: () => exportHtml() },
  ],
};
```

---

## 2. Registering Context Menu Providers

Register providers in your plugin using `this.registerContextMenu(scope, provider)`. The provider receives context data specific to that scope.

### Example: Adding Items to File Explorer

```ts
import { Plugin } from "resin";

export default class ExporterPlugin extends Plugin {
  override async onLoad(): Promise<void> {
    // Register items for right-clicking files in File Explorer
    this.registerContextMenu(
      "file-explorer:file",
      (context: { path: string; file: any }) => {
        // Only show for markdown files
        if (!context.path.endsWith(".md")) return null;

        return [
          { type: "separator" },
          {
            label: "Count Words",
            icon: "SparkleIcon",
            onSelect: async () => {
              const content = await this.app.vault.read(context.path);
              const words = content.split(/\s+/).filter(Boolean).length;
              this.app.toast.info(`Note contains ${words} words.`);
            },
          },
        ];
      },
    );
  }
}
```

---

## 3. Supported Core Scopes

| Scope                  | Context Payload                            | Description                                   |
| :--------------------- | :----------------------------------------- | :-------------------------------------------- |
| `tab:header`           | `{ leaf: WorkspaceLeaf, active: boolean }` | Right-clicking a tab header in any tab bar.   |
| `file-explorer:file`   | `{ path: string, file: TFile }`            | Right-clicking a file in the file explorer.   |
| `file-explorer:folder` | `{ path: string, folder: TFolder }`        | Right-clicking a folder in the file explorer. |
| `editor:text`          | `{ editor: any, selectedText: string }`    | Right-clicking inside the active note editor. |

---

## 4. Next Steps

- Learn about status bar widgets in **[[Status bar|Status Bar API]]**.
- Explore modals and alerts in **[[Dialogs and toasts|Modals, Banners & Toasts]]**.

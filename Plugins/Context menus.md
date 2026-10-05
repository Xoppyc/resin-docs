# Context Menus

Resin uses a decoupled, scope-based context menu registry. Plugins can inject actions, separators, and nested submenus into any right-click menu in the app.

![[plugin-context-menu-item.png|Custom plugin item injected into File Explorer context menu]]

## Register Context Menu Providers

Call `this.registerContextMenu(scope, provider)` in your plugin's `onLoad()`:

```ts
import { Plugin } from "resin-api";

export default class ExporterPlugin extends Plugin {
  override async onLoad(): Promise<void> {
    this.registerContextMenu(
      "file-explorer:file",
      (context: { path: string; file: any }) => {
        if (!context.path.endsWith(".md")) return null;

        return [
          { type: "separator" },
          {
            type: "action",
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

## Submenus

Inject flyout submenus by returning items with `type: "submenu"`:

```ts
{
  type: "submenu",
  label: "Export As...",
  icon: "ExportIcon",
  items: [
    { type: "action", label: "PDF Document", onSelect: () => exportPdf() },
    { type: "action", label: "HTML Web Page", onSelect: () => exportHtml() },
  ],
}
```

## Supported Scopes

| Scope | Context Payload | Target |
| :--- | :--- | :--- |
| `file-explorer:file` | `{ path: string, file: TFile }` | File row in File Explorer |
| `file-explorer:folder` | `{ path: string, folder: TFolder }` | Folder row in File Explorer |
| `tab:header` | `{ leaf: WorkspaceLeaf, active: boolean }` | Tab header in any tab bar |
| `editor:text` | `{ editor: any, selectedText: string }` | Text editor selection or canvas |


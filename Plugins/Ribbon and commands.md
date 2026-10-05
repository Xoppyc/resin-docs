# Ribbon and Commands

Register buttons in the left navigation ribbon and contribute commands accessible via keyboard shortcuts and the Command Palette.

![[ribbon-icon-registered.png|Custom plugin icon in left ribbon navigation]]

## Add Ribbon Icons

The left ribbon provides quick access to frequent actions:

```ts
import { Plugin } from "resin-api";

export default class QuickNotePlugin extends Plugin {
  override async onLoad(): Promise<void> {
    this.addRibbonIcon(
      "NotePencilIcon", // Phosphor icon identifier
      "Create Quick Note", // Tooltip
      () => {
        this.createQuickNote();
      },
    );
  }

  private async createQuickNote(): Promise<void> {
    const timestamp = new Date().toISOString().slice(0, 10);
    const path = `Quicknotes/Note-${timestamp}.md`;
    await this.app.vault.create(path, "# Quick Note\n\n");
    await this.app.workspace.openFile(path);
    this.app.toast.success("Created quick note");
  }
}
```

## Register Commands

Register commands using `this.addCommand()`:

![[custom-command-in-palette.png|Plugin command registered in the Command Palette]]

```ts
this.addCommand({
  id: "format-selection-uppercase",
  label: "Selection: Convert to Uppercase",
  shortcut: "mod+shift+u", // 'mod' maps to Ctrl (Win/Linux) or Cmd (macOS)
  handler: () => {
    const leaf = this.app.workspace.activeLeaf;
    if (!leaf || leaf.view.getViewType() !== "editor") {
      this.app.toast.info("Open an editor first");
      return;
    }
  },
});
```

### Command Properties

- `id`: Command identifier, automatically namespaced as `<pluginId>:<commandId>`.
- `label`: Display title in the [[Command palette]] and Settings.
- `shortcut`: Optional default keybinding (e.g. `mod+shift+k`, `alt+d`). Users can customize shortcuts in Settings.
- `handler`: Callback executed when triggered.


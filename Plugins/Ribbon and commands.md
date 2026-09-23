# Ribbon and Commands

Plugins can register buttons in the left navigation ribbon and contribute commands accessible through the keyboard and the Command Palette.

---

## 1. Adding Ribbon Icons

The left ribbon provides quick access to high-frequency actions. Call `this.addRibbonIcon()` inside your plugin's `onLoad()` lifecycle method:

```ts
import { Plugin } from "resin";

export default class QuickNotePlugin extends Plugin {
  override async onLoad(): Promise<void> {
    const ribbonIcon = this.addRibbonIcon(
      "NotePencilIcon", // Phosphor icon component name
      "Create Quick Note", // Hover tooltip
      () => {
        this.createQuickNote();
      },
    );
  }

  private async createQuickNote(): Promise<void> {
    const timestamp = new Date()
      .toISOString()
      .slice(0, 19)
      .replace(/[:T]/g, "-");
    const path = `Quicknotes/Note-${timestamp}.md`;
    await this.app.vault.create(path, "# Quick Note\n\n");
    await this.app.workspace.openFile(path);
    this.app.toast.success("Created quick note");
  }
}
```

When the plugin is unloaded or disabled, Resin automatically removes the ribbon button from the interface.

---

## 2. Registering Commands

Register commands using `this.addCommand()`:

```ts
this.addCommand({
  id: "format-selection-uppercase",
  label: "Selection: Convert to Uppercase",
  shortcut: "mod+shift+u", // 'mod' maps to Ctrl on Windows/Linux, Cmd on macOS
  handler: () => {
    const leaf = this.app.workspace.activeLeaf;
    if (!leaf || leaf.view.getViewType() !== "editor") {
      this.app.toast.info("Open an editor first");
      return;
    }
    // Perform transformation
  },
});
```

### Command Properties

- `id`: Unique identifier for the command within your plugin. Resin automatically prefixes it with your plugin ID (`<pluginId>:<commandId>`) to avoid namespace clashes.
- `label`: Human-readable title displayed in the [[Command palette]] and Settings.
- `shortcut`: Optional default keyboard shortcut (e.g. `mod+shift+k`, `alt+d`). Users can rebind this shortcut in **Settings → Hotkeys**.
- `handler`: Callback executed when the command is triggered from the palette or by its shortcut.

---

## 3. Command Palette Integration

Commands registered through `this.addCommand()` are indexed by the [[Command palette]] immediately. Users can open the palette with `Ctrl+P` and type any substring from your command's title or plugin name to find and execute it.

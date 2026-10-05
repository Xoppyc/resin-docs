# Plugin Quickstart

Build, test, and run a minimal Resin plugin in under 5 minutes.

![[word-counter-plugin-ribbon.png|Word counter plugin ribbon button and toast notification in Resin]]

## Minimal Plugin

A minimal plugin exports a default class extending `Plugin` from `resin-api`:

```ts
// src/index.ts
import { Plugin } from "resin-api";

export default class WordCounterPlugin extends Plugin {
  id = "community.word-counter";
  name = "Word Counter";

  override async onLoad(): Promise<void> {
    // 1. Add an icon to the left ribbon
    this.addRibbonIcon("SparkleIcon", "Count Vault Words", async () => {
      const files = this.app.vault.getMarkdownFiles();
      let totalWords = 0;

      for (const file of files) {
        const content = await this.app.vault.read(file.path);
        totalWords += content.trim().split(/\s+/).filter(Boolean).length;
      }

      this.app.toast.success(
        `Vault has ${totalWords.toLocaleString()} total words across ${files.length} notes!`,
      );
    });

    // 2. Register a command in the Command Palette
    this.addCommand({
      id: "show-word-count",
      label: "Count Total Vault Words",
      shortcut: "mod+shift+w",
      handler: () => {
        this.app.toast.info("Calculating vault word count...");
      },
    });
  }

  override async onUnload(): Promise<void> {
    await super.onUnload();
  }
}
```

## How It Works

- **Auto cleanup**: Commands, ribbon icons, and events registered via `this.addCommand()` or `this.addRibbonIcon()` clean up automatically when the plugin is disabled.
- **Sandboxed VFS**: File operations use `this.app.vault`, ensuring cache and watcher events trigger across the app.
- **Next steps**: See [[Plugin anatomy]] and [[Use React in your plugin]].


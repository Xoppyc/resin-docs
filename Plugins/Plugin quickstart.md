# Quickstart: Build a Plugin in 5 Minutes

This guide walks you through building, testing, and running your first Resin plugin from scratch.

---

## 1. Minimal Plugin Structure

A minimal Resin plugin consists of a single TypeScript file extending the `Plugin` class:

```ts
// src/index.ts
import { Plugin } from "resin";

export default class WordCounterPlugin extends Plugin {
  id = "community.word-counter";
  name = "Word Counter";

  override async onLoad(): Promise<void> {
    console.log("[WordCounter] Plugin loaded!");

    // 1. Add a Ribbon Icon on the left navigation bar
    this.addRibbonIcon("SparkleIcon", "Count Vault Words", async () => {
      const files = this.app.vault.getMarkdownFiles();
      let totalWords = 0;

      for (const file of files) {
        const content = await this.app.vault.read(file.path);
        totalWords += content.trim().split(/\s+/).filter(Boolean).length;
      }

      // 2. Show a Toast notification with the result
      this.app.toast.success(
        `Vault has ${totalWords.toLocaleString()} total words across ${files.length} notes!`,
      );
    });

    // 3. Register a command accessible in Command Palette & Shortcuts
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
    console.log("[WordCounter] Plugin unloaded.");
    await super.onUnload();
  }
}
```

---

## 2. Testing Your Plugin

1. When `WordCounterPlugin` is loaded by Resin, an icon appears in the left ribbon bar.
2. Clicking the ribbon icon reads all markdown notes asynchronously from the in-memory VFS via `this.app.vault.read(file.path)`.
3. A toast notification appears at the bottom of the window with the formatted word count.
4. Pressing <kbd>Ctrl+Shift+W</kbd> (or <kbd>Cmd+Shift+W</kbd> on macOS) triggers your registered command.

---

## 3. What Just Happened?

- **Automatic Event & Command Cleanup**: When the plugin is disabled or reloaded, all ribbon icons, commands, and event listeners registered through `this.addCommand()`, `this.addRibbonIcon()`, or `this.registerEvent()` are **automatically deregistered** with zero memory leaks.
- **Sandboxed Security**: File operations are routed through `this.app.vault` ensuring events (`file:modified`, `file:created`) fire properly.

---

## 4. Next Steps

- Explore **[[Plugin anatomy|Plugin Anatomy & Lifecycle]]** to understand manifests, storage, and settings.
- Learn about **[[App API sandbox|The AppAPI Sandbox]]**.

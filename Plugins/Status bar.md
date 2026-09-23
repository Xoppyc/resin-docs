# Status Bar API

The `StatusBarManager` subsystem (`app.statusBar`) controls the bottom status bar strip. It supports both **global persistent items** (available throughout the app) and **contextual items** (visible only when specific views or editor tabs are focused).

---

## 1. Registering Persistent Global Status Items

```ts
import { Plugin } from "resin";

export default class SyncStatusPlugin extends Plugin {
  override async onLoad(): Promise<void> {
    // 1. Register a persistent status item on the bottom right
    const syncItem = this.app.statusBar.registerItem({
      id: "sync-status",
      alignment: "right", // 'left' | 'right'
      priority: 100, // Ordering priority (higher = closer to edge)
      text: "Sync: Ready",
      icon: "CloudCheckIcon",
      tooltip: "Vault is synchronized",
      onClick: () => {
        this.app.toast.info("Triggering manual sync...");
      },
    });

    // 2. Dynamically update text and icon
    syncItem.update({
      text: "Syncing...",
      icon: "SpinnerIcon",
    });

    // 3. Remove item when no longer needed
    // syncItem.remove()
  }
}
```

---

## 2. Contextual Items (Per-View Status Items)

Views can set temporary contextual items that appear when the leaf is active and vanish when blurred:

```ts
export class EditorWordCountView extends View {
  override onActivate(): void {
    // Show word count in status bar while this note is focused
    this.app.statusBar.setContextItems([
      {
        id: "note-word-count",
        alignment: "right",
        text: "1,420 words",
        icon: "TextAaIcon",
      },
    ]);
  }

  override onDeactivate(): void {
    // Clear contextual items when losing focus
    this.app.statusBar.clearContextItems();
  }
}
```

---

## 3. Next Steps

- Trigger alerts and toasts in **[[Dialogs and toasts|Modals, Banners & Toasts]]**.
- Add commands and ribbon buttons in **[[Ribbon and commands|Ribbon & Command Palette]]**.

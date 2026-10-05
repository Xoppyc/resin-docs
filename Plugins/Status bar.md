# Status Bar

The status bar API (`this.app.statusBar`) lets plugins add items to the bottom status bar, including global persistent indicators and contextual items tied to active views.

![[status-bar-item-example.png|Plugin status item mounted in the bottom status bar]]

## Register Persistent Items

```ts
import { Plugin } from "resin-api";

export default class SyncStatusPlugin extends Plugin {
  override async onLoad(): Promise<void> {
    const syncItem = this.app.statusBar.registerItem({
      id: "sync-status",
      alignment: "right", // 'left' | 'right'
      priority: 100, // Higher numbers place items closer to the edge
      text: "Sync: Ready",
      icon: "CloudCheckIcon",
      tooltip: "Vault is synchronized",
      onClick: () => {
        this.app.toast.info("Triggering manual sync...");
      },
    });

    // Dynamically update text and icon
    syncItem.update({
      text: "Syncing...",
      icon: "SpinnerIcon",
    });
  }
}
```

## Contextual Items

Views can set temporary status items that appear only when the leaf is focused:

```ts
export class EditorWordCountView extends View {
  override onActivate(): void {
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
    this.app.statusBar.clearContextItems();
  }
}
```


# Events and Lifecycle

Resin uses a decoupled event bus (`this.app.events`). Subscribing through `this.registerEvent(...)` ensures listeners clean up automatically when the plugin unloads.

## Event Subscription

```ts
import { Plugin } from "resin-api";

export default class TrackerPlugin extends Plugin {
  override async onLoad(): Promise<void> {
    // Vault File Events
    this.registerEvent(
      this.app.events.on("file:created", (file) => {
        console.log("File created:", file.path);
      }),
    );

    this.registerEvent(
      this.app.events.on("file:modified", (file) => {
        console.log("File modified:", file.path);
      }),
    );

    this.registerEvent(
      this.app.events.on("file:renamed", ({ oldPath, newPath }) => {
        console.log(`Renamed from ${oldPath} to ${newPath}`);
      }),
    );

    // Workspace Events
    this.registerEvent(
      this.app.workspace.on("active-leaf-change", (leaf) => {
        console.log("Active tab changed:", leaf?.view.getViewType());
      }),
    );
  }
}
```

## Event Catalog

| Event | Source | Payload | Description |
| :--- | :--- | :--- | :--- |
| `file:created` | `app.events` | `TFile` | A file was created in the vault. |
| `file:modified` | `app.events` | `TFile` | A file's content was updated. |
| `file:deleted` | `app.events` | `string` | A file was deleted. |
| `file:renamed` | `app.events` | `{ oldPath, newPath }` | A file was renamed or moved. |
| `active-leaf-change` | `app.workspace` | `WorkspaceLeaf \| null` | The active tab changed. |
| `layout:changed` | `app.workspace` | — | Layout splits, leaves, or sidebars changed. |


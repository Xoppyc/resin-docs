# Event Bus & Subscriptions

Resin uses a lightweight, decoupled pub/sub event system based on `Events` and `EventRef` tokens.

---

## 1. Subscribing with Automatic Cleanup

Always subscribe using `this.registerEvent(...)` inside your plugin class. This guarantees all subscriptions are automatically removed when the plugin unloads:

```ts
import { Plugin } from "resin";

export default class TrackerPlugin extends Plugin {
  override async onLoad(): Promise<void> {
    // 1. Vault File Events
    this.registerEvent(
      this.app.events.on("file:created", (file) => {
        console.log("New note created:", file.path);
      }),
    );

    this.registerEvent(
      this.app.events.on("file:modified", (file) => {
        console.log("Note modified:", file.path);
      }),
    );

    this.registerEvent(
      this.app.events.on("file:deleted", (path) => {
        console.log("Note deleted:", path);
      }),
    );

    this.registerEvent(
      this.app.events.on("file:renamed", ({ oldPath, newPath }) => {
        console.log(`Note renamed from ${oldPath} to ${newPath}`);
      }),
    );

    // 2. Workspace Layout & Active Leaf Events
    this.registerEvent(
      this.app.workspace.on("active-leaf-change", (leaf) => {
        console.log("Active leaf changed to:", leaf?.getType());
      }),
    );

    this.registerEvent(
      this.app.workspace.on("layout:changed", () => {
        console.log("Workspace layout changed");
      }),
    );
  }
}
```

---

## 2. Core Event Catalog

| Event Name           | Emitter         | Payload                                | Description                              |
| :------------------- | :-------------- | :------------------------------------- | :--------------------------------------- |
| `file:created`       | `app.events`    | `TFile`                                | A file was created in the vault.         |
| `file:modified`      | `app.events`    | `TFile`                                | A file's content was modified.           |
| `file:deleted`       | `app.events`    | `string`                               | A file was deleted/trashed.              |
| `file:renamed`       | `app.events`    | `{ oldPath: string, newPath: string }` | A file was renamed or moved.             |
| `folder:created`     | `app.events`    | `TFolder`                              | A folder directory was created.          |
| `active-leaf-change` | `app.workspace` | `WorkspaceLeaf \| null`                | The user focused or switched tabs.       |
| `layout:changed`     | `app.workspace` | _(none)_                               | Splits, tabs, or sidebar states changed. |
| `settings:updated`   | `app.settings`  | `CoreSettings`                         | Global configuration options changed.    |

---

## 3. Emitting Custom Events

You can use the central event bus for inter-plugin communication:

```ts
// Emit a custom event
this.app.events.emit("my-plugin:task-completed", {
  id: "task-101",
  done: true,
});

// Listen for the custom event
this.registerEvent(
  this.app.events.on("my-plugin:task-completed", (data) => {
    console.log("Task completed:", data.id);
  }),
);
```

---

## 4. Next Steps

- Build custom views in **[[Custom views|Custom Workspace Views]]**.
- Use framework helpers in **[[Framework helpers|Framework Render Helpers]]**.

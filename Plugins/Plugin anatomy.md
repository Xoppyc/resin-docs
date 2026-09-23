# Plugin Anatomy & Lifecycle

Every plugin in Resin inherits from the `Plugin` base class. This document covers plugin properties, lifecycle hooks, manifest structure, and automatic resource tracking.

---

## 1. Plugin Class Structure

```ts
import { Plugin } from "resin";

export default class MyPlugin extends Plugin {
  /**
   * Unique identifier for the plugin (e.g. 'core.bookmarks' or 'user.kanban').
   * Used for namespacing commands and isolated persistent storage.
   */
  id = "community.my-plugin";

  /**
   * Human-readable display name shown in Settings > Community Plugins.
   */
  name = "My Plugin";

  /**
   * Invoked when the plugin is loaded during app startup or when enabled by the user.
   */
  override async onLoad(): Promise<void> {
    // Initialize UI, register views, ribbon buttons, commands, and events here.
  }

  /**
   * Invoked when the plugin is disabled or the app is shutting down.
   */
  override async onUnload(): Promise<void> {
    // Custom cleanup. (Note: standard events and commands clean up automatically via super.onUnload())
    await super.onUnload();
  }
}
```

---

## 2. The `onLoad()` Lifecycle Hook

When Resin initializes a plugin:

1. An isolated, sandboxed **`AppAPI`** instance is injected into `this.app`.
2. `await this.onLoad()` is executed.
3. Any views registered via `this.registerView()` become mountable in workspace leaves.
4. Any ribbon icons registered via `this.addRibbonIcon()` appear in the left sidebar.
5. Any commands registered via `this.addCommand()` become available in the command palette.

---

> [!NOTE]
> All UI registrations, commands, and event listeners made via `Plugin` helper methods are automatically tracked and safely destroyed when the plugin is disabled or reloaded.

---

## 3. The `onUnload()` Lifecycle Hook & Automatic Cleanup

Resin guarantees zero resource leaks when plugins are reloaded or disabled:

- **Tracked Event Listeners**: Calling `this.registerEvent(app.events.on(...))` ensures the event listener is automatically unbound when the plugin unloads.
- **Tracked Context Menus**: Calling `this.registerContextMenu(scope, provider)` unregisters the context menu provider on unload.
- **Tracked Editor Suggestions**: Calling `this.registerEditorSuggest(suggest)` unregisters autocompletion providers from CodeMirror.
- **Tracked Code Blocks**: Calling `this.registerCodeBlockHandler(language, handler)` cleans up markdown code block processors.
- **Custom Cleanups**: If your plugin opens WebSockets, worker threads, or timers, register a cleanup callback:
  ```ts
  const timer = setInterval(() => this.poll(), 5000);
  this.register(() => clearInterval(timer));
  ```

> [!TIP]
> Use `this.register(teardownFn)` whenever managing external side-effects (e.g. intervals, external DOM observers, or native event listeners) to ensure they are cleaned up on plugin unload.

---

## 4. Helper Methods on `Plugin`

| Method                                      | Description                                                            |
| :------------------------------------------ | :--------------------------------------------------------------------- |
| `this.addRibbonIcon(icon, title, callback)` | Adds an action button to the left navigation ribbon.                   |
| `this.addCommand(commandDef)`               | Registers a command palette entry with optional keybinding.            |
| `this.addSettingTab(tab)`                   | Registers a settings tab in Settings > Plugin Options.                 |
| `this.registerView(type, creator, meta)`    | Registers a custom workspace view type mounted into leaves.            |
| `this.registerContextMenu(scope, provider)` | Adds contextual menu items to right-click menus.                       |
| `this.registerEditorSuggest(suggest)`       | Registers a custom autocomplete provider in the markdown editor.       |
| `this.registerCodeBlockHandler(lang, fn)`   | Registers a custom fenced code block processor (e.g. ````mermaid`).    |
| `this.registerEvent(eventRef)`              | Binds an event listener with automatic lifecycle cleanup.              |
| `this.register(teardownFn)`                 | Registers an arbitrary teardown function executed on unload.           |
| `this.loadData<T>()`                        | Loads persistent JSON data from `.resin/plugins/<pluginId>/data.json`. |
| `this.saveData<T>(data)`                    | Saves persistent JSON data to `.resin/plugins/<pluginId>/data.json`.   |

> [!IMPORTANT]
> Never store persistent data outside of `this.loadData()` and `this.saveData()`. Resin ensures plugin data is isolated to its own JSON file, protecting against cross-plugin data corruption.

---

## 5. Next Steps

- Learn about **[[App API sandbox|The AppAPI Sandbox]]**.
- Read how to work with the **[[Vault and VFS|Vault & In-Memory VFS]]**.
- Build autocompletions with **[[Editor suggestions|Editor Suggestions]]**.

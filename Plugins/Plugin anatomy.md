# Plugin Anatomy and Lifecycle

Every Resin plugin inherits from `Plugin` and interacts with the application exclusively through its injected `this.app` (`AppAPI`) sandbox.

![[plugin-lifecycle-cleanup.png|Plugin load and unload lifecycle in developer console]]

## Plugin Class Structure

```ts
import { Plugin } from "resin-api";

export default class MyPlugin extends Plugin {
  id = "community.my-plugin";
  name = "My Plugin";

  override async onLoad(): Promise<void> {
    // Register commands, ribbon buttons, views, or events
  }

  override async onUnload(): Promise<void> {
    // Custom teardown. Standard registrations clean up automatically.
    await super.onUnload();
  }
}
```

## Lifecycle and Resource Cleanup

When a plugin loads, Resin instantiates it with a sandboxed `AppAPI`. When disabled or reloaded, all resources registered through `Plugin` methods are safely destroyed:

- **Events**: `this.registerEvent(eventRef)` unbinds listeners automatically on unload.
- **Commands & Ribbon**: `this.addCommand()` and `this.addRibbonIcon()` remove elements automatically.
- **Custom side-effects**: Use `this.register(teardownFn)` for timers, intervals, or DOM observers:
  ```ts
  const timer = setInterval(() => this.tick(), 10000);
  this.register(() => clearInterval(timer));
  ```

## Helper Methods

| Method | Description |
| :--- | :--- |
| `this.addRibbonIcon(icon, title, callback)` | Adds an action icon to the left ribbon. |
| `this.addCommand(commandDef)` | Registers a command palette entry with optional hotkey. |
| `this.addSettingTab(tab)` | Registers a settings panel under Plugin Options. |
| `this.registerView(type, creator, meta)` | Registers a custom workspace view for leaves. |
| `this.registerContextMenu(scope, provider)` | Injects items into right-click context menus. |
| `this.registerCodeBlockHandler(lang, fn)` | Registers a custom markdown code block widget. |
| `this.registerEditorSuggest(suggest)` | Injects autocomplete suggestions into the editor. |
| `this.loadData<T>()` / `saveData<T>(data)` | Manages isolated JSON storage in `.resin/plugins/<id>/data.json`. |


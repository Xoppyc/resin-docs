# Custom Workspace Views

Custom views in Resin are classes extending the `View` base class that are mounted into `WorkspaceLeaf` slots across the sidebar or main splits.

---

## 1. Anatomy of a Custom View

```ts
import { View, type WorkspaceLeaf } from "resin";

export class CustomDashboardView extends View {
  constructor(leaf: WorkspaceLeaf) {
    super(leaf);
  }

  /**
   * Unique view type identifier.
   */
  getViewType(): string {
    return "custom-dashboard";
  }

  /**
   * Label shown on the tab header.
   */
  getDisplayText(): string {
    return "Dashboard";
  }

  /**
   * Phosphor icon name for tab header.
   */
  override getIcon(): string {
    return "ChartLineUpIcon";
  }

  /**
   * Invoked when the view is mounted into the DOM.
   */
  override async onLoad(): Promise<void> {
    this.containerEl.innerHTML = `
      <div style="padding: 24px;">
        <h2>Dashboard View</h2>
        <p>Mounted in Leaf: ${this.leaf.id}</p>
      </div>
    `;
  }

  /**
   * Invoked when the view is unmounted or tab closed.
   */
  override async onUnload(): Promise<void> {
    // Teardown custom listeners or timers
    await super.onUnload();
  }
}
```

---

## 2. Registering and Opening the View

In your plugin's `onLoad()`:

```ts
import { Plugin } from "resin";
import { CustomDashboardView } from "./CustomDashboardView";

export default class DashboardPlugin extends Plugin {
  id = "community.dashboard";
  name = "Custom Dashboard";

  override async onLoad(): Promise<void> {
    // 1. Register the view with icon and default title
    this.registerView(
      "custom-dashboard",
      (leaf) => new CustomDashboardView(leaf),
      {
        icon: "ChartLineUpIcon",
        title: "Dashboard",
      },
    );

    // 2. Add Ribbon Icon to reveal the view
    this.addRibbonIcon("ChartLineUpIcon", "Open Dashboard", () => {
      this.app.workspace.revealView("custom-dashboard", "main");
    });
  }
}
```

---

## 3. View State Serialization & Restoration

Resin automatically saves your workspace tabs to disk (`workspace.json`). To persist view configuration:

```ts
export class CustomDashboardView extends View {
  private filter = "all";

  // 1. Return state object to persist
  override getState(): any {
    return { filter: this.filter };
  }

  // 2. Restore state on startup
  override async setState(state: any): Promise<void> {
    if (state?.filter) {
      this.filter = state.filter;
    }
  }
}
```

---

## 4. Next Steps

- Render modern UI using **[[Framework helpers|Framework Render Helpers]]** (`renderReact()` and `renderSvelte()`).
- Add right-click options using **[[Context menus|Decoupled Context Menus]]**.

# Custom Views

Custom views extend the `View` base class and mount into `WorkspaceLeaf` slots in the main workspace splits or sidebars.

![[custom-view-mounted-leaf.png|Custom plugin view mounted in a workspace leaf]]

## Anatomy of a View

```ts
import { View, type WorkspaceLeaf } from "resin-api";

export class CustomDashboardView extends View {
  constructor(leaf: WorkspaceLeaf) {
    super(leaf);
  }

  getViewType(): string {
    return "custom-dashboard";
  }

  getDisplayText(): string {
    return "Dashboard";
  }

  override getIcon(): string {
    return "ChartLineUpIcon";
  }

  override async onOpen(): Promise<void> {
    this.contentEl.innerHTML = `
      <div style="padding: 24px;">
        <h2>Dashboard</h2>
        <p>Leaf ID: ${this.leaf.id}</p>
      </div>
    `;
  }

  override async onClose(): Promise<void> {
    await super.onClose();
  }
}
```

## Register and Open the View

Register the view in your plugin's `onLoad()`:

```ts
import { Plugin } from "resin-api";
import { CustomDashboardView } from "./CustomDashboardView";

export default class DashboardPlugin extends Plugin {
  override async onLoad(): Promise<void> {
    this.registerView(
      "custom-dashboard",
      (leaf) => new CustomDashboardView(leaf),
      {
        icon: "ChartLineUpIcon",
        title: "Dashboard",
      },
    );

    this.addRibbonIcon("ChartLineUpIcon", "Open Dashboard", async () => {
      const leaf = this.app.workspace.getRightLeaf();
      await leaf?.setViewState({ type: "custom-dashboard", active: true });
    });
  }
}
```

## State Persistence

Resin saves open tabs to `.resin/workspace.json`. Implement `getState()` and `setState()` to save and restore tab configuration:

```ts
override getState(): any {
  return { filter: this.filter };
}

override async setState(state: any): Promise<void> {
  if (state?.filter) {
    this.filter = state.filter;
  }
}
```


# Framework Render Helpers (`renderReact` & `renderSvelte`)

Resin provides zero-boilerplate helper methods on `View` to mount components using **React 19** or **Svelte 5** with automated lifecycle and unmount handling.

---

## 1. Using React 19: `this.renderReact(element)`

`renderReact()` creates or updates a React root inside the view's container element and automatically provides `<AppContext.Provider value={this.app}>`.

### Example React View

```tsx
// MyReactView.tsx
import React, { useState, useEffect } from "react";
import { View, type WorkspaceLeaf, useApp } from "resin";

function CounterComponent() {
  const app = useApp();
  const [count, setCount] = useState(0);
  const [notesCount, setNotesCount] = useState(
    app.vault.getMarkdownFiles().length,
  );

  return (
    <div style={{ padding: 20 }}>
      <h2>React View</h2>
      <p>
        Total Vault Notes: <strong>{notesCount}</strong>
      </p>
      <button onClick={() => setCount((c) => c + 1)}>Clicks: {count}</button>
    </div>
  );
}

export class MyReactView extends View {
  constructor(leaf: WorkspaceLeaf) {
    super(leaf);
  }

  getViewType(): string {
    return "my-react-view";
  }

  getDisplayText(): string {
    return "React Counter";
  }

  override async onLoad(): Promise<void> {
    // Mounts React component with AppContext and auto-cleanup
    this.renderReact(<CounterComponent />);
  }
}
```

---

## 2. Using Svelte 5: `this.renderSvelte(Component, props)`

`renderSvelte()` mounts a Svelte 5 component inside the view container, automatically passes `{ app: this.app, leaf: this.leaf }` in props, and unmounts cleanly on unload.

### Svelte Component

```svelte
<!-- CanvasWidget.svelte -->
<script lang="ts">
  import type { AppAPI, WorkspaceLeaf } from 'resin'

  let { app, leaf }: { app: AppAPI; leaf: WorkspaceLeaf } = $props()
  let notes = $state(app.vault.getMarkdownFiles().length)
</script>

<div class="svelte-card">
  <h2>Svelte 5 Widget</h2>
  <p>Vault note count: {notes}</p>
</div>

<style>
  .svelte-card {
    padding: 1.5rem;
    color: var(--text-normal);
  }
</style>
```

### Svelte View Class

```ts
// MySvelteView.ts
import { View, type WorkspaceLeaf } from "resin";
import CanvasWidget from "./CanvasWidget.svelte";

export class MySvelteView extends View {
  constructor(leaf: WorkspaceLeaf) {
    super(leaf);
  }

  getViewType(): string {
    return "my-svelte-view";
  }

  getDisplayText(): string {
    return "Svelte Widget";
  }

  override async onLoad(): Promise<void> {
    // Mounts Svelte component with auto-cleanup
    this.renderSvelte(CanvasWidget);
  }
}
```

---

## 3. Automatic Lifecycle Management

When the tab is closed or the plugin is unloaded:

- `this.onUnload()` in `View` automatically calls `root.unmount()` for React roots.
- `this.onUnload()` in `View` automatically calls `unmount(instance)` for Svelte 5 components.
- There are **zero lingering DOM nodes or background timers**.

---

## 4. Next Steps

- Add context menu actions with **[[Context menus|Decoupled Context Menus]]**.
- Add status indicators with **[[Status bar|Status Bar API]]**.

# Use React in Your Plugin

Resin embeds React 19 in its runtime. Your plugin can use React components for custom views, sidebars, modals, or settings tabs without bundling a duplicate copy of React into your output file.

![[react-plugin-view-preview.png|Custom React sidebar panel mounted inside a workspace leaf]]

## Configure Dependencies

In your plugin's `package.json`, install React and type definitions as developer dependencies:

```bash
npm install --save-dev react react-dom @types/react @types/react-dom
```

Configure your bundler (`esbuild.config.mjs`) to mark `react`, `react-dom`, and `react/jsx-runtime` as external:

```js
// esbuild.config.mjs
import esbuild from "esbuild";

esbuild.build({
  entryPoints: ["src/index.ts"],
  bundle: true,
  format: "cjs",
  target: "es2022",
  outfile: "main.js",
  external: [
    "resin-api",
    "react",
    "react-dom",
    "react/jsx-runtime"
  ],
});
```

Because Resin provides these modules through its plugin sandbox, your final `main.js` stays lightweight and avoids React hook collisions.

In `tsconfig.json`, enable JSX:

```json
{
  "compilerOptions": {
    "jsx": "react-jsx",
    "moduleResolution": "node"
  }
}
```

## Create a React View Component

```tsx
// src/components/CounterView.tsx
import React, { useState } from "react";
import type { AppAPI } from "resin-api";

interface Props {
  app: AppAPI;
}

export const CounterView: React.FC<Props> = ({ app }) => {
  const [count, setCount] = useState(0);

  return (
    <div className="p-4 flex flex-col gap-3">
      <h3 className="text-lg font-semibold">React Counter</h3>
      <p className="text-sm text-neutral-400">
        Vault contains {app.vault.getMarkdownFiles().length} notes.
      </p>
      <button
        onClick={() => setCount((c) => c + 1)}
        className="px-3 py-1.5 bg-blue-600 text-white rounded hover:bg-blue-500"
      >
        Clicks: {count}
      </button>
    </div>
  );
};
```

## Mount into a Workspace Leaf

In your plugin class, register a view and mount the component with `createRoot`:

```tsx
// src/index.ts
import { Plugin, View } from "resin-api";
import { createRoot, Root } from "react-dom/client";
import { CounterView } from "./components/CounterView";

const VIEW_TYPE_COUNTER = "my-plugin:counter-view";

class ReactCounterView extends View {
  private root: Root | null = null;

  getViewType(): string {
    return VIEW_TYPE_COUNTER;
  }

  getDisplayText(): string {
    return "React Counter";
  }

  getIcon(): string {
    return "SparkleIcon";
  }

  override async onOpen(): Promise<void> {
    this.root = createRoot(this.contentEl);
    this.root.render(<CounterView app={this.app} />);
  }

  override async onClose(): Promise<void> {
    this.root?.unmount();
    this.root = null;
  }
}

export default class MyReactPlugin extends Plugin {
  override async onLoad(): Promise<void> {
    this.registerView(VIEW_TYPE_COUNTER, (leaf) => new ReactCounterView(leaf));

    this.addRibbonIcon("SparkleIcon", "Open React Counter", async () => {
      await this.app.workspace.getRightLeaf()?.setViewState({
        type: VIEW_TYPE_COUNTER,
        active: true,
      });
    });
  }
}
```

Always call `this.root.unmount()` inside `onClose()` to ensure listeners, timers, and component state clean up when the user closes the tab.

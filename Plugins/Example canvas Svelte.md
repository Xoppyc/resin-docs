# Recipe: Svelte 5 Canvas Tool Plugin

This recipe demonstrates building an interactive 60 FPS Canvas visualizer with **Svelte 5** and mounting it into a central workspace tab.

---

## 1. Svelte 5 Canvas Component

```svelte
<!-- src/CanvasTool.svelte -->
<script lang="ts">
  import { onMount } from 'svelte'
  import type { AppAPI, WorkspaceLeaf } from 'resin'

  let { app, leaf }: { app: AppAPI; leaf: WorkspaceLeaf } = $props()
  let canvasEl: HTMLCanvasElement

  onMount(() => {
    const ctx = canvasEl.getContext('2d')
    if (!ctx) return

    let animId: number
    let angle = 0

    const render = () => {
      ctx.clearRect(0, 0, canvasEl.width, canvasEl.height)

      const centerX = canvasEl.width / 2
      const centerY = canvasEl.height / 2

      ctx.save()
      ctx.translate(centerX, centerY)
      ctx.rotate(angle)

      ctx.fillStyle = '#6366f1'
      ctx.fillRect(-50, -50, 100, 100)

      ctx.restore()
      angle += 0.02
      animId = requestAnimationFrame(render)
    }

    render()

    return () => {
      cancelAnimationFrame(animId)
    }
  })
</script>

<div class="canvas-container">
  <canvas bind:this={canvasEl} width="600" height="400"></canvas>
</div>

<style>
  .canvas-container {
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    background: var(--bg-primary);
  }
</style>
```

---

## 2. Svelte View Class

```ts
// src/CanvasToolView.ts
import { View, type WorkspaceLeaf } from 'resin';
import CanvasTool from './CanvasTool.svelte';

export class CanvasToolView extends View {
  constructor(leaf: WorkspaceLeaf) {
    super(leaf);
  }

  getViewType(): string {
    return 'canvas-tool';
  }

  getDisplayText(): string {
    return 'Interactive Canvas';
  }

  override getIcon(): string {
    return 'PaintBrushIcon';
  }

  override async onLoad(): Promise<void> {
    // Mounts Svelte component with auto-cleanup
    this.renderSvelte(CanvasTool);
  }
}
```

---

## 3. Plugin Registration

```ts
// src/index.ts
import { Plugin } from 'resin';
import { CanvasToolView } from './CanvasToolView';

export default class CanvasToolPlugin extends Plugin {
  id = 'community.canvas-tool';
  name = 'Canvas Tool';

  override async onLoad(): Promise<void> {
    this.registerView('canvas-tool', (leaf) => new CanvasToolView(leaf), {
      icon: 'PaintBrushIcon',
      title: 'Canvas Tool',
    });

    this.addCommand({
      id: 'open-canvas-tool',
      label: 'Open Canvas Tool',
      handler: () => {
        this.app.workspace.revealView('canvas-tool', 'main');
      },
    });
  }
}
```

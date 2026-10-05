# Code Blocks

Code block processors let your plugin render custom interactive widgets, charts, diagrams, or live UI components inside Markdown code fences.

![[interactive-codeblock-widget.png|Interactive code block widget rendered in Live Preview]]

## Register a Code Block Handler

Call `this.registerCodeBlockHandler(language, handler)` inside your plugin's `onLoad()`:

```ts
import { Plugin } from "resin-api";

export default class HttpPlaygroundPlugin extends Plugin {
  override async onLoad(): Promise<void> {
    this.registerCodeBlockHandler("http-request", async (source, el, ctx) => {
      // 1. Parse your code block contents
      const trimmed = source.trim();

      // 2. Build your UI inside the el container
      el.innerHTML = `
        <div class="flex items-center gap-2 p-2 bg-neutral-900 rounded border border-neutral-800">
          <span class="text-xs font-mono text-emerald-400">POST</span>
          <span class="text-xs font-mono text-neutral-200 flex-1">${trimmed}</span>
          <button class="px-2 py-1 text-xs bg-blue-600 hover:bg-blue-500 text-white rounded send-btn">
            Send
          </button>
        </div>
      `;

      // 3. Attach interactive events
      const btn = el.querySelector(".send-btn");
      btn?.addEventListener("click", async () => {
        const res = await this.app.request({ url: trimmed, method: "POST" });
        this.app.toast.success(`Response status: ${res.status}`);
      });
    });
  }
}
```

## Interactive Controls and Event Propagation

In CodeMirror's Live Preview, clicking normal widget areas switches the cursor into source Markdown mode.

To keep interactive controls functional without collapsing back to code fences, Resin checks for interactive elements:
- Native interactive tags (`<button>`, `<input>`, `<select>`, `<textarea>`, `<a>`)
- Elements with ARIA roles `[role="button"]` or `[contenteditable="true"]`
- Any element with the CSS class `.resin-interactive`

Resin stops `mousedown` and `pointerdown` events on these elements from reaching CodeMirror, keeping focus and clicks on your UI controls:

```html
<!-- Example of custom interactive element -->
<div class="resin-interactive slider-track" role="slider">
  ...
</div>
```

## Context Information

The `ctx` object passed to your handler provides metadata about the enclosing note:
- `ctx.sourcePath`: File path of the active note.
- `ctx.from`: Document offset start position.
- `ctx.to`: Document offset end position.

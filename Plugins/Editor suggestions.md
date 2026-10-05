# Editor Suggestions

Resin provides an autocomplete suggestion framework powered by CodeMirror 6. Plugins can register custom suggestion providers for slash commands, tags, emoji pickers, or link autocompletions.

![[editor-suggest-popup.png|Custom emoji autocomplete popup inside CodeMirror]]

## Overview

Extend `EditorSuggest<T>` and register it with `this.registerEditorSuggest()`:

```
User types trigger (e.g. ':', '/')
          │
          ▼
   suggest.onTrigger() ──► Returns match { start, end, query }
          │
          ▼
 suggest.getSuggestions() ──► Returns items (sync or async)
          │
          ▼
 suggest.renderSuggestion() ──► Renders popup row HTML
          │
          ▼
 suggest.selectSuggestion() ──► Inserts selection into document
```

## Example: Emoji Autocomplete

```ts
import {
  Plugin,
  EditorSuggest,
  type EditorSuggestContext,
  type EditorSuggestTriggerInfo,
} from "resin-api";
import type { EditorView } from "@codemirror/view";
import type { TFile } from "resin-api";

interface EmojiItem {
  emoji: string;
  name: string;
}

const EMOJIS: EmojiItem[] = [
  { emoji: "🚀", name: "rocket" },
  { emoji: "✨", name: "sparkles" },
  { emoji: "💡", name: "idea" },
  { emoji: "🔥", name: "fire" },
];

class EmojiSuggest extends EditorSuggest<EmojiItem> {
  onTrigger(
    cursor: number,
    view: EditorView,
    _file: TFile | null,
  ): EditorSuggestTriggerInfo | null {
    const line = view.state.doc.lineAt(cursor);
    const textBefore = line.text.slice(0, cursor - line.from);
    const match = textBefore.match(/:([a-zA-Z0-9_+-]*)$/);
    if (!match) return null;

    const query = match[1];
    return {
      start: cursor - query.length - 1,
      end: cursor,
      query,
    };
  }

  getSuggestions(context: EditorSuggestContext): EmojiItem[] {
    const q = context.query.toLowerCase();
    return EMOJIS.filter((item) => item.name.includes(q));
  }

  renderSuggestion(item: EmojiItem, el: HTMLElement): void {
    el.innerHTML = `<span>${item.emoji}</span> <span>:${item.name}:</span>`;
  }

  selectSuggestion(item: EmojiItem, _evt: Event, view: EditorView): void {
    const cursor = view.state.selection.main.head;
    const line = view.state.doc.lineAt(cursor);
    const textBefore = line.text.slice(0, cursor - line.from);
    const match = textBefore.match(/:([a-zA-Z0-9_+-]*)$/);
    if (!match) return;

    const start = cursor - match[1].length - 1;
    view.dispatch({
      changes: { from: start, to: cursor, insert: item.emoji },
      selection: { anchor: start + item.emoji.length },
    });
  }
}

export default class EmojiPlugin extends Plugin {
  override async onLoad(): Promise<void> {
    this.registerEditorSuggest(new EmojiSuggest(this.app));
  }
}
```

Registering with `this.registerEditorSuggest()` ensures the provider unbinds cleanly when your plugin unloads.

# 07 — Editor Suggestions (EditorSuggest API)

Resin provides an open, Obsidian-grade autocomplete suggestion framework powered by CodeMirror 6. Plugins can register custom suggestion providers to trigger interactive popups for slash commands, tags, emoji pickers, math macros, and link autocomplete.

---

## 1. Overview

Instead of hacking CodeMirror extensions directly, plugins extend the abstract **`EditorSuggest<T>`** class and register it via `this.registerEditorSuggest()`:

```
User Types in Editor (e.g. ':', '/', '[[')
                 │
                 ▼
     CodeMirror Suggest Driver
                 │
                 ▼ (queries active suggests in priority order)
       suggest.onTrigger() ──► Returns match { start, end, query }
                 │
                 ▼
      suggest.getSuggestions() ──► Fetches items (sync or async)
                 │
                 ▼
      suggest.renderSuggestion() ──► Populates popup row HTML
                 │
                 ▼ (User hits Enter, Tab, or clicks)
      suggest.selectSuggestion() ──► Applies transaction to editor
```

---

## 2. The `EditorSuggest<T>` Base Class

```ts
import {
  EditorSuggest,
  type EditorSuggestContext,
  type EditorSuggestTriggerInfo,
} from 'resin';
import type { EditorView } from '@codemirror/view';
import type { TFile } from 'resin';

export abstract class EditorSuggest<T> {
  app: App;

  /**
   * Checks if the cursor is currently in a position that should trigger this suggest.
   * Return null if the trigger criteria are not met.
   */
  abstract onTrigger(
    cursor: number,
    view: EditorView,
    file: TFile | null,
  ): EditorSuggestTriggerInfo | null;

  /**
   * Fetches the candidate suggestion items matching the trigger query.
   */
  abstract getSuggestions(context: EditorSuggestContext): T[] | Promise<T[]>;

  /**
   * Custom HTML DOM renderer for an item row in the dropdown popup.
   */
  abstract renderSuggestion(value: T, el: HTMLElement): void;

  /**
   * Applies the chosen suggestion to the editor document.
   */
  abstract selectSuggestion(
    value: T,
    evt: MouseEvent | KeyboardEvent | Event,
    view: EditorView,
  ): void;
}
```

> [!NOTE]
> The suggestion driver triggers at **0ms** latency without requiring spaces or delays. Keybindings like `Tab`, `Enter`, `ArrowDown`, `ArrowUp`, and `Escape` are managed automatically by the core driver.

---

## 3. Example: Building an Emoji Suggest Plugin

Here is a complete, production-ready example of an emoji autocompletion plugin triggered by typing `:`:

```ts
import {
  Plugin,
  EditorSuggest,
  type EditorSuggestContext,
  type EditorSuggestTriggerInfo,
} from 'resin';
import type { EditorView } from '@codemirror/view';
import type { TFile } from 'resin';

interface EmojiItem {
  emoji: string;
  name: string;
}

const EMOJIS: EmojiItem[] = [
  { emoji: '🚀', name: 'rocket' },
  { emoji: '✨', name: 'sparkles' },
  { emoji: '💡', name: 'idea' },
  { emoji: '🔥', name: 'fire' },
  { emoji: '📝', name: 'memo' },
  { emoji: '✅', name: 'check' },
];

class EmojiSuggest extends EditorSuggest<EmojiItem> {
  onTrigger(
    cursor: number,
    view: EditorView,
    _file: TFile | null,
  ): EditorSuggestTriggerInfo | null {
    const line = view.state.doc.lineAt(cursor);
    const textBefore = line.text.slice(0, cursor - line.from);

    // Match a colon followed by letters up to the cursor: e.g. ":roc"
    const match = textBefore.match(/:([a-zA-Z0-9_+-]*)$/);
    if (!match) return null;

    const query = match[1];
    return {
      start: cursor - query.length - 1, // Start at the ':'
      end: cursor,
      query,
    };
  }

  getSuggestions(context: EditorSuggestContext): EmojiItem[] {
    const q = context.query.toLowerCase();
    return EMOJIS.filter((item) => item.name.includes(q));
  }

  renderSuggestion(item: EmojiItem, el: HTMLElement): void {
    el.className = 'cm-wikilink-completion-item';
    el.innerHTML = `
      <span style="font-size: 16px; margin-right: 8px;">${item.emoji}</span>
      <span class="cm-wikilink-completion-label">:${item.name}:</span>
    `;
  }

  selectSuggestion(item: EmojiItem, _evt: Event, view: EditorView): void {
    const cursor = view.state.selection.main.head;
    const line = view.state.doc.lineAt(cursor);
    const textBefore = line.text.slice(0, cursor - line.from);
    const match = textBefore.match(/:([a-zA-Z0-9_+-]*)$/);
    if (!match) return;

    const start = cursor - match[1].length - 1; // replace from ':'
    view.dispatch({
      changes: { from: start, to: cursor, insert: item.emoji },
      selection: { anchor: start + item.emoji.length },
    });
  }
}

export default class EmojiPlugin extends Plugin {
  id = 'community.emoji-suggest';
  name = 'Emoji Autocomplete';

  override async onLoad(): Promise<void> {
    // Register the suggest with automatic unregistration on unload!
    this.registerEditorSuggest(new EmojiSuggest(this.app as any));
  }
}
```

> [!TIP]
> When implementing link or note suggestions, always read headings, tags, and aliases from **`this.app.metadataCache.getFileCache(path)`**. It resolves from an in-memory index in 0ms without hitting the disk!

> [!IMPORTANT]
> Always register suggestions using `this.registerEditorSuggest()` in your plugin class. It guarantees the suggestion provider is cleanly unbound from CodeMirror when the user disables your plugin.

# Recipe: React 19 Sidebar View Plugin

This complete recipe demonstrates building a custom sidebar panel with **React 19**, `@phosphor-icons`, and automatic sidebar expansion.

---

## 1. The React Sidebar Component

```tsx
// src/SidebarComponent.tsx
import React, { useState, useEffect } from 'react';
import { useApp } from 'resin';

export function SidebarComponent() {
  const app = useApp();
  const [tags, setTags] = useState<string[]>([]);
  const [activeTag, setActiveTag] = useState<string | null>(null);
  const [files, setFiles] = useState<string[]>([]);

  useEffect(() => {
    // Initial fetch of tags
    setTags(app.metadataCache.getTags());

    // Update tags on metadata change
    const ref = app.metadataCache.on('changed', () => {
      setTags(app.metadataCache.getTags());
    });
    return () => app.metadataCache.offref(ref);
  }, [app]);

  const handleSelectTag = (tag: string) => {
    setActiveTag(tag);
    setFiles(app.metadataCache.getFilesWithTag(tag));
  };

  const handleOpenFile = (path: string) => {
    app.workspace.openFile(path);
  };

  return (
    <div
      style={{ padding: 12, display: 'flex', flexDirection: 'column', gap: 12 }}
    >
      <h3>Tag Explorer</h3>
      <div style={{ display: 'flex', flexWrap: 'wrap', gap: 6 }}>
        {tags.map((tag) => (
          <button
            key={tag}
            onClick={() => handleSelectTag(tag)}
            style={{
              padding: '4px 8px',
              borderRadius: 4,
              background:
                activeTag === tag
                  ? 'var(--color-accent)'
                  : 'var(--bg-secondary)',
              color: 'var(--text-normal)',
              cursor: 'pointer',
            }}
          >
            {tag}
          </button>
        ))}
      </div>

      {activeTag && (
        <div style={{ display: 'flex', flexDirection: 'column', gap: 4 }}>
          <h4>
            Notes with {activeTag} ({files.length}):
          </h4>
          {files.map((file) => (
            <div
              key={file}
              onClick={() => handleOpenFile(file)}
              style={{
                padding: '6px 8px',
                borderRadius: 4,
                cursor: 'pointer',
                background: 'var(--bg-primary)',
              }}
            >
              📄 {file}
            </div>
          ))}
        </div>
      )}
    </div>
  );
}
```

---

## 2. The View Wrapper

```tsx
// src/TagExplorerView.tsx
import React from 'react';
import { View, type WorkspaceLeaf } from 'resin';
import { SidebarComponent } from './SidebarComponent';

export class TagExplorerView extends View {
  constructor(leaf: WorkspaceLeaf) {
    super(leaf);
  }

  getViewType(): string {
    return 'tag-explorer';
  }

  getDisplayText(): string {
    return 'Tags';
  }

  override getIcon(): string {
    return 'TagIcon';
  }

  override async onLoad(): Promise<void> {
    // Mounts React component tree with AppContext and auto-cleanup
    this.renderReact(<SidebarComponent />);
  }
}
```

---

## 3. The Plugin Entry Point

```ts
// src/index.ts
import { Plugin } from 'resin';
import { TagExplorerView } from './TagExplorerView';

export default class TagExplorerPlugin extends Plugin {
  id = 'community.tag-explorer';
  name = 'Tag Explorer';

  override async onLoad(): Promise<void> {
    // 1. Register view
    this.registerView('tag-explorer', (leaf) => new TagExplorerView(leaf), {
      icon: 'TagIcon',
      title: 'Tags',
    });

    // 2. Ribbon button: reveals view and auto-expands left sidebar if collapsed!
    this.addRibbonIcon('TagIcon', 'Tags', () => {
      this.app.workspace.revealView('tag-explorer', 'left');
    });
  }
}
```

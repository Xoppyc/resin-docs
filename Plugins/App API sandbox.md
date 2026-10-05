# The AppAPI Sandbox

In Resin, plugins never interact directly with global runtime variables or private kernel members. Instead, every plugin receives a scoped, sandboxed **`AppAPI`** instance upon initialization.

---

## 1. What is `AppAPI`?

`AppAPI` is the bridge between your plugin and Resin's core subsystems:

```ts
export interface AppAPI {
  /** Workspace layout tree (splits, tab groups, leaves, active views) */
  workspace: Workspace;

  /** Central application pub/sub event bus */
  events: Events;

  /** In-memory note metadata, frontmatter, tags, backlinks, and link graph */
  metadataCache: MetadataCache;

  /** Core settings and plugin settings tab manager */
  settings: SettingsManager;

  /** Decoupled context menu provider registry */
  contextMenu: ContextMenuRegistry;

  /** Central toast notification manager */
  toast: ToastManager;

  /** Inverted top notification banner manager */
  banners: BannerManager;

  /** Layered modal dialog and alert/prompt manager */
  dialogs: DialogManager;

  /** Bottom status bar manager */
  statusBar: StatusBarManager;

  /** Isolated persistent JSON storage scoped to `.resin/plugins/<pluginId>/data.json` */
  storage: {
    get<T>(): Promise<T | null>;
    set<T>(data: T): Promise<void>;
  };

  /** UI registration methods for views, sidebars, blocks, ribbon items, and settings */
  ui: {
    registerView(
      type: string,
      creator: ViewCreator,
      meta?: { icon: string; title: string },
    ): void;
    registerSidebarPanel(id: string, creator: () => HTMLElement): void;
    registerRibbonItem(item: RibbonItemDef): void;
    registerContextMenuItem<T = any>(
      scope: string,
      provider: MenuItemProvider<T>,
    ): () => void;
    registerSettingTab(tab: PluginSettingTab): void;
    registerCodeBlockHandler(
      language: string,
      handler: CodeBlockHandler,
    ): () => void;
    registerEditorSuggest(suggest: EditorSuggest<any>): () => void;
  };

  /** Direct helper for registering an editor suggest provider */
  registerEditorSuggest(suggest: EditorSuggest<any>): () => void;

  /** Namespaced command registration and dispatch */
  commands: {
    register(
      id: string,
      label: string,
      handler: () => void,
      shortcut?: string,
    ): void;
    execute(id: string): void;
    list(): CommandDefinition[];
  };

  /** Virtual File System (VFS) and disk I/O */
  vault: {
    readonly adapter: DataAdapter;
    read(path: string): Promise<string>;
    write(path: string, content: string): Promise<void>;
    create(path: string, data: string): Promise<TFile>;
    createFolder(path: string): Promise<TFolder>;
    duplicate(path: string): Promise<TFile | TFolder>;
    rename(oldPath: string, newPath: string): Promise<void>;
    trash(path: string): Promise<void>;
    remove(path: string): Promise<void>;
    list(path?: string): Promise<string[]>;
    getRoot(): string;
    getName(): string;
    getNextAvailablePath(
      baseName?: string,
      ext?: string,
      folderPath?: string,
    ): string;
    resolveWikilink(linkText: string, sourcePath?: string): WikilinkResolution;
    getBacklinks(path: string): Promise<LinkRecord[]>;
    getAbstractFileByPath(path: string): TAbstractFile | null;
    getFileByPath(path: string): TFile | null;
    getFolderByPath(path: string): TFolder | null;
    getFiles(): TFile[];
    getMarkdownFiles(): TFile[];
    getAllLoadedFiles(): TAbstractFile[];
    readonly rootFolder: TFolder;
    on(event: string, handler: (payload: any) => void): EventRef;
    off(event: string, handler: (payload: any) => void): void;
    offref(ref: EventRef): void;
  };
}
```

---

## 2. Scoped Namespacing & Storage Isolation

When your plugin registers commands or writes storage via `this.app`:

- **Commands**: Automatically prefixed with `${pluginId}:` (e.g. `community.kanban:toggle-board`), preventing command collisions between plugins.
- **Storage**: Automatically mapped to `<vault>/.resin/plugins/<pluginId>/data.json`. No plugin can accidentally overwrite or corrupt another plugin's configuration.

> [!TIP]
> Always prefer registering UI extensions, context menus, and editor suggestions through `Plugin` methods (e.g. `this.registerEditorSuggest()`) rather than calling raw kernel managers directly. The `Plugin` base class ties their lifecycles to your plugin so they are destroyed cleanly on unload.

> [!NOTE]
> `AppAPI` deliberately omits destructive OS primitives and direct Node/Tauri IPC handles. All filesystem access passes through `app.vault`, ensuring proper in-memory virtual tree updates, `MetadataCache` synchronization, and typed event broadcasting.

---

## 3. Next Steps

- Learn how to interact with files using **[[Vault and VFS|Vault & In-Memory VFS]]**.
- Query note tags and relationships using **[[Metadata cache|MetadataCache & Link Graph]]**.
- Build custom autocompletions with **[[Editor suggestions|Editor Suggestions]]**.

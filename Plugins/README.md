# Developer and Plugin API

Welcome to the Resin plugin documentation. Resin supports TypeScript plugins with hot reloading, an isolated VFS, declarative settings, and embedded React 19 support.

## Table of Contents

### Getting Started

- **[[Plugin quickstart]]**: Build, test, and run your first plugin in under 5 minutes.
- **[[Plugin anatomy]]**: Lifecycle hooks (`onLoad`, `onUnload`), manifests, and auto-cleanup.
- **[[Use React in your plugin]]**: Use React 19 without bundle overhead or hook collisions.

### User Interface and Editor

- **[[Ribbon and commands]]**: Add ribbon actions and command palette entries.
- **[[Custom views]]**: Mount custom views into workspace leaves.
- **[[Code blocks]]**: Custom code block widgets with interactive controls and event handling.
- **[[Editor suggestions]]**: Custom autocomplete providers in CodeMirror 6.
- **[[Status bar]]**: Global and view-context status bar indicators.
- **[[Dialogs and toasts]]**: Notifications, banners, confirmations, and modals.
- **[[Context menus]]**: Extend right-click menus across the app.
- **[[Workspace and leaves]]**: Splits, sidebars, tabs, and layout tree management.

### Storage, Data, and Network

- **[[Vault and VFS]]**: Read, write, create, and manage files through the Virtual File System.
- **[[Metadata cache]]**: Query frontmatter, tags, headings, and backlinks in 0ms.
- **[[Persistent storage]]**: Isolated JSON storage in `.resin/plugins/<pluginId>/data.json`.
- **[[Settings tab DSL]]**: Fluent settings tabs with toggles, inputs, and sliders.
- **[[Safe HTTP requests]]**: Make external API calls through the SSRF-guarded Rust gateway.
- **[[Events and lifecycle]]**: Central event bus with automatic listener cleanup.

---

- User guides: **[[General/README|General User Guide]]**
- Custom styles: **[[Themes/README|Themes and Appearance]]**


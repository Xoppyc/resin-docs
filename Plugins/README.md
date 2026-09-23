# Developer and Plugin API (Plugins)

Welcome to the **Plugins** documentation space. This section contains the complete developer reference for building extensions, custom views, status bar items, context menus, and storage integrations in Resin. It's worth noting that as of v 0.3.5 custom plugins are not supported so this docs section is for the future only.

---

## Table of Contents

### 1. Getting Started

- **[[Plugin quickstart]]**: Build, test, and run your first plugin in under 5 minutes.
- **[[Plugin anatomy]]**: Lifecycle hooks (`onLoad`, `onUnload`), manifest configuration, and cleanup tracking.

### 2. Core Architecture

- **[[App API sandbox]]**: The sandboxed API boundary and scoped access model.
- **[[Vault and VFS]]**: Reading, writing, modifying, and streaming files through the virtual file system.
- **[[Metadata cache]]**: Querying parsed frontmatter, headings, links, and tags.
- **[[Workspace and leaves]]**: The hierarchical workspace layout tree, splits, tabs, and leaves.
- **[[Events and lifecycle]]**: Vault events, workspace state transitions, and auto-cleanup registration.

### 3. UI and Components

- **[[Custom views]]**: Registering custom leaf views in the workspace.
- **[[Framework helpers]]**: Mounting React, Svelte, Vue, or Vanilla JS components into leaves.
- **[[Context menus]]**: Registering scoped context menu items and submenus.
- **[[Status bar]]**: Adding persistent or dynamic items to the bottom status bar.
- **[[Dialogs and toasts]]**: Spawning modal dialogs, confirmations, and toast notifications.
- **[[Ribbon and commands]]**: Registering command palette actions and left ribbon icons.
- **[[Editor suggestions]]**: Custom auto-complete suggestions inside CodeMirror 6.
- **[[Developer tools]]**: Inspecting elements, debugging plugins, and live console inspection.

### 4. Storage, Network, and Settings

- **[[Safe HTTP requests]]**: Making external network requests safely through the SSRF-guarded Rust gateway.
- **[[Persistent storage]]**: Isolated JSON data storage in `.resin/plugins/<pluginId>/data.json`.
- **[[Settings tab DSL]]**: Declarative settings tabs with switches, text inputs, dropdowns, and buttons.

### 5. Recipes and Examples

- **[[Example sidebar React]]**: A complete React-based sidebar panel plugin.
- **[[Example canvas Svelte]]**: An interactive canvas view built with Svelte.
- **[[Example full plugin]]**: An end-to-end production plugin example.

---

- Looking for user guides? Head over to the **[[General/README|General User Guide]]**.
- Looking to customize styles? Head over to **[[Themes/README|Themes & Snippets (Themes Space)]]**.

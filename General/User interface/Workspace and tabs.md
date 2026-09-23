# Workspace and Tabs

Resin lets you arrange your notes into tabs, split panes, and collapsible sidebars. Workspace layouts persist automatically so your exact configuration reloads when you reopen the app.

---

## Tab Management

Each pane contains its own tab bar for managing open notes:

- **Switch tabs**: Click any tab header, or press `Ctrl+Tab` and `Ctrl+Shift+Tab` to cycle forward and backward.
- **Close tabs**: Click the `×` button on the tab, middle-click the tab header, or press `Ctrl+W`.
- **Reorder tabs**: Drag any tab header left or right to reposition it.
- **Pin and split actions**: Right-click any tab header to access the [[Context menus|tab context menu]].

---

## Split Panes

You can divide the main workspace into multiple editor panes to view and edit notes side by side:

- Right-click an open tab and select **Split Right** to open the note in a new vertical split.
- Select **Split Down** to open the note in a new horizontal split.
- Each split pane maintains its own tab list, active document, and scroll position.
- Close all tabs in a split to remove that pane and return space to adjacent panes.

---

## Sidebars and Ribbon

The outer edges of the window contain navigation and tool toggles:

- **Left ribbon**: Provides quick buttons for toggling the sidebar, opening the [[Command palette]], launching [[Daily notes]], and accessing **Settings** (`Ctrl+,`).
- **File explorer sidebar**: The collapsible panel holding your vault tree. Click the folder icon in the ribbon to show or hide the sidebar.
- **Hiding the ribbon**: If you prefer a clean writing surface without side icons, disable the ribbon under **Settings → Appearance**. You can still access Settings with `Ctrl+,` and commands with `Ctrl+P`.

---

## Status Bar

The status bar sits at the bottom edge of the window and provides contextual info about the active editor:

- **Cursor position**: Displays current line and column numbers.
- **Mode toggle**: Shows whether the editor is in **Live Preview** or **Source Mode**. Click the label or press `Ctrl+Shift+E` to toggle between modes.
- **Word and character counts**: Updates in real time as you write.
- **File encoding and syntax**: Shows file format (`Markdown`) and encoding (`UTF-8`).

---

## Workspace Layout Persistence

Every change you make to your layout—including split ratios, open tabs, and sidebar visibility—saves immediately to `<vault>/.resin/workspace.json`. When you restart Resin, the workspace restores exactly where you left off.

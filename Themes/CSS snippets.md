# CSS Snippets

CSS snippets are small `.css` files that let you tweak the interface without creating a full theme. They live directly in your vault and can be toggled on or off individually.

![[css-snippets-settings.png|CSS snippets manager in Appearance Settings]]

## How to Add a Snippet

1. Open **Settings** (`Ctrl+,` or `Cmd+,`) -> **Appearance**.
2. Scroll to **CSS Snippets**.
3. Click the folder icon to open `.resin/snippets/` in your system file explorer.
4. Save your `.css` file in that folder.
5. In Resin, click the reload button and toggle the snippet on.

Resin reloads styles automatically when you save changes in your code editor.

## Inspecting with DevTools

Press `Ctrl+Shift+I` (or `F12`) to open Developer Tools. Click the element inspector icon in the top-left to select any UI component and view its CSS classes and variables.

## Examples

### 1. Floating Rounded Tabs (`floating-tabs.css`)

Makes tabs appear as floating pill containers:

![[floating-tabs-snippet-result.png|Tabs styled as floating pills via CSS snippet]]

```css
.tab {
  border-radius: 6px;
  border: 1px solid var(--background-modifier-border);
  box-shadow: var(--shadow-sm);
  margin: 2px 3px;
  transition: all 0.15s ease;
}

.tab.mod-active {
  border: 1px solid transparent;
  outline: 1px solid var(--accent-color);
  outline-offset: -1px;
  background-color: var(--background-primary);
}

.tab-bar {
  padding: 4px 8px;
  height: auto;
}
```

### 2. Rainbow Markdown Headings (`rainbow-headings.css`)

```css
:root {
  --h1-color: #f7768e;
  --h2-color: #ff9e64;
  --h3-color: #e0af68;
  --h4-color: #9ece6a;
  --h5-color: #7aa2f7;
  --h6-color: #bb9af7;
}

.cmt-heading-1 { color: var(--h1-color) !important; font-weight: 700; }
.cmt-heading-2 { color: var(--h2-color) !important; }
.cmt-heading-3 { color: var(--h3-color) !important; }
.cmt-heading-4 { color: var(--h4-color) !important; }
.cmt-heading-5 { color: var(--h5-color) !important; }
.cmt-heading-6 { color: var(--h6-color) !important; }
```

### 3. Hide Ribbon (`zen-mode.css`)

```css
.ribbon {
  display: none !important;
}
```


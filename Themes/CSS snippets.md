# CSS Snippets

CSS snippets are small `.css` files that let you tweak the Resin interface without building a full theme. They live right in your vault and you can turn them on or off whenever you want.

---

## Why Snippets Are Cool

You don't need to fork a theme or write a plugin just to change a border radius or hide a button.

Snippets inject at the very end of the stylesheet cascade. That means whatever CSS you write here wins over Resin's defaults and whatever theme you have active. If an update changes something small, you can fix it yourself in 10 seconds.

---

## How to Add a Snippet

1. Open **Settings** (`Ctrl+,` or `Cmd+,`).
2. Head to the **Appearance** tab.
3. Scroll down to **CSS Snippets**.
4. Click the folder icon to open `.resin/snippets/` in your file manager.
5. Drop a `.css` file in there (or create a new one).
6. Back in Resin, hit the **Reload** button next to snippets.
7. Flip the toggle switch to turn it on.

You don't have to restart the app. When you edit and save the snippet in your favorite text editor, Resin picks up the change immediately.

---

## Inspecting the UI with DevTools

I added built-in Developer Tools in v0.2.5 so you don't have to guess class names.

Press `Ctrl+Shift+I` (or `F12`) inside Resin.

Click the element picker icon in the top-left of DevTools and click on whatever part of the UI you want to restyle—a tab, the sidebar, a button, or a callout. The Styles panel shows you every CSS variable and class currently attached to that element.

---

## Examples You Can Use Right Now

Here are some real snippets people use to tweak Resin:

### 1. Floating Rounded Tabs (`floating-tabs.css`)

Makes your open note tabs look like modern floating pills instead of a flat connected bar:

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
  max-height: none;
}
```

### 2. Rainbow Markdown Headings (`rainbow-headings.css`)

Colors your `#` headers so you can visually parse long notes at a glance:

```css
:root {
  --h1-color: #f7768e;
  --h2-color: #ff9e64;
  --h3-color: #e0af68;
  --h4-color: #9ece6a;
  --h5-color: #7aa2f7;
  --h6-color: #bb9af7;
}

.cmt-heading-1 {
  color: var(--h1-color) !important;
  font-weight: 700;
}
.cmt-heading-2 {
  color: var(--h2-color) !important;
}
.cmt-heading-3 {
  color: var(--h3-color) !important;
}
.cmt-heading-4 {
  color: var(--h4-color) !important;
}
.cmt-heading-5 {
  color: var(--h5-color) !important;
}
.cmt-heading-6 {
  color: var(--h6-color) !important;
}
```

### 3. Hide Ribbon for Pure Minimalism (`zen-mode.css`)

If you prefer using keyboard shortcuts (`Ctrl+P`, `Ctrl+,`) and don't need the left vertical ribbon taking up horizontal space:

```css
.ribbon {
  display: none !important;
}
```

### 4. Custom Accent Glow on Active Line (`active-line-glow.css`)

Adds a subtle accent indicator next to whatever line your cursor is currently on:

```css
.cm-activeLine {
  background-color: var(--background-modifier-hover);
  border-left: 2px solid var(--accent-color);
  padding-left: 6px !important;
}
```

---

## Best Practices

- **Use CSS variables when you can**: Before writing custom hex codes, check `var(--background-primary)`, `var(--text-normal)`, `var(--accent-color)`, etc. That way, if you switch between Dark and Light mode, your snippet won't look broken.
- **Keep snippets small and focused**: Make one file for tabs, one for typography, and one for editor tweaks. It's much easier to turn one off when debugging than untangling a giant 500-line stylesheet.

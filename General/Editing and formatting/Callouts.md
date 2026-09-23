# Callouts Guide

Callouts let you highlight side notes, warnings, quotes, tips, and important information inside your markdown documents.

---

## Basic Syntax

A callout starts with a blockquote arrow (`>`), followed by `[!type]`, an optional title, and indented body text:

> [!note] My Callout Title
> This is the body of the callout. You can use **bold**, *italics*, and `code` here too.

If you don't provide a custom title, Resin automatically capitalizes the callout type as the title (e.g. `[!warning]` displays as **Warning**).

---

## Collapsible Callouts

You can make any callout collapsible by adding a plus (`+`) or minus (`-`) immediately after the type name:

- `> [!tip]+ Expanded by default`: Clicking the chevron on the right collapses the callout.
- `> [!tip]- Collapsed by default`: Starts hidden until clicked.

> [!faq]- How do I rebind hotkeys?
> Go to Settings (`Ctrl+,`) → Hotkeys, find the command, and press your preferred keys.

---

## All 34 Built-In Callout Types

Resin supports 34 distinct callout types grouped by visual intent and color palette:

| Group | Types | Color Token |
| :--- | :--- | :--- |
| **Primary & Info** | `note`, `info`, `bookmark` | `--color-blue` |
| **Action & Tasks** | `todo`, `tip`, `hint`, `important` | `--accent-color` / `--color-orange` |
| **Success & Resolution** | `success`, `check`, `done`, `answer` | `--color-green` |
| **Questions & Help** | `help`, `faq`, `question` | `--color-yellow` |
| **Warnings & Caution** | `warning`, `caution`, `attention` | `--color-orange` |
| **Danger & Errors** | `danger`, `error`, `bug`, `failure`, `fail`, `missing` | `--color-red` |
| **Examples & Summaries** | `example`, `abstract`, `summary`, `tldr` | `--color-purple` |
| **Favorites & Ideas** | `favorite`, `heart`, `love`, `pin`, `idea` | `--color-pink` / `--color-yellow` |
| **Quotes** | `quote`, `cite` | `--text-muted` |

---

## Customizing Callouts with CSS Snippets

Every callout has a `data-callout="[type]"` attribute on its container. You can tweak existing callouts or add your own custom callout type using a CSS snippet in `.resin/snippets/`:

```css
/* Custom "coffee" callout */
.cm-callout[data-callout="coffee"] {
  --callout-color: #a07855;
  border-left: 3px solid var(--callout-color);
  background-color: rgba(160, 120, 85, 0.08);
}
```

Now you can type:

```markdown
> [!coffee] Morning Fuel
> Remember to drink some water too!
```


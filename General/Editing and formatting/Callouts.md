# Callouts

Callouts highlight side notes, warnings, tips, and quotes inside your notes without interrupting text flow.

![[rendered-callouts-examples.png|Rendered callout types in Live Preview]]

## Basic Syntax

A callout starts with a blockquote marker (`>`), followed by `[!type]`, an optional title, and body text:

```markdown
> [!note] Callout title
> Callout body content. Supports **markdown**, [[Internal links|wikilinks]], and `code`.
```

If you omit the title, Resin capitalizes the type name as the title (for example, `[!warning]` displays as **Warning**).

## Foldable Callouts

Make any callout foldable by adding a plus (`+`) or minus (`-`) directly after the type:

- `> [!tip]+ Expanded by default`: Starts open; clicking the chevron collapses it.
- `> [!tip]- Collapsed by default`: Starts closed; clicking the header reveals content.

![[collapsible-callout-interaction.png|Collapsing and expanding a callout in the editor]]

## Supported Types

Resin supports 34 built-in callout types:

| Category | Types |
| :--- | :--- |
| **Info** | `note`, `info`, `bookmark` |
| **Actions** | `todo`, `tip`, `hint`, `important` |
| **Success** | `success`, `check`, `done`, `answer` |
| **Questions** | `help`, `faq`, `question` |
| **Warnings** | `warning`, `caution`, `attention` |
| **Danger** | `danger`, `error`, `bug`, `failure`, `fail`, `missing` |
| **Summaries** | `example`, `abstract`, `summary`, `tldr` |
| **Favorites** | `favorite`, `heart`, `love`, `pin`, `idea` |
| **Quotes** | `quote`, `cite` |

## Custom Callouts

Every callout container includes a `data-callout="[type]"` attribute. You can style existing types or define custom types with a CSS snippet:

```css
/* Custom "coffee" callout */
.cm-callout[data-callout="coffee"] {
  --callout-color: #a07855;
  border-left: 3px solid var(--callout-color);
  background-color: rgba(160, 120, 85, 0.08);
}
```

```markdown
> [!coffee] Morning Fuel
> Remember to drink some water too.
```



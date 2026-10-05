# Editing and Formatting

Resin uses standard CommonMark and GitHub Flavored Markdown (GFM) with real-time Live Preview syntax collapsing.

![[live-preview-formatting.png|Live preview text formatting and delimiter expansion]]

## Text Formatting

Standard Markdown formatting applies directly:

- `**bold**` or `__bold__` renders as **bold**
- `*italic*` or `_italic_` renders as *italic*
- `~~strikethrough~~` renders as ~~strikethrough~~
- `` `inline code` `` renders as `inline code`
- `==highlight==` renders as ==highlight==

When your cursor moves inside formatted text, the markdown delimiters expand so you can edit them directly. When the cursor leaves, the delimiters collapse back into styled text.

## Auto-Pairing and Smart Skip

Whenever you type an opening delimiter (`[`, `(`, `{`, `"`, `'`, `` ` ``), Resin inserts the matching closing character:

- **Smart Skip**: Your cursor sits between the pair. If you type the closing character naturally, the cursor steps over it instead of inserting a duplicate.
- **Navigation awareness**: If you navigate away with arrow keys or a mouse click, Resin clears the auto-pair buffer so normal typing resumes.
- **Code fence expansion**: Typing ```` ``` ```` and pressing `Enter` expands into a closed code block with your cursor positioned inside.

## Lists

- **Unordered lists**: Type `- ` or `* ` followed by text.
- **Ordered lists**: Type `1. ` followed by text.
- **Hanging indent**: Wrapped list lines align with the text of the first line, keeping bullets visually isolated.
- **Indentation**: Press `Tab` to indent sub-items, and `Shift+Tab` to outdent.

## LaTeX Math

Resin renders LaTeX expressions using KaTeX:

- **Inline math**: Wrap expressions in single dollar signs: `$E = mc^2$`
- **Block math**: Wrap multiline expressions in double dollar signs:

```markdown
$$
\frac{-b \pm \sqrt{b^2 - 4ac}}{2a}
$$
```

![[latex-math-rendering.png|Rendered KaTeX math equation in Live Preview]]

## Tables

Type standard GFM markdown tables:

```markdown
| Feature | Status |
| :--- | :--- |
| Callouts | Supported |
| Tables | Supported |
```

![[markdown-tables.png|Rendered Markdown table in Live Preview]]

Tables scale with your configured editor font size, and rows render without cursor jitter when editing adjacent lines.



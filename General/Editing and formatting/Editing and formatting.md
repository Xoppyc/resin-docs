# Editing & Formatting

Resin uses standard CommonMark and GitHub Flavored Markdown (GFM) with real-time Live Preview mark collapsing.

---

## 1. Text Formatting

Standard formatting works just like you expect:

- `**bold**` or `__bold__` renders as **bold**
- `*italic*` or `_italic_` renders as *italic*
- `~~strikethrough~~` renders as ~~strikethrough~~
- `` `inline code` `` renders as `inline code`
- `==highlight==` renders as highlight

When your cursor moves inside a word, the syntax marks expand so you can edit the delimiters. When your cursor moves away, the marks smoothly collapse.

---

## 2. Auto-Pairing & Context-Based Auto Skip

Whenever you type an opening bracket or delimiter (`[`, `(`, `{`, `"`, `'`, `` ` ``), Resin automatically inserts the matching closing character:

- **Smart Skip**: When a character is auto-inserted, your cursor stays inside. If you type the closing character naturally on your keyboard, the cursor jumps over it instead of inserting a duplicate.
- **Manual Navigation Awareness**: If you manually navigate before the closing character with arrows or mouse, Resin clears the skip state so you can type normally.
- **Closing Fence Expansion**: Typing ```` ``` ```` and pressing `Enter` automatically creates a full closed code fence block with cursor positioned on the empty line inside.

---

## 3. Lists

- **Unordered Lists**: Type `- ` or `* ` followed by text.
- **Ordered Lists**: Type `1. ` followed by text.
- **Hanging Indentation**: In v0.3, if a list item wraps across multiple lines, the wrapped lines align with the text of the first line, never wrapping under the bullet or number.
- **Noticeable Tabs**: Indenting sub-items with `Tab` provides clear, readable tab spacing.

---

## 4. LaTeX Math

Resin supports full LaTeX mathematical expressions powered by KaTeX:

- **Inline Math**: Wrap expressions in single dollar signs: `$E = mc^2$`
- **Block Math**: Wrap multiline expressions in double dollar signs:

```markdown
$$
\frac{-b \pm \sqrt{b^2 - 4ac}}{2a}
$$
```

---

## 5. Tables

Type standard GFM markdown tables:

```markdown
| Feature | Status |
| :--- | :--- |
| Callouts | Supported |
| Tables | Experimental |
```

In v0.3, tables scale smoothly with your chosen editor font size, and the line immediately following a table is fully editable without triggering parser jitter.


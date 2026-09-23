# Command Palette

The Command Palette is a core plugin that lets you search and trigger any command in Resin directly from the keyboard. It indexes built-in editor actions, workspace split controls, and commands contributed by installed plugins.

---

## Opening the Command Palette

To open the Command Palette, use either of the following:

- Press `Ctrl+P` (or `Cmd+P` on macOS).
- Click the terminal icon in the left ribbon.

---

## Running Commands

1. Open the palette with `Ctrl+P`.
2. Start typing to filter commands.
3. Use the `↑` and `↓` arrow keys to move through the results.
4. Press `Enter` to run the selected command.

To close the palette without executing a command, press `Esc` or click outside the modal.

> [!tip]
> The search bar includes a clear button (`✕`) that appears whenever you have entered text. Click it or press `Ctrl+A` followed by `Backspace` to reset your query.

---

## Fuzzy Search

The Command Palette uses a fuzzy matching algorithm that weights word boundaries and consecutive character matches. You do not need to type exact command names. For example:

- Typing `split` highlights **Split Right** and **Split Down**.
- Typing `daily` finds **Open today's daily note**.
- Typing `theme` lists options from the Theme Switcher.

Matches are ranked so the closest match stays at the top of the list.

---

## Shortcut Badges

Commands that have dedicated hotkeys display their key combination pill on the right side of the list item. This helps you discover shortcuts for commands you use often so you can run them directly in the future.

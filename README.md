# zpaced

A [Spacemacs](https://www.spacemacs.org/) / [VSpaceCode](https://vspacecode.github.io/)-style
**space leader** keymap for the [Zed](https://zed.dev) editor.

It maps Zed's actions onto the familiar `SPC`-prefixed mnemonic menus
(`SPC f` for files, `SPC g` for git, `SPC w` for windows, and so on), so if
your fingers already know Spacemacs or VSpaceCode, they will feel at home in Zed.

Where Spacemacs and VSpaceCode disagree, **VSpaceCode's layout is preferred**.

## Requirements

- Zed with **Vim mode enabled**. In your `settings.json`:

  ```json
  { "vim_mode": true }
  ```

  The leader key is `space`, active in normal/visual mode **and on non-editor
  screens** (welcome, extensions, empty panes, settings UI, etc.). It never
  interferes with typing a literal space while inserting text.

## Installation

Copy `keymap.json` into your Zed keymap location:

| Platform      | Path                                   |
| ------------- | -------------------------------------- |
| macOS / Linux | `~/.config/zed/keymap.json`            |
| Windows       | `~\AppData\Roaming\Zed\keymap.json`    |

If you already have a `keymap.json`, merge the array entries rather than
overwriting the file.

> Tip: a which-key style popup makes the menus discoverable, but every binding
> works without one.

## What's inside

The bindings mirror VSpaceCode's default menu tree. Top-level groups:

| Prefix  | Group                         |
| ------- | ----------------------------- |
| `SPC :` | Tasks                         |
| `SPC b` | Buffers                       |
| `SPC c` | Compile / Comments            |
| `SPC d` | Debug                         |
| `SPC e` | Errors                        |
| `SPC f` | Files                         |
| `SPC g` | Git                           |
| `SPC h` | Help                          |
| `SPC i` | Insert                        |
| `SPC j` | Jump / Join / Split           |
| `SPC l` | Layouts                       |
| `SPC p` | Project                       |
| `SPC q` | Quit                          |
| `SPC r` | Resume / Repeat               |
| `SPC s` | Search / Symbol               |
| `SPC t` | Toggles                       |
| `SPC w` | Windows                       |
| `SPC x` | Text                          |
| `SPC z` | Zoom / Fold                   |
| `SPC D` | Diff / Compare                |
| `SPC F` | Frame                         |
| `SPC S` | Show (panels)                 |
| `SPC T` | UI toggles                    |

### Zed-specific extras (not in VSpaceCode)

- **`SPC a` — AI / Agent**: toggle the agent panel, new thread, inline assist,
  model/profile selectors, and edit prediction.
- **`SPC S`** additionally exposes Zed's agent, collaboration, and outline panels.
- **`SPC z . 1`–`9`** — fold at indentation level, via `editor::FoldAtLevel_N`.
- **`alt-h/j/k/l`** — dock-aware focus movement between the editor and every
  panel (project, git, debug, collab, outline, agent, terminal). This is a fast,
  mouse-free complement to `SPC w h/j/k/l`, which only works inside vim contexts.
  It preserves edit-prediction accepts and git commit-message generation.

### Keymap structure

The keymap is split into three context blocks to keep every binding DRY:

| Block | Context | Purpose |
| ----- | ------- | ------- |
| 1 (Shared) | `(VimControl && !menu && vim_mode != insert) \|\| (!Editor && !Terminal && !Dock && !menu)` | Workspace-level bindings — files, projects, git, windows, panels, UI toggles, debug, tasks, help. Active everywhere. |
| 2 (Editor) | `VimControl && !menu && vim_mode != insert` | Editor-specific bindings — text transforms, folds, diff, insert, errors, copy-path, comments. Active only in vim normal/visual mode. |
| 3 (Dock nav) | `(Editor && !edit_prediction) \|\| ProjectPanel \|\| ...` | `alt-h/j/k/l` pane navigation across docks and panels. |

This means `SPC f f`, `SPC p p`, `SPC S`, and all workspace-level commands work
from the welcome screen, extensions, and any empty pane — no editor needed.

## Conventions in the keymap comments

Because Zed and VSpaceCode do not have a 1:1 feature mapping, each binding is
annotated:

- **Exact match** — the direct Zed action, no annotation.
- **`// APPROX: <reason>`** — the closest available Zed action, with a note on
  how it differs from the VSpaceCode behavior.
- **`// HACK: <reason>`** — a fragile workaround (e.g. chained keystrokes).
- **`null`** — no suitable Zed action exists; the key is intentionally left
  unbound, with a comment explaining why.

## Known gaps

Some VSpaceCode features have no Zed equivalent and are left as `null`, e.g.
merge-conflict accept actions, editor column layouts, "detect indentation",
render-whitespace toggle, and language-specific major-mode menus (`SPC m`).
Build/test/compile tasks are inherently project-specific — they open Zed's task
picker (`task::Spawn`); define real commands in a `.zed/tasks.json`.

Clipboard-path variants under `SPC f y` are approximations: Zed offers
`CopyPath` / `CopyRelativePath` / `CopyFileLocation` but has no path-with-column
variant, so those keys copy the closest available form (see the comments).

## Contributing

Improvements welcome — especially replacing `APPROX`/`HACK`/`null` entries with
better Zed actions as the editor gains features. Please keep the comment
annotations accurate to the actual behavior.

## License

MIT

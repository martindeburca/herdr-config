# herdr-config

My Herdr configuration — a terminal multiplexer I use as a tmux replacement.
The keys mirror my tmux setup (prefix `Ctrl+a`, `|` / `-` splits, `Ctrl+h/j/k/l` to move between
panes), so the muscle memory carries over.

This is a copy of the `config/herdr` folder in my private configs repo, kept public so I can set
Herdr up on a machine that can't reach that repo.

## Files

- `config.toml` — the shared config (macOS, and the base for Windows): keybindings and the
  `Ctrl+h/j/k/l` pane/Neovim navigation.
- `windows.toml` — Windows-only additions, merged on top of `config.toml` (see below). Currently
  just sets `cmd.exe` as the pane shell, since Herdr defaults to PowerShell on Windows.

> The four `[[keys.command]]` blocks for `Ctrl+h/j/k/l` use the `jsheely.vim-navigation` Herdr
> plugin (a vim-tmux-navigator equivalent). Install that plugin for them to work, or delete those
> four blocks if you don't want it — the rest of the config is independent.

## macOS / Linux

Herdr reads `~/.config/herdr/config.toml`. Symlink or copy `config.toml` there:

```sh
mkdir -p ~/.config/herdr
ln -sf "$PWD/config.toml" ~/.config/herdr/config.toml   # or: cp config.toml ~/.config/herdr/
```

Reload after changes with prefix `r`, or `herdr server reload-config`. Check with
`herdr config check`.

## Windows

Herdr reads `%APPDATA%\herdr\config.toml`. Herdr has no include mechanism, so the Windows config
is `config.toml` **plus** the contents of `windows.toml`.

On a machine where PowerShell is available you could script that merge, but it's small enough to do
by hand — which is the only option on a locked-down machine where PowerShell is blocked:

1. Create the folder `%APPDATA%\herdr\` (paste `%APPDATA%\herdr` into the File Explorer address
   bar, or `mkdir "%APPDATA%\herdr"` in `cmd`).
2. Copy `config.toml` into it as `%APPDATA%\herdr\config.toml`.
3. Open that copy and append the Windows-only section from `windows.toml` at the end:

   ```toml
   [terminal]
   default_shell = "cmd.exe"
   ```

4. In Herdr, reload with prefix `r` (or `herdr server reload-config`), and verify with
   `herdr config check`.

That's the whole merge — `windows.toml` only adds the one `[terminal]` section; nothing in the
base config is removed on Windows.

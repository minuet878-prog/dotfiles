# dotfiles

My zsh configuration for an Ubuntu server (hearwell-vps).

## What's included

- **PATH**: adds `~/.local/bin` for user-installed binaries
- **History**: saved to `~/.zsh_history`, written after every command, consecutive duplicates ignored
- **fnm**: Node.js version manager, auto-switches version on `cd` (reads `.node-version` / `.nvmrc`)
- **zoxide**: smarter `cd` — `z <keyword>` jumps by frecency, `zi` picks interactively with fzf
- **Prompt**: `user@host:cwd%`
- **zsh-autosuggestions**: suggests commands from history (accept with →)
- **zsh-syntax-highlighting**: green = valid command, red = not found

## Prerequisites

Install with apt:

```bash
sudo apt install zsh zsh-autosuggestions zsh-syntax-highlighting fzf
```

Install with official scripts (they install to `~/.local`):

```bash
# zoxide
curl -sSfL https://raw.githubusercontent.com/ajeetdsouza/zoxide/main/install.sh | sh

# fnm
curl -fsSL https://fnm.vercel.app/install | bash
```

> fnm's installer appends a config block to the shell rc file. That block already exists in this repo's `.zshrc`, so remove any duplicate it adds.

## Installation

```bash
# 1. Clone
git clone https://github.com/YOUR_USERNAME/dotfiles.git ~/dotfiles

# 2. Back up the existing config, if any
mv ~/.zshrc ~/.zshrc.bak 2>/dev/null

# 3. Symlink (target first, link name second)
ln -s ~/dotfiles/.zshrc ~/.zshrc

# 4. Make zsh the login shell, then log out and back in
chsh -s "$(which zsh)"
```

Verify:

```bash
ls -l ~/.zshrc   # should show -> /home/<user>/dotfiles/.zshrc
type z           # should print "z is a shell function"
```

> Keep your current SSH session open while testing the new shell in a second session. If something breaks, revert with `chsh -s /bin/bash`.

## Notes

- **Why PATH is set in `.zshrc`**: on Ubuntu, `~/.profile` sources `.bashrc` *before* adding `~/.local/bin` to PATH, and zsh doesn't read `.profile` at all. Without the explicit PATH line, `zoxide init` fails at login.
- **zsh doesn't save history by default**: `HISTFILE` / `HISTSIZE` / `SAVEHIST` must be set, otherwise autosuggestions has nothing to suggest.
- **`zsh-syntax-highlighting` must be sourced last**, per its documentation.
- **Never commit `~/.zsh_history`**: it may contain secrets typed on the command line.
- **Ubuntu package names**: `fd` is installed as `fdfind`, `bat` as `batcat`.
- **Pushing from the server**: this repo uses a repo-scoped deploy key (`~/.ssh/dotfiles_deploy`), set via `git config core.sshCommand`, so the server never holds a key with access to my whole GitHub account.

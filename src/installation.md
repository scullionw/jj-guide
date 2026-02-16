# Installation & Setup

## Install jj

```bash
# macOS
brew install jj

# Cargo (any platform)
cargo install --locked jj-cli

# Arch Linux
pacman -S jujutsu
```

Check your version:

```bash
jj version
# jj 0.38.0
```

## Initial Configuration

Set your identity (required):

```bash
jj config set --user user.name "Your Name"
jj config set --user user.email "you@example.com"
```

Your config lives at `~/.jjconfig.toml`. You can also edit it directly:

```bash
jj config edit --user
```

## Starting a Repository

### Clone an existing repo (recommended: colocated)

```bash
jj git clone --colocate https://github.com/org/repo.git
cd repo
```

Colocated mode keeps a `.git` directory alongside `.jj`, so Git tooling (CI, editors, GitHub CLI) all still work.

### Init in an existing Git repo

```bash
cd my-git-repo
jj git init --colocate
```

### Brand new repo

```bash
jj init my-project
cd my-project
```

## Editor Setup

jj respects `$EDITOR` and `$VISUAL`. For VS Code:

```bash
jj config set --user ui.editor "code --wait"
```

For neovim:

```bash
jj config set --user ui.editor "nvim"
```

## Verify It Works

```bash
jj log
```

You should see a log with your working copy commit (`@`) on top.

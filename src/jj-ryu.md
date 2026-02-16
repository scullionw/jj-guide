# jj-ryu: Stacked PRs on GitHub

[jj-ryu](https://github.com/dmmulroy/jj-ryu) bridges the gap between jj's local stacking and GitHub's PR model. Each bookmark in your stack becomes a PR that targets the previous bookmark — exactly like Graphite, but open source and local-first.

## Install

```bash
# npm (prebuilt binaries)
npm install -g jj-ryu

# Cargo
cargo install jj-ryu
```

The binary is called `ryu`.

## Authentication

ryu uses your existing GitHub CLI or environment tokens:

```bash
# Option 1: GitHub CLI (recommended)
gh auth login

# Option 2: Environment variable
export GITHUB_TOKEN=ghp_...

# Verify
ryu auth github test
```

For GitLab, use `glab auth login` or `GITLAB_TOKEN`.

## The Workflow

### 1. Build your stack

```bash
jj new main
# ... work on user model ...
jj commit -m "add user model"
jj bookmark create feat-user -r @-

# ... work on auth ...
jj commit -m "add auth middleware"
jj bookmark create feat-auth -r @-

# ... work on session ...
jj commit -m "add session handling"
jj bookmark create feat-session -r @-
```

### 2. Track bookmarks

```bash
ryu track --all               # track all bookmarks in trunk()..@
# or
ryu track feat-user feat-auth feat-session
```

### 3. View the stack

```bash
ryu
```

Shows your stack with tracking status and PR links.

### 4. Submit as PRs

```bash
ryu submit
```

This creates chained PRs on GitHub:

- `feat-user` → PR targeting `main`
- `feat-auth` → PR targeting `feat-user`
- `feat-session` → PR targeting `feat-auth`

Reviewers see only the diff for each individual change — clean, focused review just like Graphite.

### 5. Update after changes

After making local edits, just submit again:

```bash
# Make changes, then:
ryu submit
```

ryu force-pushes the updated bookmarks and updates PR descriptions.

### 6. After a PR merges

```bash
jj git fetch
ryu sync                      # retargets remaining PRs
```

## Useful Options

```bash
ryu submit --dry-run          # preview without creating PRs
ryu submit --confirm          # preview and prompt before executing
ryu submit --draft            # create PRs as drafts
ryu submit --publish          # mark draft PRs as ready
ryu submit --only feat-auth   # submit one bookmark only
ryu submit --upto feat-auth   # submit stack up to a bookmark
ryu submit --stack            # include all descendant bookmarks
ryu submit --update-only      # update existing PRs, don't create new
ryu submit --select           # interactively choose bookmarks
```

## Tips

- **Name bookmarks clearly.** ryu uses bookmark names as branch names on GitHub.
- **One bookmark per logical change.** Don't bookmark intermediate work-in-progress.
- **Use `--draft` early.** Submit drafts while still iterating, then `--publish` when ready.
- **`ryu sync` after merges.** This keeps your stack properly retargeted.

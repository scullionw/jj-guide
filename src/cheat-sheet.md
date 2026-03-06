# Cheat Sheet

## Daily Commands

| Command | What it doess |
|---|---|
| `jj new` | Start a new change on top of `@` |
| `jj describe -m "msg"` | Set description of current change |
| `jj commit -m "msg"` | Describe `@` and start a new change (= describe + new) |
| `jj edit <change>` | Make an existing change the working copy |
| `jj log` | Show commit graph |
| `jj log -r 'trunk()..@'` | Show only your stack |
| `jj status` | Show what's changed in `@` |
| `jj diff` | Show diff of `@` |
| `jj show <change>` | Show a change's diff + description |

## History Editing

| Command | What it does |
|---|---|
| `jj squash` | Squash `@` into its parent |
| `jj squash --into <change>` | Squash `@` into any change |
| `jj split` | Interactively split `@` into two changes |
| `jj absorb` | Auto-route fixups to the correct ancestor |
| `jj rebase -d trunk()` | Rebase `@` onto trunk |
| `jj rebase -r X -A Y` | Move change X to after Y |
| `jj new -B <change>` | Insert a new change before another |
| `jj abandon` | Delete `@` from history |
| `jj restore` | Discard all changes in `@` |
| `jj duplicate <change>` | Copy a change |

## Bookmarks & Remotes

| Command | What it does |
|---|---|
| `jj bookmark create name -r @` | Create a bookmark at `@` |
| `jj bookmark list` | List all bookmarks |
| `jj bookmark delete name` | Delete a bookmark |
| `jj git fetch` | Fetch from all remotes |
| `jj git push` | Push all tracked bookmarks |
| `jj git push --bookmark name` | Push a specific bookmark |

## Undo & Recovery

| Command | What it does |
|---|---|
| `jj undo` | Undo the last operation |
| `jj op log` | View operation history |
| `jj op restore <op-id>` | Restore to a specific operation |

## Revsets Quick Reference

| Revset | Meaning |
|---|---|
| `@` | Current change (working copy) |
| `@-` | Parent of `@` |
| `@--` | Grandparent of `@` |
| `trunk()` | main/master branch |
| `trunk()..@` | Your stack (commits since trunk) |
| `bookmarks()` | All bookmarked commits |
| `description("text")` | Changes matching description |
| `author("name")` | Changes by author |
| `empty()` | Empty changes |
| `conflict()` | Changes with conflicts |
| `all()` | Everything |

## ryu (Stacked PRs)

| Command | What it does |
|---|---|
| `ryu` | Show stack status |
| `ryu track --all` | Track all bookmarks in your stack |
| `ryu submit` | Create/update PRs for tracked bookmarks |
| `ryu submit --draft` | Submit as draft PRs |
| `ryu submit --dry-run` | Preview without executing |
| `ryu sync` | Sync stack after PR merges |

## Git-to-jj Rosetta Stone

| Git | jj |
|---|---|
| `git add . && git commit` | `jj commit -m "msg"` (or just `jj describe`) |
| `git checkout -b branch` | `jj new` + `jj bookmark create name` |
| `git switch branch` | `jj edit <change>` |
| `git stash` | Not needed (`jj new` and come back later) |
| `git rebase -i` | `jj squash`, `jj split`, `jj rebase` |
| `git log` | `jj log` |
| `git diff` | `jj diff` |
| `git blame` | `jj file annotate` |
| `git push` | `jj git push` |
| `git pull` | `jj git fetch` (no auto-merge) |
| `git reset --hard` | `jj restore` |
| `git reflog` | `jj op log` |

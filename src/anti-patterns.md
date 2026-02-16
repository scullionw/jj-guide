# Anti-Patterns

## Treating jj like Git

The biggest mistake is mapping Git habits 1:1 onto jj.

**Don't do this:**

```bash
jj new
# ... edit ...
jj commit -m "my change"
jj new
# ... edit ...
jj commit -m "next change"
```

`jj commit` is `describe` + `new` combined. It works, but it's not how jj is designed to be used. The idiomatic flow is:

```bash
jj new
# ... edit ... (your edits ARE the commit)
jj describe -m "my change"
jj new
```

This matters because in jj, you're always working inside a change. There's no separate "stage and commit" step.

## Creating bookmarks for everything

Bookmarks are for pushing to remotes. You don't need them for local work.

**Don't:**

```bash
jj bookmark create experiment-1 -r @
jj bookmark create experiment-2 -r @
jj bookmark create temp-fix -r @
```

**Do:** Use `jj describe` to label changes and reference them by change ID.

## Mega-changes

Just because jj doesn't have a staging area doesn't mean you should dump everything into one change.

**Don't:** Work for hours without running `jj new`.

**Do:** Run `jj new` every time you finish one logical unit. If a change grew too big, use `jj split` to break it apart.

## Ignoring conflicts

jj lets you defer conflict resolution — that doesn't mean you should forget about them. Unresolved conflicts in your stack will cause problems when pushing.

**Do:** Resolve conflicts soon after they appear. Use `jj log` to spot the `(conflict)` marker.

## Deep stacks

Stacks beyond 7-8 changes become hard to review and fragile to rebase.

**Don't:** Build a 15-change stack.

**Do:** Split related work into parallel stacks or land earlier changes before stacking more.

## Editing immutable changes

Changes that have been pushed to `main` or tagged are immutable by default. Trying to edit them will fail.

**Don't:** Try to `jj edit` or `jj rebase` changes that are already on main.

**Do:** Create new changes on top of trunk.

## Force-pushing without ryu

If you're managing stacked PRs manually (without ryu), force-pushing one bookmark can break the PR chain.

**Don't:** Manually `jj git push` individual bookmarks in a stack without updating all of them.

**Do:** Use `ryu submit` which handles the entire stack atomically, or push all bookmarks in the right order.

## Not fetching before rebasing

```bash
# Don't:
jj rebase -d main              # might be stale

# Do:
jj git fetch
jj rebase -d main@origin       # always fresh
```

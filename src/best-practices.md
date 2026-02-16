# Best Practices

## Use `jj new` liberally

Start a new change before you start new work. Keep changes small and focused.

```bash
jj new
# ... one logical unit of work ...
jj describe -m "what you did"
jj new
# ... next thing ...
```

Small changes are easier to review, easier to rebase, and easier to bisect.

## Describe as you go

Don't leave changes undescribed. Future-you won't remember what `(no description set)` was about.

```bash
jj describe -m "extract validation into helper"
```

Even a short description beats nothing. You can always refine it later.

## Use `jj absorb` for fixups

When iterating on a stack, make all your fixes in `@` then run `jj absorb`. It automatically routes each hunk to the correct ancestor change. This is faster and less error-prone than manually squashing into specific changes.

## Keep stacks shallow

5-7 changes is a practical limit. Beyond that, review becomes painful and merge conflicts multiply. If your stack is growing large, consider splitting it into independent stacks.

## Rebase onto trunk frequently

```bash
jj git fetch
jj rebase -d main@origin
```

Frequent rebasing keeps conflicts small and manageable. Don't let your stack drift far from trunk.

## Use bookmarks only for pushing

Bookmarks are for communicating with remotes. You don't need them for local organization. The change ID is your stable reference.

```bash
# Don't do this for local work:
jj bookmark create wip-experiment -r @

# Do this:
jj describe -m "experiment: try caching approach"
# Reference it later by change ID
```

## Use revsets to stay oriented

```bash
jj log -r 'trunk()..@'          # your current stack
jj log -r 'bookmarks()'         # all bookmarked changes
jj log -r 'description("auth")' # find changes by description
```

## Use `jj op log` before panicking

Something went wrong? Check the operation log:

```bash
jj op log
jj undo                          # undo last operation
jj op restore <op-id>            # go back to any point
```

## Use colocated repos

Always use `--colocate` when cloning or initializing. This keeps Git tooling (CI, editors, GitHub CLI) working alongside jj.

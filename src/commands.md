# Essential Commands

## Viewing State

```bash
jj log                        # show commit graph (your branch)
jj log -r 'trunk()..@'       # show only commits since main
jj log -r 'all()'            # show everything
jj status                    # what's changed in the working copy (st works too)
jj diff                      # diff of current change
jj diff -r xyz               # diff of a specific change
jj diff --stat               # summary of changes
```

## Creating & Describing Changes

```bash
jj new                        # start a new empty change on top of @
jj new -A xyz                 # new change after change xyz
jj new -B xyz                 # new change before change xyz (inserts in history)
jj new x y                    # new merge change with parents x and y

jj describe -m "add login"   # set description of current change
jj describe xyz -m "fix bug" # set description of a different change
jj commit -m "done"          # = describe + new (describe @ and start a new change)
```

`jj new` is the command you'll use most. It's how you "finish" one change and start the next.

## Navigating

```bash
jj edit xyz                   # make change xyz the working copy
                              # (like git checkout but for editing in place)
jj new xyz                    # create a new change on top of xyz and go there
```

`jj edit` modifies an existing change in place. `jj new` creates a fresh change on top.

## Showing Changes

```bash
jj show                       # show current change's diff + description
jj show xyz                   # show a specific change
```

## Working with Files

```bash
jj file list                  # list tracked files
jj file show xyz:path/to.rs  # show file contents at a revision
jj file annotate path/to.rs  # like git blame
```

## Fetching & Pushing

```bash
jj git fetch                  # fetch from all remotes
jj git push                   # push all bookmarks with remote tracking
jj git push --bookmark feat   # push a specific bookmark
```

## Undoing Mistakes

```bash
jj undo                       # undo the last operation
jj op log                     # view operation history
jj op restore <op-id>         # go back to any point in operation history
```

`jj undo` is your safety net. It undoes the last jj operation — rebase, squash, anything. `jj op log` shows your full operation history so you can go back to any point.

# Editing History

This is where jj shines. Rewriting history is safe, easy, and automatic.

## Squash: Combine Changes

```bash
jj squash                     # squash @ into its parent
jj squash --into xyz          # squash @ into any change
jj squash -i                  # interactively pick which hunks to squash
```

Common use: you made a fix that belongs in a previous change.

```bash
# You're at @ and realize the fix belongs in change xyz
jj squash --into xyz
# Done. Descendants auto-rebase.
```

## Split: Break a Change Apart

```bash
jj split                      # interactively split @ into two changes
jj split -r xyz               # split a different change
jj split path/to/file.rs     # split out specific files
```

Opens an interactive diff selector. The parts you select go into the first change; the rest becomes the second.

## Absorb: Auto-Route Fixes to the Right Change

```bash
jj absorb
```

`jj absorb` looks at the lines you changed in `@`, finds which ancestor change last touched those lines, and automatically moves each hunk to the correct change. It's like an automatic `squash --into` that figures out the target for you.

This is incredibly useful when you're iterating on a stack — just make all your fixes in `@` and run `jj absorb`.

## Rebase: Move Changes Around

```bash
jj rebase -d trunk()          # rebase @ onto latest trunk
jj rebase -r xyz -d abc       # move a single change
jj rebase -b xyz -d trunk()   # rebase xyz and all its descendants
```

### Insert a Change Into History

```bash
jj new -B xyz                 # create new change before xyz
# ... make edits ...
jj describe -m "extracted helper function"
# xyz and everything after it auto-rebase
```

### Reorder Changes

```bash
jj rebase -r B -A C           # move B to after C
```

## Restore: Reset Content

```bash
jj restore                    # discard all changes in @
jj restore --from xyz         # restore @ to match xyz's content
jj restore path/to/file.rs   # discard changes to specific file
```

## Duplicate

```bash
jj duplicate xyz              # copy a change (new change ID)
```

## Abandon: Delete a Change

```bash
jj abandon                    # abandon @ (delete it from history)
jj abandon xyz                # abandon a specific change
```

Descendants are automatically rebased onto the abandoned change's parent.

## The Golden Rule

**You can always `jj undo`.** Every operation is recorded. If a rebase goes wrong, if you squash into the wrong place, if you accidentally abandon something — `jj undo` brings you right back.

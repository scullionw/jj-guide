# Working with Stacks

Stacked changes are jj's bread and butter. Where Git makes stacked branches painful, jj makes them natural.

## What's a Stack?

A stack is a series of changes built on top of each other, each representing one logical unit of work:

```
@  feat-session    add session handling
○  feat-auth       add auth middleware
○  feat-user       add user model
○  trunk()         main
```

## Creating a Stack

```bash
# Start from trunk
jj new trunk()
# ... build user model ...
jj describe -m "add user model"

jj new
# ... build auth middleware ...
jj describe -m "add auth middleware"

jj new
# ... build session handling ...
jj describe -m "add session handling"
```

That's it. Three changes stacked on top of each other. No branch names needed for local work.

## Viewing Your Stack

```bash
jj log -r 'trunk()..@'
```

## Editing a Change Mid-Stack

This is where jj destroys Git. Want to fix something in the middle of your stack?

```bash
jj edit <change-id>           # jump to the change
# ... make your fix ...
# All descendants auto-rebase. Done.
```

Or use `jj new -B <change-id>` to insert a new change in the middle:

```bash
jj new -B feat-auth-change
# ... add a migration ...
jj describe -m "add user migration"
# The stack restructures automatically
```

## Adding Bookmarks for Pushing

When you're ready to push to GitHub, add bookmarks:

```bash
jj bookmark create feat-user -r <user-change-id>
jj bookmark create feat-auth -r <auth-change-id>
jj bookmark create feat-session -r <session-change-id>
```

Then push:

```bash
jj git push --bookmark feat-user
jj git push --bookmark feat-auth
jj git push --bookmark feat-session
```

## Rebasing Your Stack on Latest Main

```bash
jj git fetch
jj rebase -d main@origin
```

The entire stack rebases. Conflicts, if any, are recorded in the relevant commits — they don't block the operation.

## Reordering Changes in a Stack

```bash
jj rebase -r B -A C           # move change B to after C
```

## Pulling a Change Out of a Stack

```bash
jj rebase -r xyz -d trunk()   # move xyz out, children reparent to xyz's parent
```

## The Stack Workflow Summary

1. `jj new` to start each change
2. `jj describe` to label them
3. `jj edit` to go back and fix anything
4. Bookmark + push when ready for review
5. `jj rebase -d main@origin` to stay up to date

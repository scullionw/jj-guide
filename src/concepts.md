# Core Concepts

Understanding these four ideas makes everything else click.

## 1. The Working Copy Is a Commit

In Git, you have an index (staging area) and a working tree that are separate from commits. In jj, **your working copy is always a commit** — the `@` commit.

Every time you run a jj command, it snapshots your working directory into `@` automatically. There is no `git add`. There is no "dirty working tree" error. You are always working inside a commit.

```
@  ksqlvpry you@email.com 2025-02-15 (empty) (no description set)
○  zqrmopwl you@email.com 2025-02-14 add login page
○  trunk() main
```

The `@` symbol always means "where I am right now."

## 2. Changes, Not Commits

jj has **changes** instead of Git commits. The key difference:

| | Git Commit | jj Change |
|---|---|---|
| Identity | SHA (changes when amended) | Change ID (stays the same forever) |
| Mutability | "Immutable" (rewrite = new SHA) | Mutable until pushed |
| Referencing | `abc123` | `ksqlvpry` (short change ID) |

When you amend, rebase, or squash a change, its **change ID stays the same** while the underlying commit hash updates. This means you can always refer to a change by the same short ID, even after rewriting it.

## 3. Bookmarks = Branches

jj calls branches **bookmarks**. They work like Git branches — named pointers to commits — but you don't need them for local work.

```bash
# Create a bookmark (like git checkout -b)
jj bookmark create my-feature -r @

# List bookmarks
jj bookmark list

# Push a bookmark to remote
jj git push --bookmark my-feature
```

You typically only create bookmarks when you need to push to a remote.

## 4. Revsets

Revsets are a query language for selecting commits. Think of them as CSS selectors for your commit graph.

```bash
# Common revsets
@              # working copy (current change)
@-             # parent of working copy
@--            # grandparent
trunk()        # the main/master branch
..@            # all ancestors of @ back to root
trunk()..@     # commits between trunk and @ (your branch)
bookmarks()    # all bookmarked commits
```

Used everywhere:

```bash
jj log -r 'trunk()..@'       # show only your branch
jj rebase -d trunk()          # rebase onto latest main
jj diff -r @-                 # diff of parent commit
```

## The Big Mental Model Shift

In Git, you think: "I'm on a branch, I stage files, I commit."

In jj, you think: **"I'm editing a change. When I'm done, I start a new one."**

```bash
# Git mental model
git checkout -b feature
# ... edit files ...
git add .
git commit -m "feature"

# jj mental model
jj new                        # start a new change
# ... edit files ...
jj describe -m "feature"     # describe what you did
jj new                        # start the next change
```

There's no staging, no branch creation for local work, and no commit command in the typical flow. Your edits are the commit.

# Conflicts

jj handles conflicts differently than Git — and it's a massive improvement.

## Conflicts Are Commits, Not Blockers

In Git, a conflict during rebase halts everything until you resolve it. In jj, **conflicts are recorded in the commit itself** and the operation completes. You can resolve them whenever you want.

```bash
jj rebase -d trunk()
# Rebased 3 commits, 1 with conflicts.
# The operation succeeds. You keep working.
```

The conflicted commit shows up in the log:

```
×  ksqlvpry you@email.com (conflict) add auth middleware
○  zqrmopwl you@email.com add user model
○  trunk() main
```

## Resolving Conflicts

### Option 1: Edit the conflict markers directly

```bash
jj new ksqlvpry               # check out the conflicted change
# Edit the files — jj uses standard conflict markers
# <<<<<<< Side #1
# ...
# %%%%%%% (diff from base)
# ...
# >>>>>>> Side #2
jj squash                     # squash resolution into the conflicted change
```

### Option 2: Use a merge tool

```bash
jj new ksqlvpry
jj resolve                    # opens configured merge tool
jj squash
```

Configure your merge tool:

```bash
jj config set --user merge-tools.meld.program "meld"
jj config set --user ui.merge-editor "meld"
```

### Option 3: Ignore for now

Conflicts don't block you. You can keep working on other changes and come back later. This is especially useful when rebasing a stack — resolve at your convenience.

## Conflict Labels

jj includes labels showing where each side of the conflict came from:

```
<<<<<<< Side #1 (ksqlvpry add auth middleware)
  return authenticate(user);
%%%%%%% (diff from zqrmopwl add user model)
- return validate(user);
+ return validate_and_auth(user);
>>>>>>> Side #2 (trunk)
```

This context makes it much easier to understand *why* there's a conflict.

## Key Differences from Git

| Git | jj |
|---|---|
| Rebase stops at first conflict | Rebase completes, conflicts recorded |
| Must resolve before continuing | Resolve whenever you want |
| `git mergetool` | `jj resolve` or edit markers + `jj squash` |
| Lose work if you `--abort` | Everything is in the op log, always recoverable |

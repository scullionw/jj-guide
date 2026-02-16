# Working with Git Users

Your teammates use Git. You use jj. This works seamlessly because jj speaks Git natively — they'll never know the difference.

## Fetching Other People's Branches

```bash
jj git fetch
```

This pulls all remote branches. They appear as remote bookmarks:

```bash
jj bookmark list --all
# main: abcdef12
# feature-login@origin: 789abc01
# fix-typo@origin: def45678
```

The `@origin` suffix means it's a remote-tracking bookmark you haven't set up locally yet.

## Checking Out Someone's Branch

```bash
# Create a new change on top of their branch
jj new feature-login@origin
```

This puts you on a fresh change with their branch as the parent. You can browse the code, run tests, or start building on top of it.

To just look at their code without creating a new change:

```bash
jj diff -r feature-login@origin    # see what they changed vs parent
jj show feature-login@origin       # see the change details
jj log -r '..feature-login@origin' # see their branch history
```

## Tracking a Remote Branch

If you want a local bookmark that follows a remote branch:

```bash
jj bookmark track feature-login@origin
```

Now `jj git fetch` will keep it updated. The local bookmark moves when the remote one does.

## Building on Someone's Branch

```bash
jj git fetch
jj new feature-login@origin
# ... add your changes ...
jj describe -m "add session expiry to login"
jj bookmark create login-expiry -r @
jj git push --bookmark login-expiry
```

Your PR targets their branch. When their branch updates:

```bash
jj git fetch
jj rebase -d feature-login@origin   # rebase onto their latest
```

## Reviewing a PR Locally

```bash
jj git fetch
jj diff -r feature-login@origin              # what did they change?
jj log -r 'main@origin..feature-login@origin' # all commits in their PR
```

To check it out and run tests:

```bash
jj new feature-login@origin
# run tests, poke around
jj abandon                                    # clean up when done
```

## Handling Upstream Force-Pushes

When a teammate force-pushes (rebases their branch), `jj git fetch` handles it cleanly:

```bash
jj git fetch
# jj updates the remote bookmark to the new position
```

If you had local changes on top of their old branch, rebase:

```bash
jj rebase -d feature-login@origin
```

## Pushing to Someone Else's Branch

If you have write access and want to push directly to their branch:

```bash
jj new feature-login@origin
# ... make fixes ...
jj squash                           # squash into their branch tip
jj bookmark set feature-login -r feature-login@origin
jj git push --bookmark feature-login
```

## Merging Main into a Colleague's Branch

Sometimes a teammate asks you to "merge main into my branch." In jj:

```bash
jj git fetch
jj new feature-login@origin main@origin    # create a merge commit
jj describe -m "merge main into feature-login"
jj bookmark set feature-login -r @
jj git push --bookmark feature-login
```

## Using ryu to Stack on Someone's Branch

When you're building a stack of PRs on top of a teammate's feature branch, `ryu sync` handles the tedious rebase-and-push cycle for you.

### Setup: Stack on their branch

```bash
jj git fetch
jj new feature-login@origin
# ... add your auth middleware ...
jj commit -m "add auth middleware"
jj bookmark create my-auth -r @-

jj new
# ... add session handling ...
jj commit -m "add session handling"
jj bookmark create my-session -r @-

ryu track --all
ryu submit
```

This creates:
- `my-auth` → PR targeting `feature-login`
- `my-session` → PR targeting `my-auth`

### When they update their branch

Your teammate pushes new commits to `feature-login`. Now your stack is stale. Without ryu, you'd manually fetch, rebase, and push each bookmark. With ryu:

```bash
ryu sync
```

This does three things in one step:
1. Fetches the latest from the remote
2. Rebases your local stack onto the updated `feature-login@origin`
3. Updates your PRs on GitHub so they show clean diffs against the new base

### When their branch merges into main

Once `feature-login` lands in main, your stack needs to retarget:

```bash
ryu sync
```

ryu detects that the base branch was merged, rebases your stack onto `main`, and retargets your PRs accordingly. `my-auth` now targets `main` instead of the deleted `feature-login`.

### Full collaborative workflow

```bash
# Day 1: Build on their branch
jj git fetch
jj new feature-login@origin
# ... work ...
jj commit -m "extend login with 2FA"
jj bookmark create login-2fa -r @-
ryu track login-2fa
ryu submit --draft

# Day 2: They pushed updates
ryu sync                        # rebase + update PRs

# Day 3: They pushed more, you have feedback to address
ryu sync
jj edit login-2fa               # address review comments
# ... fix ...
ryu submit                      # push updates

# Day 4: Their branch merged into main
ryu sync                        # retargets your PR to main
ryu submit --publish            # mark ready for review
```

### Why this matters for Graphite users

This is the workflow Graphite handled well — you'd `gt stack submit` and it managed retargeting automatically. `ryu sync` gives you the same experience: your PRs stay correctly chained regardless of what happens upstream, and reviewers always see clean per-change diffs.

## Common Patterns

### "I need to base my work on two people's branches"

```bash
jj new alice-feature@origin bob-feature@origin
# merge commit with both as parents
```

### "Their branch is gone, I still need it"

Remote branches that were deleted show up in the operation log:

```bash
jj op log    # find when the branch existed
jj log -r 'remote_bookmarks()'  # see what's still tracked
```

### "I want to see all remote activity"

```bash
jj git fetch
jj log -r 'remote_bookmarks()' --no-graph
```

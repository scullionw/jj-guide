# Graphite to jj

If you're coming from Graphite CLI, jj + ryu gives you the same stacked PR workflow — without the SaaS dependency and with a far more powerful local VCS.

## Concept Mapping

| Graphite | jj + ryu |
|---|---|
| `gt create` | `jj new` + `jj bookmark create` |
| `gt modify` | `jj describe` |
| `gt commit` | `jj commit` |
| `gt checkout` | `jj edit` or `jj new <change>` |
| `gt stack submit` | `ryu submit` |
| `gt sync` | `jj git fetch && ryu sync` |
| `gt restack` | Automatic (jj rebases descendants for you) |
| `gt squash` | `jj squash` |
| `gt split` | `jj split` |
| `gt absorb` | `jj absorb` |
| `gt log` | `jj log -r 'trunk()..@'` |
| `gt branch delete` | `jj abandon` + `jj bookmark delete` |
| Graphite dashboard | GitHub PR chain (via ryu) |

## What You Gain

### No more `gt restack`
In Graphite, after editing a commit mid-stack you run `gt restack` to propagate changes. In jj, **descendants auto-rebase** whenever you modify a change. There is no restack command because restacking is always automatic.

### Real undo
`jj undo` reverts any operation. `jj op log` shows your full operation history. Graphite doesn't have this.

### Conflicts don't block you
Graphite (like Git) stops on conflicts. jj records conflicts in commits and lets you resolve them on your own schedule.

### No wrapper around Git
Graphite is a layer on top of Git. jj replaces Git's internals with a better model while remaining compatible with Git remotes.

### No SaaS required
Graphite's dashboard is great, but requires a hosted service. With jj + ryu, PRs are on GitHub directly — your team doesn't need any extra tooling to review.

## What You Lose (and How to Cope)

### Graphite's web dashboard
Graphite's web UI for viewing stacks is excellent. With ryu, your stacks are visible as chained PRs on GitHub — each PR targets the previous one. Reviewers see the dependency chain in the PR description.

For a visual overview locally:

```bash
jj log -r 'trunk()..@'        # see your stack
ryu                            # see tracked bookmarks + PR status
```

### Auto-naming branches
Graphite auto-generates branch names. In jj, you name bookmarks yourself. Keep them short and consistent:

```bash
jj bookmark create auth-model -r @
jj bookmark create auth-middleware -r @
jj bookmark create auth-session -r @
```

### `gt submit` simplicity
Graphite's submit is one command. With ryu, you need to create bookmarks first, then track, then submit. But this extra step gives you more control over what gets pushed.

## Migration Workflow

### Step 1: Set up jj in your repo

```bash
cd your-repo
jj git init --colocate
```

### Step 2: Install ryu

```bash
npm install -g jj-ryu
```

### Step 3: Start stacking

```bash
jj new main
# ... code ...
jj commit -m "first change"
jj bookmark create my-first -r @-

jj new
# ... code ...
jj commit -m "second change"
jj bookmark create my-second -r @-

ryu track --all
ryu submit --draft
```

### Step 4: Uninstall Graphite (when ready)

```bash
npm uninstall -g @withgraphite/graphite-cli
```

## Day-to-Day: Graphite vs jj Side by Side

### Starting a new stack

```bash
# Graphite
gt create -m "add user model"

# jj
jj new main
# ... work ...
jj describe -m "add user model"
jj bookmark create user-model -r @
jj new   # ready for next change
```

### Editing a change mid-stack

```bash
# Graphite
gt checkout <branch>
# ... edit ...
gt modify
gt restack

# jj
jj edit <change-id>
# ... edit ...
# done (auto-rebase)
```

### Submitting

```bash
# Graphite
gt stack submit

# jj + ryu
ryu submit
```

### Syncing after merge

```bash
# Graphite
gt sync

# jj + ryu
jj git fetch
ryu sync
```

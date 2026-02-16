# Configuration

jj uses TOML config files. Edit with:

```bash
jj config edit --user           # global config (~/.jjconfig.toml)
jj config edit --repo           # repo-specific config (.jj/repo/config.toml)
```

## Recommended Starter Config

```toml
[user]
name = "Your Name"
email = "you@example.com"

[ui]
editor = "nvim"
default-command = "log"
diff-editor = "meld"
merge-editor = "meld"
pager = "less -FRX"

[git]
auto-local-bookmark = false
```

## Useful Aliases

```toml
[aliases]
l = ["log", "-r", "trunk()..@"]
la = ["log", "-r", "all()"]
d = ["diff"]
ds = ["diff", "--stat"]
s = ["status"]
f = ["git", "fetch"]
p = ["git", "push"]
```

Then use:

```bash
jj l                            # show your stack
jj la                           # show everything
jj d                            # current diff
jj ds                           # diff summary
jj f                            # fetch
jj p                            # push
```

## Custom Log Template

Customize what `jj log` shows:

```toml
[templates]
log = '''
separate(" ",
  change_id.shortest(8),
  if(bookmarks, bookmarks.join(", ")),
  if(conflict, "(conflict)"),
  description.first_line(),
) ++ "\n"
'''
```

## Diff Format

```toml
[ui]
diff.format = "git"              # git-style diffs (default)
# diff.format = "color-words"   # inline word-level diffs
```

`color-words` is great for prose and small changes. `git` format is better for code review.

## Immutable Commits

By default, jj prevents editing commits on `main` or tagged commits. Customize:

```toml
[revset-aliases]
"immutable_heads()" = "trunk() | tags()"
```

## Config Precedence

1. **CLI flags** (highest priority)
2. **Repo config** (`.jj/repo/config.toml`)
3. **User config** (`~/.jjconfig.toml`)
4. **Default config** (built into jj)

## Viewing Current Config

```bash
jj config list                  # show all config values
jj config list ui               # show ui section
jj config get ui.editor         # show specific value
```

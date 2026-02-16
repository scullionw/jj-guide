# The jj Guide

**Jujutsu (jj)** is a Git-compatible version control system that replaces Git's clunky mental model with something that actually makes sense. It's built in Rust, stores commits as immutable snapshots, and treats your working copy as a real commit — meaning "dirty working directory" errors are a thing of the past.

This guide is for developers who want to be productive with jj quickly, especially those coming from **Graphite CLI** who want stacked PRs without the SaaS dependency.

## Why jj?

- **No staging area.** Your working copy *is* a commit. Every change is automatically tracked.
- **Automatic rebasing.** Edit any commit in your history and descendants rebase themselves.
- **First-class conflicts.** Conflicts are recorded in commits, not workflow-blocking emergencies.
- **Undo everything.** Every operation is recorded and reversible with `jj undo`.
- **Git compatible.** Uses Git as a storage backend. Your teammates don't need to know you're using jj.

## Who This Guide Is For

- Developers curious about jj who want a practical, no-fluff guide
- **Graphite CLI users** looking for an open-source, local-first alternative for stacked PRs
- Git power users tired of `rebase -i` nightmares

## What This Guide Covers

1. Setup and core mental model
2. Daily commands and history editing
3. Stacked PR workflows with **jj-ryu**
4. A full Graphite-to-jj migration path
5. Best practices, anti-patterns, and a cheat sheet

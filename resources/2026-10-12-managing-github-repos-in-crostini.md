---
title: Managing github repos in crostini
date: 2026-10-12 01:02 +0100
status: done
tags: []
---

# Workspace CLI Cheatsheet (`cdw` & `ws`)

A unified command-line reference for managing multi-repo workflows in `~/workspace`.

---

## 1. Directory Navigation (`cdw`)

Fast directory switching to the workspace root or directly into any cloned repository.

| Command | Action |
| --- | --- |
| `cdw` | Jump directly to `~/workspace` |
| `cdw <repo>` | Jump directly into a repository (e.g., `cdw notes`, `cdw readover`) |

---

## 2. Multi-Repo Operations (`ws`)

Manage Git status, commits, synchronisation, and GitHub alignment across all repositories or against a specific target repository.

### Daily Git Workflow

| Command | Alias | Scope | Action |
| --- | --- | --- | --- |
| `ws status` | `wst` | All | Scans all repos; lists dirty files and unpushed commits |
| `ws status <repo>` | `wst <repo>` | Single | Checks status for a specific repository |
| `ws commit` | `wsc` | All | Interactively steps through dirty repos: diff (`d`), commit (`c`), skip (`s`), or quit (`q`) |
| `ws commit <repo>` | `wsc <repo>` | Single | Interactively reviews, commits, and pushes only the specified repo |
| `ws pull` | `wsl` | All | Safely pulls upstream changes across all repos (auto-rebase & autostash) |
| `ws pull <repo>` | `wsl <repo>` | Single | Safely pulls upstream changes for a single repository |
| `ws push` | `wsp` | All | Pushes all committed, outgoing commits across all repositories |
| `ws push <repo>` | `wsp <repo>` | Single | Pushes outgoing commits for a specific repository |

### Account & Repository Sync

| Command | Action |
| --- | --- |
| `ws list` / `ws ls` | Lists all active Git repositories inside `~/workspace` |
| `ws clone-missing` | Clones any newly created GitHub repos on your account and refreshes `main.code-workspace` |
| `ws prune-deleted` | Detects local folders whose remotes were deleted on GitHub, verifies dirty state, prompts before deletion, and updates `main.code-workspace` |

---

## 3. Common Workflows

### Starting the Day

Pull downstream changes across all repositories without overwriting local drafts:

```bash
wsl

```

### Reviewing & Committing Changes

Check for modified files after editing in an external editor or the Files app:

```bash
wst

```

Commit only one specific repository:

```bash
wsc notes

```

*Prompt actions:* Press `d` to inspect line diffs, `c` to enter a commit message, and confirm `Y` to push immediately.

Commit all dirty repositories across the workspace sequentially:

```bash
wsc

```

### Keeping Local Storage in Sync with GitHub

When you add a new repo on GitHub via the web or mobile:

```bash
ws clone-missing

```

When you delete an old repo on GitHub:

```bash
ws prune-deleted

```

### Deep Dive into a Single Repo

Jump in, use native Git features, and return to workspace root:

```bash
cdw readover
git log -n 5 --oneline
cdw

```

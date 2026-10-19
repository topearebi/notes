---
title: Github Repo Cheatsheet
date: 2026-10-19 19:49 +0100
tags: []
---

### Phase 1: Pre-Work Routine (Before Writing Code)

Whenever you sit down to start a session, make this a single continuous muscle-memory habit:

1. **Check the board:**
```bash
wst

```


Because `wst` auto-fetches if online, this immediately flags incoming commits (the `behind` count) across all repositories without you needing to remember `git fetch`.
2. **Jump and align:**
```bash
cdw rally
wsl rally

```


Running `wsl` uses `--rebase` and `--autostash`. If you left accidental dirty edits or untracked changes in the repo from a previous session, it safely stashes them, pulls the latest commits cleanly onto your branch, and restores your work.
3. **Launch your tools:**
Open your file in VS Code or trigger `opencode`. You are now guaranteed to be starting from the exact remote `HEAD`.

---

### Phase 2: In-Work Routine (While Working with OpenCode)

OpenCode will generate file edits directly on disk. Keep track of progress using your repo-targeted shortcuts:

* Check what OpenCode just modified:
```bash
wst rally

```


* Review diffs and save your progress in clean units:
```bash
wsc rally

```


Use `d` to inspect the code diff proposed by the agent, then `c` to commit and push immediately.

---

### Phase 3: Post-Work Routine (Closing Out)

When you finish your work or switch contexts:

```bash
wss rally

```

Or, if you touched multiple projects during the session:

```bash
wss

```

`wss` runs the end-to-end safe sync (fetch $\to$ rebase pull $\to$ push). This guarantees you never leave local unpushed commits sitting on this machine that will later cause a conflict if you edit elsewhere.

---

### Weekly / Start-of-Week Housekeeping

Run this once at the start of the week or after doing repo management on GitHub:

```bash
ws clone-missing   # Pulls down any repos newly created on GitHub
wsrem              # Re-aligns any renamed repos
wsbc               # Verifies you aren't stuck on an accidental detached HEAD or orphan branch

```

### Quick Reference Card

| Scenario | Command |
| --- | --- |
| **"What's behind or dirty right now?"** | `wst` |
| **"I'm about to work on project X"** | `cdw X && wsl X` |
| **"Commit what I/OpenCode just did"** | `wsc X` |
| **"Wrap up and sync everything cleanly"** | `wss` |


* `cdw`: Jump directly to `~/workspace`
* `cdw <repo>`: Jump directly into a specific repository folder
* `ws list` (or `ws ls`): List all managed Git repositories in the workspace
* `ws status` (or `wst`): Scan all repos; auto-fetches if online to display uncommitted edits, branch name, and ahead/behind counts
* `ws status <repo>` (or `wst <repo>`): Check real-time Git status for a specific repository
* `ws commit` (or `wsc`): Step through all modified repos interactively to inspect diffs (`d`), commit (`c`), skip (`s`), or quit (`q`)
* `ws commit <repo>` (or `wsc <repo>`): Interactively review, commit, and push changes for a specific repository
* `ws sync` (or `wss`): End-to-end safe sync (fetches, pulls with rebase & autostash, and pushes local commits) across all repos
* `ws sync <repo>` (or `wss <repo>`): Safe end-to-end sync for a single repository
* `ws fetch` (or `wsf`): Fetch and prune all remote references across all repositories
* `ws fetch <repo>` (or `wsf <repo>`): Fetch and prune remote references for a specific repository
* `ws pull` (or `wsl`): Pull remote updates across all repos using explicit `--rebase` and `--autostash`
* `ws pull <repo>` (or `wsl <repo>`): Pull remote updates for a specific repository
* `ws push` (or `wsp`): Push all outgoing local commits across all repos using `--force-with-lease`
* `ws push <repo>` (or `wsp <repo>`): Push outgoing local commits for a specific repository
* `ws sync-remotes` (or `wsrem`): Query GitHub API to detect renamed repos, update origin URLs, rename local folders, and refresh `main.code-workspace`
* `ws branch-check` (or `wsbc`): Audit the workspace for detached HEAD states or non-default branches
* `ws clone-missing`: Clone any newly created GitHub repos on your account and refresh `main.code-workspace`
* `ws prune-deleted`: Detect local directories deleted on GitHub, check for uncommitted work, prompt for confirmation, and update `main.code-workspace`

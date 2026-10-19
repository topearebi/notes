---
title: Managing Github Repos in Crostini Cheatsheet
date: 2026-10-19 19:49 +0100
---

# Workspace & Git CLI Cheatsheet (`ws` & `cdw`)

A definitive reference manual for managing multi-repository workflows, notes, and code projects under `~/workspace` on ChromeOS.

---

## 1. Command & Alias Reference

### `cdw [repo]`

* **One-sentence description:** Changes your current working directory directly to `~/workspace` or immediately into any named repository.
* **Scenarios & Utility:**
* Jump straight into your notes vault from anywhere in the filesystem: `cdw notes`.
* Jump into a project folder to run granular developer commands (e.g. `npm run dev`, `cargo build`, or complex `git rebase` tasks): `cdw reflex`.
* Return to the workspace root to check folder structures: `cdw`.



---

### `ws list` (or `ws ls`)

* **One-sentence description:** Lists all active Git repositories found inside your workspace root.
* **Scenarios & Utility:**
* Quick recall of the exact repository directory names on disk before running targeted commands.
* Verifying whether a newly cloned project is indexed and recognized as a valid Git repo.



---

### `ws status [repo]` (or `wst [repo]`)

* **One-sentence description:** Silently fetches remote refs if online and displays uncommitted changes, active branches, detached HEAD states, and ahead/behind counts against GitHub.
* **Scenarios & Utility:**
* **Morning check / Pre-work routine:** Running `wst` first thing to see which projects received updates from other devices while you were away.
* **Pre-commit sanity check:** Running `wst notes` or `wst reflex` after modifying files in the ChromeOS Files app to verify which files were created, edited, or deleted before committing.
* **Offline triage:** Safely inspects dirty local files when disconnected without throwing network timeout errors or stalling.



---

### `ws commit [repo]` (or `wsc [repo]`)

* **One-sentence description:** Interactively steps through modified repositories, allowing you to review line diffs, enter custom commit messages, stage changes, and immediately push.
* **Scenarios & Utility:**
* **Targeted single-repo commit:** You finished drafting an essay or updating code and want to ship it immediately: `wsc notes` (press `d` to verify diff, `c` to commit, and `Y` to push).
* **End-of-day fleet review:** You spent the afternoon writing in multiple repos and want to review and commit each project sequentially with distinct commit messages: `wsc`.
* **GUI file replacement:** You dropped an asset pack or replaced an entire subfolder in the Files app and need Git to stage the deletions and additions in one clean commit.



---

### `ws sync [repo]` (or `wss [repo]`)

* **One-sentence description:** Executes an end-to-end two-way synchronization by fetching upstream, applying remote commits via rebase with autostash, and pushing outgoing local commits.
* **Scenarios & Utility:**
* **The "I want everything up to date" command:** The fastest way to pull in latest updates and push out local commits across the entire workspace in one shot: `wss`.
* **Before switching devices:** You are packing your Chromebook to leave your desk and want all notes pushed to GitHub while pulling any changes made earlier on your phone or desktop: `wss notes`.



---

### `ws fetch [repo]` (or `wsf [repo]`)

* **One-sentence description:** Downloads all remote references and prunes deleted tracking branches from GitHub without modifying or moving your active local files.
* **Scenarios & Utility:**
* **Zero-risk inspection:** You want your Git database updated to know if colleagues or external automations pushed commits, but you are in the middle of active uncommitted work and cannot risk modifying your working tree.
* **Pruning dead branch markers:** Removing stale `origin/<branch>` references locally after pull requests have been merged and deleted on GitHub.



---

### `ws pull [repo]` (or `wsl [repo]`)

* **One-sentence description:** Downloads remote changes and integrates them into your current branch using explicit `--rebase` and `--autostash`.
* **Scenarios & Utility:**
* **Pulling down web edits:** You edited a Markdown note directly on the GitHub web interface or via an e-reader sync script, and need those commits pulled down into your Chromebook: `wsl notes`.
* **Pulling while actively drafting:** You have uncommitted notes sitting in your Files app and want incoming remote updates; `wsl` automatically stashes your drafts, pulls the remote commits underneath, and pops your drafts back on top cleanly.



---

### `ws push [repo]` (or `wsp [repo]`)

* **One-sentence description:** Pushes all committed, outgoing commits across the target repositories using `--force-with-lease` safety rules.
* **Scenarios & Utility:**
* **Publishing offline commits:** You made several local commits while offline on a train or plane; once back on Wi-Fi, run `wsp` to broadcast all outgoing commits across all repositories simultaneously.
* **Pushing a specific finished project:** Pushing only a specific repo where you previously committed locally: `wsp reflex`.



---

### `ws sync-remotes [repo]` (or `wsrem [repo]`)

* **One-sentence description:** Audits your local repositories against the GitHub API, updates `origin` URLs if a repo was renamed, renames the local folder to match, and refreshes `main.code-workspace`.
* **Scenarios & Utility:**
* **After renaming a repo on GitHub Web:** You renamed `speedrecall` to `reflex` on GitHub; running `wsrem` updates the `.git/config` remote URL, renames `/home/ta/workspace/speedrecall` to `/home/ta/workspace/reflex`, and updates your VS Code workspace automatically.
* **Fixing redirect warnings:** Eliminates annoying Git remote redirect notices emitted by GitHub when interacting with an old repository slug.



---

### `ws branch-check [repo]` (or `wsbc [repo]`)

* **One-sentence description:** Scans all repositories and flags any that are in a detached HEAD state or resting on a branch other than `main` or `master`.
* **Scenarios & Utility:**
* **Post-merge or checkout audit:** Catches accidental checkouts of commit tags or detached HEADs created by automated tools, scripts, or AI assistants before you write new files.
* **Detecting unmerged feature branches:** Highlights repos where you left a feature branch checked out (e.g., `feature/v3`) instead of returning to `main`.



---

### `ws clone-missing`

* **One-sentence description:** Scans your GitHub account via `gh` CLI, clones any remote repository not currently found in `~/workspace`, and regenerates `main.code-workspace`.
* **Scenarios & Utility:**
* **New repo setup:** You created a new repository via the GitHub web interface or imported an external template on another device; running `ws clone-missing` automatically pulls it into your local workspace.
* **New environment restore:** Fast-clones your entire account fleet on a freshly installed container.



---

### `ws prune-deleted`

* **One-sentence description:** Compares your local directories against active GitHub repositories, alerts you if a remote repo was deleted, verifies local dirty states, and safely prompts before removing the folder.
* **Scenarios & Utility:**
* **Decommissioning dead projects:** You deleted or archived disposable test repositories on GitHub and want to prune the leftover dead folders from your Chromebook local storage without wiping uncommitted work.



---

## 2. Best Practice Multi-Repo Hygiene

1. **Pull First, Edit Second (`wsl` before work):**
* Git clashes happen when you edit lines locally that were modified remotely before your machine fetched the update. Make running `wst` or `wsl` your automatic first action when opening your Chromebook.


2. **Never Touch `.git` During GUI Operations:**
* When dragging, dropping, or bulk-replacing files in the ChromeOS Files app, ensure hidden files stay hidden (`Ctrl + .` toggles visibility). As long as `.git` remains intact inside each project root, Git treats large file swaps as ordinary modifications/deletions.


3. **Commit Atomically by Context:**
* Use `wsc notes` when writing notes, and separate commits for codebases. Keep note commits descriptive (e.g. `refactor: consolidate highlights taxonomy`) rather than relying on endless generic messages.


4. **Enforce `rebase.autoStash` Everywhere:**
* Standard merge commits (`Merge branch 'main' of...`) create spiderweb commit logs and frequent branch divergences. The `--rebase --autostash` flag configured in your helper keeps commit histories strictly linear and painless.


5. **Keep Track of Branch States (`wsbc`):**
* If a repo enters detached HEAD, any commits you make will disappear from the branch pointer once you switch branches. Periodically run `wsbc` to verify all repos reside safely on `main`.



---

## 3. Quick Fixes for Frequently Occurring Git Issues

### Issue 1: "Local repo is behind, but `git status` says up to date"

* **Cause:** `git status` only queries your local cache. If no fetch occurred, Git does not know the remote moved forward.
* **Fix:** Run `wst` (which fetches before checking) or run `wsl` to pull down the newest commits directly.

### Issue 2: "Push rejected (non-fast-forward / fetch first)"

* **Cause:** Remote commits were pushed from another device that do not exist locally yet.
* **Fix:**
```bash
cdw <repo>
git pull --rebase --autostash origin main
git push

```



### Issue 3: "Merge / Rebase Conflict markers appear in Markdown files (`<<<<<<< HEAD`)"

* **Cause:** You edited the exact same paragraph on two different devices before syncing.
* **Fix:**
1. Open the file in your editor or ChromeOS Files app.
2. Search for `<<<<<<< HEAD`, `=======`, and `>>>>>>>`.
3. Delete the marker lines and keep the desired text.
4. Run:
```bash
git add <filename>
git rebase --continue
git push

```





### Issue 4: "You are in 'detached HEAD' state"

* **Cause:** A specific commit hash or tag was checked out instead of a named branch pointer.
* **Fix:**
* If you have no commits to keep:
```bash
git checkout main

```


* If you created commits while detached that you want to keep:
```bash
# Create a temporary rescue branch containing your detached commits
git branch temp-rescue
git checkout main
git merge temp-rescue
git branch -d temp-rescue
git push

```





### Issue 5: "Case-sensitivity collision (`Highlights` vs `highlights`)"

* **Cause:** Creating a folder with uppercase characters remotely while a lowercase version exists locally on Linux.
* **Fix:**
```bash
cdw <repo>
git mv oldfolder tempfolder
git commit -m "temp rename"
git mv tempfolder TargetFolder
git commit -m "normalize case"
git push

```

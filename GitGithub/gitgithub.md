# Git & GitHub Notes

## Git

| Command | Description |
|---|---|
| `git init` | Initialize a new Git repository |
| `ls .git` | View Git's internal components |
| `git status` | Show tracked/untracked and modified files |
| `git add .` | Stage all untracked/modified files |
| `git add <filename>` | Stage a specific file |
| `git commit -m "<message>"` | Commit staged files with a message |
| `git restore --staged <filename>` | Unstage a file (undo `git add`) |
| `git log` | View full commit history |
| `git reset <commitID>` | Remove all commits after the given commit ID |

### Stashing

| Command | Description |
|---|---|
| `git stash` | Save work-in-progress to a temporary area |
| `git stash pop` | Restore the latest stash and remove it |
| `git stash apply` | Restore the latest stash without deleting it |
| `git stash drop` | Delete the latest stash without applying |
| `git stash clear` | Delete all stashes without applying |

---

## GitHub

| Command | Description |
|---|---|
| `git remote add origin <repo-link>` | Link a GitHub repo to your local Git |
| `git remote -v` | View all remote links |
| `git push origin master` | Push commits to GitHub |

---

## Branching

| Command | Description |
|---|---|
| `git branch <branchname>` | Create a new branch |
| `git checkout <branchname>` | Switch to a branch |
| `git merge <branchname>` | Merge a branch into the current branch |

### Branch Lifecycle

**Initial state (on `main`):**
```
A --- B --- C   (main, HEAD)
```

**After creating and switching to a feature branch:**
```bash
git branch feature-login
git checkout feature-login
```
```
A --- B --- C   (main)
            \
             (feature-login, HEAD)
```

**After committing on the feature branch:**
```
A --- B --- C            (main)
            \
             D --- E     (feature-login, HEAD)
```

**After merging (pull request accepted):**
```
A --- B --- C ---------- F   (main, HEAD)
            \           /
             D --- E
```

---

## Forking & Contributing to Open Source

| Command | Description |
|---|---|
| Fork (on GitHub) | Create your own copy of a project |
| `git clone <forked-repo-link>` | Download the forked repo locally |
| `git remote add upstream <original-repo-link>` | Link the original repo |
| `git push origin <branch-name> -f` | Force push (rewrites history) |
| `git fetch --all --prune` | Fetch all changes from all remotes |
| `git reset --hard upstream/main` | Match your local repo to upstream's state |
| `git pull upstream main` | Shortcut for fetch + reset (syncs with upstream) |
| `git push origin main` | Push the synced commits to your fork |

> **Tip:** For multiple pull requests, create separate branches for each change!

---

## Useful Shortcuts

```bash
# Stage and commit in one line
git add .; git commit -m "your message"
```

---

## Advanced

### Squashing Commits with Rebase

```bash
git rebase -i <commitID>
```

A file opens listing your commits. Mark each as either `pick` or `s` (squash). Any commit marked `s` is squashed into the `pick` commit above it. You can also edit the final commit message.

### Merge Conflict Resolution

GitHub provides a built-in UI to resolve merge conflicts — it lets you choose which changes to keep and discard the rest.

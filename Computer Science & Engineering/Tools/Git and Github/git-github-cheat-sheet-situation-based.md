# Git and Github Cheat Sheet (situation based)
Here is a comprehensive Git and GitHub cheat sheet organized by **problem/situation**. This is designed to follow the natural workflow of a programmer, from starting a task to fixing mistakes.

---

## 1. Setup & Configuration
**Situation:** *I’m setting up Git for the first time or on a new machine.*

| Command | Description |
| :--- | :--- |
| `git config --global user.name "Name"` | Set your username (visible in commits). |
| `git config --global user.email "email"` | Set your email (must match GitHub account). |
| `git config --global core.editor "code --wait"` | Set VS Code (or others) as the default text editor for Git messages. |
| `git config --global init.defaultBranch main` | Set default branch to `main` instead of `master`. |
| `git config --list` | View all current configuration settings. |

---

## 2. Starting a Project
**Situation:** *I need to start a new project or download an existing one.*

| Command | Description |
| :--- | :--- |
| `git init` | Initialize a new local Git repository in the current folder. |
| `git clone <url>` | Download an existing repository from GitHub/GitLab. |
| `git clone <url> .` | Clone into the *current* directory (folder must be empty). |

---

## 3. The Daily "Save" Loop
**Situation:** *I have written code and want to save my progress.*

| Command | Description |
| :--- | :--- |
| `git status` | **Check this constantly.** Shows modified, staged, and untracked files. |
| `git diff` | See exactly what lines of code changed in modified files. |
| `git add <file>` | Stage a specific file for the next commit. |
| `git add .` | Stage **all** changed/new files in the current directory. |
| `git commit -m "message"` | Commit staged files with a descriptive message. |
| `git commit -am "message"` | Shortcut: Stage tracked files AND commit (skips `git add` for new files). |

---

## 4. Branching (Feature Workflow)
**Situation:** *I need to work on a new feature or fix without breaking the main code.*

| Command | Description |
| :--- | :--- |
| `git branch` | List all local branches. |
| `git branch <name>` | Create a new branch (but stay on the current one). |
| `git switch <name>` | Switch to an existing branch. |
| `git switch -c <name>` | Create a new branch **and** switch to it immediately. |
| `git merge <name>` | Merge branch `<name>` into your *current* branch. |
| `git branch -d <name>` | Delete a branch (safe delete). |
| `git branch -D <name>` | Force delete a branch (even if unmerged). |

---

## 5. Syncing with GitHub (Remote)
**Situation:** *I need to upload my code or get the latest code from teammates.*

| Command | Description |
| :--- | :--- |
| `git remote add origin <url>` | Connect local repo to a GitHub repo. |
| `git remote -v` | Verify connected remote URLs. |
| `git push -u origin <branch>` | Upload branch to GitHub (run once to link branch). |
| `git push` | Upload commits (after linking with `-u`). |
| `git pull` | Fetch changes from GitHub and merge them instantly. |
| `git fetch` | Download changes from GitHub but **do not** merge them yet. |

---

## 6. "Oh No!" (Undoing Mistakes)
**Situation:** *I messed something up and need to go back.*

### Case A: I haven't committed yet
| Command | Description |
| :--- | :--- |
| `git restore <file>` | Discard changes in a file (revert to last commit state). |
| `git restore .` | **Destructive:** Discard ALL local changes in the folder. |
| `git restore --staged <file>` | Unstage a file (keep changes, but remove from `git add`). |

### Case B: I just committed, but messed up the message or forgot a file
| Command | Description |
| :--- | :--- |
| `git commit --amend -m "New msg"` | Change the last commit message. |
| `git add <file>` + `git commit --amend` | Add the forgotten file to the previous commit. |

### Case C: I want to undo commits (Time Travel)
| Command | Description |
| :--- | :--- |
| `git reset --soft HEAD~1` | Undo last commit, but **keep code changes** staged. |
| `git reset --hard HEAD~1` | **Danger:** Undo last commit and **destroy** all work since then. |
| `git revert <commit-hash>` | Create a *new* commit that does the opposite of a previous one (safe for shared repos). |

---

## 7. Context Switching (Stashing)
**Situation:** *I’m in the middle of messy work, but I need to switch branches to fix a bug immediately.*

| Command | Description |
| :--- | :--- |
| `git stash` | Temporarily shelf (hide) all modified tracked files. |
| `git stash list` | View the list of stashed changes. |
| `git stash pop` | Apply the last stash back to your workspace and delete it from the stash list. |
| `git stash apply` | Apply the stash but keep it in the list. |

---

## 8. Cleaning Up History (Refining)
**Situation:** *My commit history is messy or I'm on the wrong branch.*

| Command | Description |
| :--- | :--- |
| `git log` | View commit history. |
| `git log --oneline --graph` | View a pretty, summarized visual graph of history. |
| `git rebase main` | Update your branch with the latest `main` code (keeps history linear). |
| `git cherry-pick <hash>` | Copy a specific commit from another branch onto your current branch. |

---

## 9. Dealing with Conflicts
**Situation:** *Git says "Merge Conflict" and stops me.*

1.  Run `git status` to see which files are conflicting.
2.  Open those files in your editor.
3.  Look for the markers: `<<<<<<<`, `=======`, `>>>>>>>`.
4.  Delete the markers and the code you **don't** want, keeping the code you **do** want.
5.  **Routine:**
    ```bash
    git add <fixed-file>
    git commit              # (If merging)
    git rebase --continue   # (If rebasing)
    ```

---

## 10. Useful .gitignore Patterns
**Situation:** *Git keeps trying to upload my node_modules or .env file.*

Create a file named `.gitignore` in the root. Common patterns:
```text
# Dependencies
node_modules/
/venv
/bin

# Environment Variables (Passwords)
.env

# OS generated files
.DS_Store
Thumbs.db

# Build output
dist/
build/
```

---

### 💡 Pro-Tip: Aliases
Programmers love speed. You can add these to your global config to type less:

```bash
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.lg "log --graph --oneline --decorate"
```

**Usage:** Now you can just type `git st` instead of `git status`.
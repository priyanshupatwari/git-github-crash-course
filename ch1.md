
# Chapter 1: The Core Workflow (Modules 01-05)

This section covers the absolute fundamentals: creating repos, the specific rules of adding/committing, viewing history, and file management.

## 1. Setup & Configuration (Module 03)

Before you type a single command, ensure your environment is set.

### Global vs. Local Config

Git has three levels of config: System, Global (User), and Local (Project).

* **Check your config:** `git config --list`
* **Edit your config:** `git config --global --edit`

**The Essential Commands:**

```bash
# Identity (Required for commits)
git config --global user.name "Your Name"
git config --global user.email "you@example.com"

# Editor (Vim is default, change it if you hate it)
git config --global core.editor "code --wait" 

# Line Endings (CRLF vs LF)
# Windows:
git config --global core.autocrlf true
# Mac/Linux:
git config --global core.autocrlf input

```
<!-- 
> **Visual Reference:** See **`001 03 - Installation & Setup.pdf`** (Pages 12-14) for the specific syntax on setting user identity. -->

---

## 2. The Repository & The 3 States (Module 04)

### Initialization

```bash
git init

```

This creates the hidden `.git` folder.

* **Crucial Concept:** If you delete the `.git` folder, your project is no longer a Git repo. You lose all history, but your current files remain.

### The Three States (The "Area" Model)

You cannot understand `git add` without this.

1. **Working Directory:** The files you see in your folder.
2. **Staging Area (Index):** The "waiting room" for the next commit.
3. **Repository:** The committed history.

---

## 3. The Commit Cycle: Status, Add, Commit (Module 04)

### Inspection (`git status`)

Run this before *every* other command.

```bash
git status

```

* **Untracked:** Git sees the file but isn't tracking it.
* **Modified:** Git tracks it, and it has changed since the last commit.
* **Staged:** Ready to be committed.

### Staging (`git add`)

The command `git add` moves changes from **Working Directory**  **Staging Area**.

```bash
# Stage a specific file
git add index.html

# Stage multiple specific files
git add index.html style.css

# Stage EVERYTHING in the current directory (New, Modified, and Deleted files)
git add .

```

### Committing (`git commit`)

Moves changes from **Staging Area**  **Repository**.

```bash
# Standard commit (opens your default text editor for the message)
git commit

# Inline message (faster, for short messages)
git commit -m "Fix login bug"

```
<p align="center">
  <img src="./assets/{7A8FFDDD-FF34-4437-8B8E-709866091FCA}.png" width="49%" alt="Image 2"/>
  <img src="./assets/{5B1AC1F2-1A11-4FAD-BAC4-2EBA63EF98A6}.png" width="49%" alt="Image 1"/>
</p>


---

## 4. Viewing History (`git log`) (Module 04)

The default `git log` is often too verbose.

```bash
# Standard log (shows full hash, author, date, message)
git log

# The "One Line" Log (Essential for overview)
# Shows only the short hash and the title.
git log --oneline

# Limit the number of commits (e.g., last 2)
git log -2

# Show the log for a SPECIFIC file (History of just one file)
git log index.html

```

---

## 5. Commits in Detail (Module 05)

### Amending Commits (Fixing the previous commit)

**Scenario:** You committed, but you made a typo in the message, OR you forgot to add one file.

* **Warning:** Do **not** do this if you have already pushed the commit to GitHub. It rewrites history.

```bash
git commit -m ""

# 1. Make your changes (fix typo in file, add missing file)
git add forgotten_file.js

# 2. Add them to the previous commit (and optionally update the message)
git commit --amend

```

<p align="center">
  <img src="./assets/{D09CF543-63A7-4C83-BD0F-89D56534B7EF}.png" width="93%" alt="Image"/>
</p>

### Atomic Commits

* **Concept:** A commit should do **one thing**.
* **Why?** If you commit a "Bug Fix" and a "New Feature" together, and the "New Feature" breaks the site, you can't easily undo *just* the feature without undoing the bug fix.

---

## 6. File Management: Deleting & Renaming (Module 05)

This is where beginners get stuck.

### Deleting Files (`rm`)

* **The "Unix" Way (Manual):**
1. Delete the file in your file explorer: `rm file.txt`
2. Tell Git you deleted it: `git add file.txt`
3. Commit.


* **The "Git" Way (Faster):**
Removes the file from the file system AND stages the deletion in one step.
```bash
git rm file.txt

```



### The "Cached" Delete (Crucial for `.gitignore`)

**Scenario:** You accidentally committed a huge video file or a secrets file. You want to keep the file on your computer, but stop Git from tracking it.

```bash
git rm --cached secret_config.json

```

### Renaming Files (`mv`)

* **The "Unix" Way:**
1. Rename file: `mv old.txt new.txt`
2. Git sees this as **Deleted `old.txt**` and **Created `new.txt**`.
3. You must `git add old.txt` (to delete) and `git add new.txt` (to create).


* **The "Git" Way:**
Git detects the rename immediately.
```bash
git mv old.txt new.txt

```



---

## 7. The `.gitignore` File (Module 05)

A text file named `.gitignore` in your project root tells Git what to **never** track.

**Common Patterns:**

```text
# Ignore specific file
secrets.json

# Ignore all files with an extension
*.log

# Ignore a folder (and everything inside it)
node_modules/

# Negate (Ignore everything BUT this file)
!important.log

```

* **Note:** If a file is *already* being tracked, adding it to `.gitignore` does nothing. You must use `git rm --cached <file>` first.
---

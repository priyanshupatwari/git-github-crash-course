# Chapter 5: Advanced Mechanics & Productivity (Modules 15–20)

## 1. Rebasing: The "Clean History" Tool (Module 15)

Rebasing solves the same problem as Merging (combining branches), but the result looks different.

### The Golden Rule

> **NEVER rebase a branch that you have shared with others (pushed to GitHub).**
> Only rebase your local, private feature branches.

### Rebase vs. Merge

* **Merge:** "True history." Shows exactly when branches split and rejoined. Safe. Result: A diamond shape in the graph.
* **Rebase:** "Linear history." Moves your work to the tip of the target branch. Destructive (rewrites commit hashes). Result: A straight line.

**Scenario:** `main` has moved forward while you were working on `feature`.

```bash
# 1. Switch to your feature branch
git switch feature

# 2. Rebase onto main (Move my work to start AFTER the latest main commit)
git rebase main

```

* If conflicts occur: Fix files  `git add`  `git rebase --continue`.

> **Visual Reference:** **`001 15 - Rebasing The Scariest Git Command.pdf`** (Page 2) shows the "messy" merge history vs. the "clean" rebase history.
<p align="center">
  <img src="./assets/ch5/11.png" width="93%" alt="Image 2"/>
</p>
---

## 2. Interactive Rebase: "Photoshop for Commits" (Module 16)

This is how you clean up your "Work in Progress" mess before merging.

**Command:**

```bash
# Edit the last 4 commits
git rebase -i HEAD~4

```

**The Menu:**
Git opens a text editor with a list of commits. Change the word `pick` to one of these:

* **`pick`**: Keep the commit as is.
* **`reword`**: Keep the code, but change the commit message.
* **`edit`**: Pause here to change the *content* of the commit (add/remove files).
* **`squash`**: Meld this commit into the previous one (combine 3 small commits into 1 big one).
* **`drop`**: Delete this commit entirely.

> **Visual Reference:** **`001 16 - Cleaning Up History...pdf`** (Page 24) shows the interactive editor interface where you select `pick`, `squash`, etc.
<p align="center">
  <img src="./assets/ch5/21.png" width="93%" alt="Image 2"/>
</p>
---

## 3. Tagging: Marking Releases (Module 17)

A **Branch** moves. A **Tag** stays still. Use tags for production releases (e.g., v1.0).

### Creating Tags

* **Lightweight Tag:** Just a bookmark.
```bash
git tag v1.0

```

* **Annotated Tag (Professional Standard):** Stores author, date, and message.
```bash
git tag -a v1.0 -m "First Public Release"

```

### Commom commands
```bash
git tag  # View Tag
git checkout <tag>  # Checkout Tag
git tag -d <tagname> # Delete Tag
```

### Pushing Tags

Tags are **not** pushed by default. You must be explicit.

```bash
# Push specific tag
git push origin v1.0

# Push ALL tags
git push origin --tags

```

> **Visual Reference:** **`001 17 - Git Tags...pdf`** (Pages 4-6) visualizes tags as permanent markers on specific commits, even as the branch moves forward.

<p align="center">
  <img src="./assets/ch5/21.png" width="49%" alt="Image 2"/>
  <img src="./assets/ch5/32.png" width="49%" alt="Image 2"/>
</p>

---

## 4. Git Internals: Hashing & Objects (Module 18)

Understanding this demystifies Git. It's not magic; it's a database.

### The Object Database (Key-Value Store)

Everything in Git is hashed using **SHA-1** (a 40-character string). The **Hash** is the Key; the **Data** is the Value.

### The 3 Main Objects

1. **Blob (Binary Large Object):** The content of a file. (Git doesn't store filenames here, just the text inside).
2. **Tree:** A directory listing. It maps filenames to Blobs (or other Trees).
3. **Commit:** A wrapper object. It points to a **Tree** (the root folder of your project) and adds metadata (Author, Parent Commit, Message).

> **Visual Reference:** **`001 18 - Git Behind The Scenes...pdf`** (Page 43) clearly diagrams the hierarchy: **Commit**  **Tree**  **Blob**.

<p align="center">
  <img src="./assets/ch5/41.png" width="49%" alt="Image 2"/>
  <img src="./assets/ch5/42.png" width="49%" alt="Image 2"/>
</p>

<p align="center">
  <img src="./assets/ch5/43.png" width="49%" alt="Image 2"/>
</p>

---

## 5. Reflogs: The Safety Net (Module 19)

If you deleted a branch or did a hard reset and lost code, **Reflog** is your backup.
Git records *every* time the HEAD pointer moves.

```bash
# View the movement history
git reflog
# Output:
# e4d909 HEAD@{0}: reset: moving to HEAD~1
# 8a2b12 HEAD@{1}: commit: add login feature

```

**Recovery:**
To go back to the state *before* you messed up (e.g., index 1):

```bash
git reset --hard HEAD@{1}

```

**Limitations:**
reflogs are kept on your local activity and not shared with collaborators. Reflogs also **expire**. Git cleans out old entries after **around 90 days**, though this can be configured 

> **Visual Reference:** **`001 19 - The Power of Reflogs...pdf`** (Page 9) shows the list of "breadcrumbs" Git leaves behind for every action.

<p align="center">
  <img src="./assets/ch5/51.png" width="49%" alt="Image 2"/>
  <img src="./assets/ch5/52.png" width="49%" alt="Image 2"/>
</p>

---

## 6. Custom Aliases (Module 20)

Type less, do more.

**Setting up Shortcuts:**

```bash
# 'git s' = 'git status'
git config --global alias.s status

# 'git cm' = 'git commit -m'
git config --global alias.cm "commit -m"

# 'git l' = A beautiful, colored graph log (The "Pro" Log)
git config --global alias.l "log --oneline --graph --decorate --all"

```

> **Visual Reference:** **`001 20 - Writing Custom Git Aliases.pdf`** (Page 3) shows the `.gitconfig` file structure where these aliases are saved.

<p align="center">
  <img src="./assets/ch5/61.png" width="49%" alt="Image 2"/>
  <img src="./assets/ch5/62.png" width="49%" alt="Image 2"/>
</p>

---

# Course Complete.

You now have the full content:

* **Chapter 1:** Basics (Setup, Add, Commit).
* **Chapter 2:** Branching & Merging (Switch, Merge, Conflicts).
* **Chapter 3:** The "Oh No" Toolkit (Diff, Stash, Reset, Revert).
* **Chapter 4:** Collaboration (Remotes, Fetch vs Pull, PRs).
* **Chapter 5:** Advanced (Rebase, Tags, Internals, Reflog).

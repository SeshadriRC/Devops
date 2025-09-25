Got it 👍 Let’s walk through step by step for your task on the `games` repo:

---

### 1. Go to the repo

```bash
cd /usr/src/kodekloudrepos/games
```

---

### 2. List all stashes

```bash
git stash list
```

You should see something like:

```
stash@{0}: WIP on master: ...
stash@{1}: WIP on feature: ...
```

---

### 3. Apply the required stash (`stash@{1}`)

```bash
git stash apply stash@{1}
```

🔹 This restores the changes but **keeps them in stash**.
If you want to restore **and remove it from stash**, use:

```bash
git stash pop stash@{1}
```

---

### 4. Stage the changes

```bash
git add .
```

---

### 5. Commit the changes

```bash
git commit -m "Restored changes from stash@{1}"
```

---

### 6. Push to origin

```bash
git push origin master
```

(or replace `master` with the current branch name, e.g. `main`).

---

✅ That’s it — stash restored, committed, and pushed.

Do you want me to also show you how to **verify which branch the stash belonged to** before applying it, so you don’t restore it on the wrong branch?


Good question 👍

We use **`git stash`** when we want to temporarily save our uncommitted changes (both staged and unstaged) **without committing them** — so we can switch branches, pull updates, or work on something else, and later restore those changes.

---

### 🔹 Main reasons to use `git stash`

1. **Switch branches safely**

   * Suppose you’re working on `feature1` but need to quickly fix a bug on `main`.
   * You don’t want to commit half-done work.
   * `git stash` saves your changes, you switch branch, fix the bug, then come back and restore the work.

2. **Keep working directory clean**

   * Sometimes you want to test something or pull the latest changes from remote.
   * Git requires a clean working tree.
   * You stash changes, update, then reapply them.

3. **Experimentation**

   * You try out an idea but don’t want to lose your current work.
   * Stash it, test your experiment, and restore if needed.

---

### 🔹 Example workflow

```bash
# Save current changes
git stash

# Switch branch
git checkout main

# Pull latest code
git pull origin main

# Switch back and restore changes
git checkout feature1
git stash pop
```

---

👉 In short:
`git stash` = **temporary storage for in-progress work** without making a commit.

Do you want me to also explain the **difference between `git stash save/apply/pop` and `git commit`** (since sometimes people confuse when to stash vs when to commit)?

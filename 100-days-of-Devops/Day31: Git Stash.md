The Nautilus application development team was working on a git repository /usr/src/kodekloudrepos/games present on Storage server in Stratos DC. 
One of the developers stashed some in-progress changes in this repository, but now they want to restore some of the stashed changes. 
Find below more details to accomplish this task:


Look for the stashed changes under /usr/src/kodekloudrepos/games git repository, and restore the stash with stash@{1} identifier. 
Further, commit and push your changes to the origin.

## Solution

```
git stash list
git stash apply stash@{1}
```
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

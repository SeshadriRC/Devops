In **Git**, a **cherry-pick** means taking a *specific commit* from one branch and applying it onto another branch, without merging the whole branch.

---

### Example:

* Suppose you have two branches:

  * `feature` branch has commits A → B → C.
  * `master` branch does not have these commits.

If you only want **commit B** from `feature` (not A and C), you use:

```bash
git cherry-pick <commit-hash-of-B>
```

This applies just that commit’s changes on top of your current branch (`master` in this case).

---

### Key Points:

* **`git merge`** → brings *all commits* from one branch.
* **`git cherry-pick`** → brings *only specific commits* you choose.
* Useful when:

  * You want to apply a hotfix from one branch into another.
  * You don’t want incomplete or experimental work from the whole branch.

---

👉 Think of it like **picking one cherry from a tree (branch)** instead of cutting down the whole tree.


## Practicals

Got it 👍. Here’s the step-by-step to **cherry-pick the commit with message `Update info.txt` from `feature` branch to `master`** and push it:

---

### Steps:

1. **Go to the repo directory**

```bash
cd /usr/src/kodekloudrepos
```

2. **Check branches**

```bash
git branch
```

(You should see `master` and `feature`.)

3. **Switch to master branch**

```bash
git checkout master
```

4. **Find the commit hash in `feature` branch**

```bash
git log feature --oneline
```

Look for the commit with message:

```
<commit-hash> Update info.txt
```

5. **Cherry-pick that commit into master**

```bash
git cherry-pick <commit-hash>
```

6. **Verify commit is applied**

```bash
git log --oneline
```

You should see the `Update info.txt` commit on top of `master`.

7. **Push the changes to origin**

```bash
git push origin master
```

---

✅ That will merge **only that single commit** into `master` without merging the entire `feature` branch.

Do you want me to also give you the **exact command sequence** assuming the commit hash is, say, `abc1234`?


Would you like me to also explain with a **small real Git command demo** (with two branches and one commit cherry-picked)?


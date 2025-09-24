The Nautilus application development team was working on a git repository /usr/src/kodekloudrepos/apps present on Storage server in Stratos DC. This was just a test repository and one of the developers just pushed a couple of changes for testing, but now they want to clean this repository along with the commit history/work tree, so they want to point back the HEAD and the branch itself to a commit with message add data.txt file. Find below more details:



In /usr/src/kodekloudrepos/apps git repository, reset the git commit history so that there are only two commits in the commit history i.e initial commit and add data.txt file.


Also make sure to push your changes.

## Solution

```
git reset --hard <commit-hash>
git push origin master --force
git log --oneline

```



Got it 👍 You need to **reset the repo so that only two commits remain**:  

1. `initial commit`  
2. `add data.txt file`  

Right now you have many commits on top of `add data.txt file`. Here’s how you can clean it up:  

---

### Steps
```bash
# Go to repo
cd /usr/src/kodekloudrepos/apps

# Make a new branch pointer that resets to the "add data.txt file" commit
git reset --hard 4598ab7dd17b2bb3fb757ef1465c52def1c13392

# Force push so remote history is also reset
git push origin master --force
```

---

### Verify
```bash
git log --oneline
```
You should now see only:
```
4598ab7 add data.txt file
705ee2b initial commit
```

And if you check remote:
```bash
git fetch origin
git log origin/master --oneline
```
It should match the local.

---

✅ That keeps only the **two required commits** and removes the rest from history.


Good question 👍 let me break it down.

When you run `git log`, Git shows you commits and sometimes **extra labels in parentheses** after the commit hash.

---

### Example 1

```
commit 4598ab7dd17b2bb3fb757ef1465c52def1c13392 (HEAD -> master)
```

* `4598ab7d...` → commit hash.
* `HEAD -> master` → means your **HEAD (current checked-out commit)** is pointing to the branch `master`, and that branch is pointing to this commit.
* So: both you and your local `master` branch are at this commit.

---

### Example 2

```
commit d431eb248528e7c7e9115ed5ceb2abf770030193 (HEAD -> master, origin/master)
```

* `HEAD -> master` → same as above, your local `master` branch is pointing here.
* `origin/master` → the **remote tracking branch** (on `origin`) is also pointing to the same commit.
* So: your local branch `master` and the remote branch `origin/master` are in sync at this commit.

---

🔑 **Difference**

* If you see only `(HEAD -> master)` → your local branch points there, but remote might be elsewhere (behind/ahead).
* If you see `(HEAD -> master, origin/master)` → local and remote are exactly aligned at this commit.

---

Do you want me to also show what it would look like **if your local and remote were out of sync** (e.g., after `reset --hard` but before `git push --force`)?


That command is asking Git:

👉 **“Show me the commit history of the branch `origin/master` (the remote-tracking branch), not my local `master`.”**

---

### Key points

* `master` → your **local branch**.
* `origin/master` → Git’s local reference (a *remote-tracking branch*) that records where the remote repository’s `master` branch was the last time you did a `git fetch` or `git pull`.

So:

```bash
git log master --oneline
```

Shows commits your **local branch** points to.

```bash
git log origin/master --oneline
```

Shows commits your **remote tracking branch** points to (what Git thinks is on the remote server).

---

### Example scenario

1. You do a `git reset --hard` locally → your `master` moves backward.
2. Remote still has the old history.

Now:

```bash
git log master --oneline
```

will show fewer commits.

```bash
git log origin/master --oneline
```

will still show the old (longer) history.

That’s why after a rewrite (like `reset --hard`) you must do:

```bash
git push origin master --force
```

so that `origin/master` and `master` match again.

---

⚡In short:
`git log origin/master --oneline` = “Show me the commits that exist on the remote branch (as Git last fetched them).”

Do you want me to show you how `git log master --oneline` vs `git log origin/master --oneline` will differ *immediately after you reset but before you force push*?


Do you want me to also give you the exact `git log --oneline` output you should expect after the reset, so you can quickly verify?


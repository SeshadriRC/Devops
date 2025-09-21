The Nautilus application development team was working on a git repository /usr/src/kodekloudrepos/demo present on Storage server in Stratos DC. However, they reported an issue with the recent commits being pushed to this repo. They have asked the DevOps team to revert repo HEAD to last commit. Below are more details about the task:


In /usr/src/kodekloudrepos/demo git repository, revert the latest commit ( HEAD ) to the previous commit (JFYI the previous commit hash should be with initial commit message ).


Use revert demo message (please use all small letters for commit message) for the new revert commit.

## Solution

https://www.nbtechsupport.co.in/2021/07/git-revert-some-changes.html

```
git log
git revert HEAD
git add .
git commit -m "revert demo"

```

`git revert HEAD`

This command creates a **new commit** that undoes the changes made by the last commit (the one currently pointed to by `HEAD`).

* `HEAD` → refers to the most recent commit in your current branch.
* `revert` → instead of deleting history, it **adds a new commit** that applies the reverse of the changes.

🔹 Example:

* If your last commit added a line to a file, `git revert HEAD` will create a new commit that removes that line.
* History remains intact — unlike `git reset`, which rewrites history.

👉 Useful when you want to undo a commit but **keep history clean and visible**.

Do you want me to also explain the difference between `git revert HEAD` and `git reset HEAD~1` (since they’re often confused)?

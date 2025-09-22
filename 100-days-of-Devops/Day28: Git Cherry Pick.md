The Nautilus application development team has been working on a project repository /opt/blog.git. This repo is cloned at /usr/src/kodekloudrepos on storage server in Stratos DC. They recently shared the following requirements with the DevOps team:



There are two branches in this repository, master and feature. One of the developers is working on the feature branch and their work is still in progress, however they want to merge one of the commits from the feature branch to the master branch, the message for the commit that needs to be merged into master is Update info.txt. Accomplish this task for them, also remember to push your changes eventually.

## Solution

```
git checkout master
git log feature --oneline
git cherry-pick 8beae92

```


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

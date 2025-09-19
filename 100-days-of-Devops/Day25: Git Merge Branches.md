The Nautilus application development team has been working on a project repository /opt/demo.git. This repo is cloned at /usr/src/kodekloudrepos on storage server in Stratos DC. They recently shared the following requirements with DevOps team:



Create a new branch nautilus in /usr/src/kodekloudrepos/demo repo from master and copy the /tmp/index.html file (present on storage server itself) into the repo. Further, add/commit this file in the new branch and merge back that branch into master branch. Finally, push the changes to the origin for both of the branches.

## Solution

- https://www.nbtechsupport.co.in/2021/07/git-merge-branches.html

```
cd /usr/src/kodekloudrepos/demo
git checkout -b nautilus
cp /tmp/index.html .
git add .
git commit -m "index"
git push origin
git checkout master
git merge nautilus
git push -u origin nautilus
git push -u origin master
```


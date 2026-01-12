1. As developers are currently in the process of creating Git repositories, a new requirement has emerged to create a bare repository on the storage server. Below are the details for this request.


Create a bare repository named /home/sarah/apps.git on Storage server.

Use below credentials to SSH into the storage server and to complete this task.

Username: sarah
Password: S3cure321

## Solution

To create a **bare Git repository** on the **Storage server**, follow these steps:



## 1️⃣ SSH into the Storage Server

```bash
ssh sarah@<storage_server_ip_or_hostname>
```

When prompted, enter the password:

```text
S3cure321
```



## 2️⃣ Create the bare repository

Run the following commands:

```bash
mkdir -p /home/sarah/apps.git
cd /home/sarah/apps.git

git init --bare


[sarah@ststor01 apps.git]$ git init --bare
hint: Using 'master' as the name for the initial branch. This default branch name
hint: is subject to change. To configure the initial branch name to use in all
hint: of your new repositories, which will suppress this warning, call:
hint: 
hint:   git config --global init.defaultBranch <name>
hint: 
hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
hint: 'development'. The just-created branch can be renamed via this command:
hint: 
hint:   git branch -m <name>

[sarah@ststor01 apps.git]$ ls -a
.  ..  HEAD  branches  config  description  hooks  info  objects  refs

```



## 3️⃣ Verify the bare repository

```bash
ls
```

You should see files like:

```text
HEAD
branches
config
description
hooks
info
objects
refs
```

This confirms it is a **bare repository** (no working tree).



## Key point (important for understanding)

* A **bare repository** is used as a **central/shared repo**
* It does **not** contain working files
* Developers will **clone** this repo, not work directly inside it



### ✅ Task completed

Bare repository successfully created at:

```text
/home/sarah/apps.git
```

If you want, I can also show:

* How developers can clone this repo
* Difference between bare and non-bare repo (interview-focused)
* How to set permissions for team access

---

2. Commit the change of file lion-and-mouse-t1q6.txt which is placed under /home/sarah/story-blog-t1q6 repository on Storage server, file is already staged. A commit records the change in the repository compared to its previous state. But before that we must configure the git user who will be the owner of the commit.


Set git username as sarah and user email as sarah@example.com using the below commands.

git config user.email sarah@example.com
git config user.name sarah

Use below credentials to SSH into the storage server and to complete this task.

Username: sarah
Password: S3cure321

# Solution

```
check the existing config

[sarah@ststor01 story-blog-t1q6]$ git config --list
user.email=sarah@stratos.xfusioncorp.com
user.name=sarah
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true


[sarah@ststor01 story-blog-t1q6]$ git config user.email sarah@example.com
[sarah@ststor01 story-blog-t1q6]$ git config user.name sarah

[sarah@ststor01 story-blog-t1q6]$ git config --list
user.email=sarah@stratos.xfusioncorp.com
user.name=sarah
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
user.email=sarah@example.com
user.name=sarah


[sarah@ststor01 story-blog-t1q6]$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   lion-and-mouse-t1q6.txt


[sarah@ststor01 story-blog-t1q6]$ git commit -m "lion-and-mouse-2nd-ques"
[master (root-commit) 91f37b3] lion-and-mouse-2nd-ques
 1 file changed, 1 insertion(+)
 create mode 100644 lion-and-mouse-t1q6.txt
[sarah@ststor01 story-blog-t1q6]$ git status
On branch master
nothing to commit, working tree clean
[sarah@ststor01 story-blog-t1q6]$ 
```


---
3. Recently a bare repository was created by one of the developers. Now, they were planning to add some content in this repository so this needs to be cloned somewhere so that one of the developers can start adding data in it. Below you can find more details.


Clone the repository /opt/story-blog-t1q11.git under sarah user's home on storage server.

Use below credentials to SSH into the storage server and to complete this task.

Username: sarah
Password: S3cure321


# Solution

```
cd /home/sarah
git clone /opt/story-blog-t1q11.git
```

---
4. Nautilus developers are actively working on one of the project repositories, /usr/src/kodekloudrepos/apps-t2q1. Recently, they decided to implement some new features in the application, and they want to maintain those new changes in a separate branch. Below are the requirements that have been shared with the DevOps team:


On Storage server in Stratos DC create a new branch xfusioncorp_apps-t2q1 from master branch in /usr/src/kodekloudrepos/apps-t2q1 git repo.

Please do not try to make any changes in the code.

Use below credentials to SSH into the storage server and to complete this task.

Username: sarah
Password: S3cure321

# Solution
```
[sarah@ststor01 apps-t2q1]$ git branch
* master
[sarah@ststor01 apps-t2q1]$ pwd
/usr/src/kodekloudrepos/apps-t2q1
[sarah@ststor01 apps-t2q1]$ git checkout -b xfusioncorp_apps-t2q1
Switched to a new branch 'xfusioncorp_apps-t2q1'
[sarah@ststor01 apps-t2q1]$ git branch
  master
* xfusioncorp_apps-t2q1
```
5. The Nautilus application development team has been working on a project repository /opt/apps-t2q3.git. This repo is cloned at /usr/src/kodekloudrepos/apps-t2q3 on storage server in Stratos DC. They recently shared the following requirements with DevOps team:


Create a new branch nautilus-t2q3 in /usr/src/kodekloudrepos/apps-t2q3 repo from master and copy the /tmp/index-t2q3.html file (present on storage server itself) into the repo. Further, add/commit this file in the new branch and merge back that branch into master branch. Finally, push the changes to the origin for both of the branches.

Use below credentials to SSH into the storage server and to complete this task.

# Solution

```

[sarah@ststor01 apps-t2q1]$ cd /usr/src/kodekloudrepos/apps-t2q3

[sarah@ststor01 apps-t2q3]$ git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
[sarah@ststor01 apps-t2q3]$ git checkout -b nautilus-t2q3
Switched to a new branch 'nautilus-t2q3'

[sarah@ststor01 apps-t2q3]$ ls
info-t2q3.txt  welcome-t2q3.txt

[sarah@ststor01 apps-t2q3]$ git status
On branch nautilus-t2q3
nothing to commit, working tree clean
[sarah@ststor01 apps-t2q3]$

[sarah@ststor01 apps-t2q3]$ git branch
  master
* nautilus-t2q3

[sarah@ststor01 apps-t2q3]$ cp /tmp/index-t2q3.html /usr/src/kodekloudrepos/apps-t2q3
[sarah@ststor01 apps-t2q3]$ ls
index-t2q3.html  info-t2q3.txt  welcome-t2q3.txt

[sarah@ststor01 apps-t2q3]$ git status
On branch nautilus-t2q3
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        index-t2q3.html

nothing added to commit but untracked files present (use "git add" to track)


[sarah@ststor01 apps-t2q3]$ git add .
[sarah@ststor01 apps-t2q3]$ git commit -m "committed index file"
[nautilus-t2q3 c71ce73] committed index file
 1 file changed, 1 insertion(+)
 create mode 100644 index-t2q3.html
[sarah@ststor01 apps-t2q3]$ git status
On branch nautilus-t2q3
nothing to commit, working tree clean

Your branch is up to date with 'origin/master'.
[sarah@ststor01 apps-t2q3]$ git log
commit 67dc131d3055961a5ef1e75657bc25097216ed76 (HEAD -> master, origin/master)
Author: Admin <admin@kodekloud.com>
Date:   Mon Jan 12 13:25:51 2026 +0000

    initial commit
[sarah@ststor01 apps-t2q3]$ date
Mon Jan 12 13:54:46 UTC 2026
[sarah@ststor01 apps-t2q3]$ git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean

[sarah@ststor01 apps-t2q3]$ git merge nautilus-t2q3
Updating 67dc131..c71ce73
Fast-forward
 index-t2q3.html | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 index-t2q3.html

[sarah@ststor01 apps-t2q3]$ git merge nautilus-t2q3
Updating 67dc131..c71ce73
Fast-forward
 index-t2q3.html | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 index-t2q3.html


[sarah@ststor01 apps-t2q3]$ git remote -v
origin  /opt/apps-t2q3.git (fetch)
origin  /opt/apps-t2q3.git (push)
[sarah@ststor01 apps-t2q3]$ git branch
* master
  nautilus-t2q3
[sarah@ststor01 apps-t2q3]$ git push
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 16 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 345 bytes | 345.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To /opt/apps-t2q3.git
   67dc131..c71ce73  master -> master
[sarah@ststor01 apps-t2q3]$ git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean

[sarah@ststor01 apps-t2q3]$ git push origin nautilus-t2q3
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
To /opt/apps-t2q3.git
 * [new branch]      nautilus-t2q3 -> nautilus-t2q3


[sarah@ststor01 apps-t2q3]$ git push origin master
Everything up-to-date
[sarah@ststor01 apps-t2q3]$ git push origin nautilus-t2q3
Everything up-to-date



```

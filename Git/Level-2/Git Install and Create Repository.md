The Nautilus development team shared with the DevOps team requirements for new application development, setting up a Git repository for that project. Create a Git repository on Storage server in Stratos DC as per details given below:



Install git package using yum on Storage server.


After that, create/init a git repository named /opt/ecommerce.git (use the exact name as asked and make sure not to create a bare repository).

## Solution

Follow these steps on the **Storage Server in Stratos DC**.

---

## 1️⃣ SSH into the Storage Server

```bash
ssh <user>@storage-server
```

---

## 2️⃣ Install Git using yum

```bash
sudo su -
sudo yum install -y git
```

Verify installation:

```bash
git --version
```


---

## 4️⃣ Initialize a **non-bare** Git repository

⚠️ Important: **Do NOT use `--bare`**

```bash
cd /opt/ecommerce.git
git init
```

---

## 5️⃣ Verify it is a normal (non-bare) repository

```bash
ls -a
```

You should see:

```
.git
```

✅ Presence of `.git` directory confirms this is **NOT a bare repository**.

---

## ✅ Final Result

* Git installed on Storage Server
* Repository created at **/opt/ecommerce.git**
* Repository is **non-bare**, as required

If you want, I can also show:

* Difference between **bare vs non-bare**
* How to push code to this repo from another server


**Myoutput**

```
sudo yum install -y git
sudo mkdir -p /opt/ecommerce.git

[natasha@ststor01 ecommerce.git]$ git init
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
Initialized empty Git repository in /opt/ecommerce.git/.git/


[natasha@ststor01 ecommerce.git]$ git status
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)
[natasha@ststor01 ecommerce.git]$ ls -a
.  ..  .git

```

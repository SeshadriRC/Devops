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
sudo yum install -y git
```

Verify installation:

```bash
git --version
```

---

## 3️⃣ Create the repository directory

```bash
sudo mkdir -p /opt/ecommerce.git
```

(Optional: ensure correct permissions)

```bash
sudo chown -R $(whoami):$(whoami) /opt/ecommerce.git
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

```

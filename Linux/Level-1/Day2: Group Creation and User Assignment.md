The system admin team at xFusionCorp Industries has streamlined access management by implementing group-based access control. Here's what you need to do:



a. Create a group named nautilus_developers across all App servers within the Stratos Datacenter.


b. Add the user jarod into the nautilus_developers group on all App servers. If the user doesn't exist, create it as well.

Note: You can find the infrastructure details by clicking on the Details of all Users and Servers button on the top-right section of the page.

## Solution

```
sudo groupadd nautilus_developers
[tony@stapp01 ~]$ cat /etc/group | grep nautilus_developers
nautilus_developers:x:1002:

sudo useradd jarod
[tony@stapp01 ~]$ cat /etc/passwd | grep jarod
jarod:x:1002:1003::/home/jarod:/bin/bash

sudo usermod -aG nautilus_developers jarod
```

This is a **Linux user & group management task** and must be done **on all App servers** in the Stratos Datacenter.

Below are **clear, safe steps** you can follow on **each App server**.

---

## 🔹 Step 1: Login to each App server

SSH into every App server one by one (example):

```bash
ssh tony@stapp01
ssh tony@stapp02
ssh tony@stapp03
```

(Use the actual usernames/hosts shown in the **Details of all Users and Servers** section.)

---

## 🔹 Step 2: Create the group `nautilus_developers`

Run on **each App server**:

```bash
sudo groupadd nautilus_developers
```

If the group already exists, you’ll see:

```
groupadd: group 'nautilus_developers' already exists
```

👉 That’s fine, you can continue.

---

## 🔹 Step 3: Check if user `jarod` exists

```bash
id jarod
```

### If user exists

You’ll see output like:

```
uid=1002(jarod) gid=1002(jarod) groups=1002(jarod)
```

### If user does NOT exist

Create it:

```bash
sudo useradd jarod
```

(Optional but safe to set password if required)

```bash
sudo passwd jarod
```

---

## 🔹 Step 4: Add `jarod` to `nautilus_developers` group

Run on **each App server**:

```bash
sudo usermod -aG nautilus_developers jarod
```

⚠️ **Important**:

* `-a` → append (do NOT remove existing groups)
* `-G` → secondary group

---

## 🔹 Step 5: Verify group membership

```bash
id jarod
```

Expected output should include:

```
groups=...,nautilus_developers
```

Or:

```bash
getent group nautilus_developers
```

Example:

```
nautilus_developers:x:1005:jarod
```

---

## ✅ Final Checklist (must be true on ALL App servers)

✔ Group `nautilus_developers` exists
✔ User `jarod` exists
✔ `jarod` is a member of `nautilus_developers`

---

If you want, I can also give:

* A **single script** to run on all servers
* A **one-liner command**
* A **verification-only command** (useful for exam/lab checks)

---

This line is the **user account entry for `jarod`** from `/etc/passwd`.

```
jarod:x:1002:1003::/home/jarod:/bin/bash
```

Let’s break it down **field by field** (very important for Linux admin basics):

---

## `/etc/passwd` format

Each line has **7 colon-separated fields**:

```
username:password:UID:GID:comment:home_directory:login_shell
```

---

## Field-by-field explanation for `jarod`

| Field              | Value         | Meaning                                        |
| ------------------ | ------------- | ---------------------------------------------- |
| **username**       | `jarod`       | Login name                                     |
| **password**       | `x`           | Password is stored in `/etc/shadow` (not here) |
| **UID**            | `1002`        | User ID of `jarod`                             |
| **GID**            | `1003`        | **Primary group ID**                           |
| **comment**        | *(empty)*     | GECOS / description field                      |
| **home directory** | `/home/jarod` | User’s home directory                          |
| **shell**          | `/bin/bash`   | Default login shell                            |

---

## Important observation

👉 **Primary group is NOT `jarod`** (usually GID matches UID)

Check what GID `1003` maps to:

```bash
getent group 1003
```

You’ll likely see something like:

```
nautilus_developers:x:1003:jarod
```

This means:

* `nautilus_developers` is the **primary group** of `jarod`
* Not just a secondary group

---

## Why this matters

* Primary group controls:

  * Default file group ownership
  * Permissions on newly created files
* Secondary groups come from:

```bash
id jarod
```

---

## Quick verification commands

```bash
id jarod
groups jarod
getent passwd jarod
getent group nautilus_developers
```

---

## In one line

👉 This output confirms **user `jarod` exists**, has **UID 1002**, uses **bash**, and belongs primarily to **group ID 1003**.

If you want, I can explain:

* Difference between **primary vs secondary group**
* Why `/etc/passwd` shows `x`
* How this differs from `/etc/shadow`

---

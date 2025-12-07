In response to heightened security concerns, the xFusionCorp Industries security team has opted for custom Apache users for their web applications. Each user is tailored specifically for an application, enhancing security measures. Your task is to create a custom Apache user according to the outlined specifications:



a. Create a user named siva on App server 3 within the Stratos Datacenter.


b. Assign a unique UID 1013 and designate the home directory as /var/www/siva.

Note: You can find the infrastructure details by clicking on the Details of all Users and Servers button on the top-right section of the page.

## Solution

```
useradd -u 1013 -d /var/www/siva -m siva
```

Below is the exact set of commands you need to run on **App Server 3** to create the custom Apache user as required:

---

# ✅ **Step 1: Create user `siva` with UID 1013 and custom home directory**

Run this command on **App Server 3**:

```
sudo useradd -u 1013 -d /var/www/siva -m siva
```

### Explanation:

* `-u 1013` → assigns UID 1013
* `-d /var/www/siva` → sets custom home directory
* `-m` → creates the home directory if not present
* `siva` → username

---

# ✅ **Step 2: Verify user creation**

```
id siva
```

You should see UID 1013.

---

# ✅ **Step 3: Verify home directory**

```
ls -ld /var/www/siva
```

It must exist and be owned by **siva**.

---

If you want, I can explain why custom Apache users are created for applications or how they improve security.



Here’s a clear explanation in **interview-style**, simple and practical:

---

# ✅ **Why create custom Apache users for applications?**

Creating a **separate Linux user for each web application** is a strong security practice. Here’s why:

### **1. Process isolation**

Each application's files and processes run under **its own user** instead of the default `apache` or `www-data`.

➡️ If one application is compromised, the attacker **cannot access files of another app**, because they belong to different users.

---

### **2. Least privilege principle**

Each app user gets **only the permissions it needs**—nothing more.

➡️ Reduces risk of accidental or malicious modifications to system files.

---

### **3. Better access control**

You can assign file permissions, directories, and service access **per user**.

➡️ Makes troubleshooting & auditing easier.

---

### **4. Prevents privilege escalation**

If attackers access an app, they get the privileges of **only that app's user**.

➡️ They cannot gain access to other apps or core OS components.

---

### **5. Easier maintenance**

You can:

* rotate permissions
* isolate resource usage
* track logs
  per application user.

---

# 🔑 **In short (interview version):**

> “Custom Apache users isolate each application at the OS level. If one app is hacked, the attacker cannot touch others. It enforces least privilege, improves security, and simplifies maintenance.”

---

# ✅ **What is UID in Linux?**

**UID stands for User Identifier**.

It is a **unique number assigned to each user** in Linux.

Example of a user entry:

```
siva:x:1013:1013::/var/www/siva:/bin/bash
```

Here, **1013** is the UID.

### Why UID is important?

1. Linux internally identifies users by UID, not by username.
2. File permissions and ownership (`ls -l`) are tied to UID.
3. You can change the username, but UID remains same, so permissions are preserved.
4. System users normally have UIDs < 1000 (varies by distro).
5. Application users are usually given custom UIDs (like 1013 in your case).

---

# 🔑 **Interview-ready definition:**

> “UID is a numeric identifier for each Linux user. The OS uses UID for permission checks and file ownership. Even if you rename a user, its UID stays the same and keeps its access intact.”

---

If you want, I can also explain **GID**, `/etc/passwd`, `/etc/group`, or how Apache typically uses user accounts.



A **home directory** in Linux is the *personal workspace* for a user — a dedicated folder where that user’s files, settings, and configurations are stored.

---

# ✅ **Simple Definition**

A **home directory** is the default directory a user is placed in after logging in.
Each user gets their own home directory so their files stay separate.

Examples:

* User **root** → `/root`
* Normal users → `/home/username`
* Custom app user (your case) → `/var/www/siva`

---

# 🔍 What does a home directory contain?

A home directory typically stores:

* User-specific files
* Configuration files (hidden “dotfiles” like `.bashrc`, `.profile`)
* Application-related data owned by that user
* Custom scripts
* SSH keys (under `~/.ssh/`)

---

# 🎯 Why do we need a home directory?

Because it provides:

### **1. Isolation**

Each user gets private storage space.

### **2. Default login location**

When the user logs in, they start in their home directory:

```
cd ~
```

`~` always means the user's home directory.

### **3. Permissions**

Only that user can access their home directory (unless permissions are changed).

### **4. Application users**

For application-specific users (like Apache users), the home directory may store:

* website data
* configuration files
* logs
* environment files

Example:
`/var/www/siva` would store files specifically for the `siva` application.

---

# 🧠 Interview-ready answer:

> “A home directory is the default working directory assigned to a Linux user. It stores that user’s files and configuration settings. Each user has their own home directory to keep data isolated and secure.”

---

If you want, I can explain the difference between **home directory vs root directory**, or help you check or set a user's home directory.



Here's a clear explanation of the line you posted:

---

# ✅ **What this line means**

```
sesha:x:1014:1014::/home/sesha:/bin/bash
```

This line comes from `/etc/passwd`, which stores **user account information**.

Each field is separated by a colon (`:`).
Let’s break it down:

---

# 🧩 **Field-by-field Explanation**

| Field | Value         | Meaning                                                      |
| ----- | ------------- | ------------------------------------------------------------ |
| 1     | `sesha`       | **Username**                                                 |
| 2     | `x`           | Password placeholder — real password stored in `/etc/shadow` |
| 3     | `1014`        | **UID** — User ID number                                     |
| 4     | `1014`        | **GID** — Primary Group ID                                   |
| 5     | *(empty)*     | User description / comment field (a.k.a. GECOS)              |
| 6     | `/home/sesha` | **Home directory**                                           |
| 7     | `/bin/bash`   | **Login shell** used when this user logs in                  |

---

# 🔍 **Simplified Meaning**

This tells us:

* A Linux user named **sesha** exists
* Their UID (User ID) is **1014**
* Their primary group ID is also **1014**
* Their home directory is **/home/sesha**
* When they log in, they get a Bash shell (`/bin/bash`)

---

# 📝 Why is this important?

### **1. UID**

Used internally by Linux—permissions rely on UID, not username.

### **2. GID**

Defines which group’s permissions the user inherits.

### **3. Home Directory**

User’s personal workspace.

### **4. Shell**

Defines what command-line environment they get on login.

---

# 🧠 Interview-ready explanation:

> "Every user on Linux has an entry in `/etc/passwd`, which includes their username, UID, GID, home directory, and login shell. The password is not stored here but in `/etc/shadow` for security."

---

If you'd like, I can also explain `/etc/shadow`, `/etc/group`, or how to create/edit such users.



Here's a clear explanation of the line you posted:

---

# ✅ **What this line means**

```
sesha:x:1014:1014::/home/sesha:/bin/bash
```

This line comes from `/etc/passwd`, which stores **user account information**.

Each field is separated by a colon (`:`).
Let’s break it down:

---

# 🧩 **Field-by-field Explanation**

| Field | Value         | Meaning                                                      |
| ----- | ------------- | ------------------------------------------------------------ |
| 1     | `sesha`       | **Username**                                                 |
| 2     | `x`           | Password placeholder — real password stored in `/etc/shadow` |
| 3     | `1014`        | **UID** — User ID number                                     |
| 4     | `1014`        | **GID** — Primary Group ID                                   |
| 5     | *(empty)*     | User description / comment field (a.k.a. GECOS)              |
| 6     | `/home/sesha` | **Home directory**                                           |
| 7     | `/bin/bash`   | **Login shell** used when this user logs in                  |

---

# 🔍 **Simplified Meaning**

This tells us:

* A Linux user named **sesha** exists
* Their UID (User ID) is **1014**
* Their primary group ID is also **1014**
* Their home directory is **/home/sesha**
* When they log in, they get a Bash shell (`/bin/bash`)

---

# 📝 Why is this important?

### **1. UID**

Used internally by Linux—permissions rely on UID, not username.

### **2. GID**

Defines which group’s permissions the user inherits.

### **3. Home Directory**

User’s personal workspace.

### **4. Shell**

Defines what command-line environment they get on login.

---

# 🧠 Interview-ready explanation:

> "Every user on Linux has an entry in `/etc/passwd`, which includes their username, UID, GID, home directory, and login shell. The password is not stored here but in `/etc/shadow` for security."

---

If you'd like, I can also explain `/etc/shadow`, `/etc/group`, or how to create/edit such users.



Great — let’s go deeper into **groups**, **GID**, and how Linux uses them for permissions.
Here’s a clean and simple explanation.

---

# ✅ **1. Primary Group vs Secondary Groups**

### **Primary Group**

* This is the group linked to the user in `/etc/passwd`.
* It has **one GID**.
* Every new file created by the user normally gets this group.

Example from your entry:

```
sesha:x:1014:1014::/home/sesha:/bin/bash
```

* Primary GID = **1014**
* Primary group name normally = **sesha**

### **Secondary Groups**

* Extra groups assigned to the user.
* Stored in `/etc/group`.

Check secondary groups:

```
id sesha
```

Example:

```
uid=1014(sesha) gid=1014(sesha) groups=1014(sesha),10(wheel)
```

Here:

* Secondary group = **wheel**

---

# ✅ **2. How to Check Group Information**

### **Check all groups of a user**

```
id <username>
```

### **Check group file**

```
cat /etc/group | grep sesha
```

Example:

```
wheel:x:10:root,sesha
```

### **Check group by GID**

```
getent group 1014
```

---

# ✅ **3. Adding & Removing Users from Groups**

### **Add user to a secondary group**

```
sudo usermod -aG wheel sesha
```

### **Remove user from a group**

(You must reassign groups completely)

```
sudo usermod -G <comma-separated-group-list> sesha
```

Example:

```
sudo usermod -G sesha sesha
```

(This removes all other secondary groups)

---

# ✅ **4. How UID and GID Affect Permissions**

Every file in Linux has:

* **Owner (UID)**
* **Group (GID)**
* **Other**

Check with:

```
ls -l
```

Example:

```
-rw-r--r-- 1 sesha sesha 200 Jan 10 10:00 data.txt
```

Breakdown:

* Owner: **sesha** (UID 1014)
* Group: **sesha** (GID 1014)
* Permissions:

  * Owner → rw
  * Group → r
  * Others → r

If a user is in the same group as the file’s GID, they get the **group permissions**.

---

# ✅ **5. How to Change Ownership & Groups**

### Change owner:

```
sudo chown newuser file.txt
```

### Change group:

```
sudo chown :wheel file.txt
```

### Change both:

```
sudo chown sesha:wheel file.txt
```

---

# If you want, I can explain next:

### 🔹 What is `/etc/shadow`?

### 🔹 How password hashing works?

### 🔹 File permissions (`rwx`) deeply explained?

### 🔹 ACLs (Advanced permissions) for users & groups?

Tell me what you want next.


This message means **your user `sesha` does NOT have sudo privileges**, so you cannot switch to root using `sudo su -`.

Let me break down everything you are seeing:

---

# ✅ **1. Why you see this message**

```
sesha is not in the sudoers file. This incident will be reported.
```

Linux checks `/etc/sudoers` and `/etc/group` to see if the user has permission to run `sudo`.

Your user **is not part of any sudo-privileged group** like:

* `wheel` (CentOS/RHEL)
* `sudo` (Ubuntu/Debian)

So sudo is denied.

---

# ✅ **2. How to give `sesha` sudo access (3 methods)**

⚠️ You must have root access first.

### **Method 1: Add user to wheel group (CentOS/RHEL)**

On App Server:

```
sudo usermod -aG wheel sesha
```

Verify:

```
id sesha
```

You should see:

```
groups=1014(sesha),10(wheel)
```

Now test:

```
su - sesha
sudo su -
```

---

### **Method 2: Add entry directly into sudoers**

```
visudo
```

Add this line:

```
sesha  ALL=(ALL)       NOPASSWD: ALL
```

Or password-based:

```
sesha  ALL=(ALL)       ALL
```

---

### **Method 3: Create a sudoers file under /etc/sudoers.d**

```
echo "sesha ALL=(ALL) ALL" > /etc/sudoers.d/sesha
chmod 440 /etc/sudoers.d/sesha
```

---

# ✅ **3. Why it says: “This incident will be reported”**

This simply means:

* The failed sudo attempt is logged in `/var/log/secure`
* It is NOT actually “reported” anywhere outside your system.

Check logs:

```
sudo tail -f /var/log/secure
```

---

# ✅ **4. Why you saw the “lecture” message**

```
We trust you have received the usual lecture...
```

This message appears the **first time a user runs sudo** (per user basis).

It’s a normal reminder about safe usage of sudo.

---

# If you want, I can show you:

* How to list users with sudo access
* How to remove sudo access
* How to safely edit sudoers without breaking system

Want me to explain those?


To change the **primary group** of a user in Linux, you need to modify the user’s **GID (primary group ID)**.

Let me show you the steps.

---

# ✅ **1. Check current primary group**

You already saw:

```
-rw-r--r-- 1 sesha sesha 0 Dec  7 03:18 example.txt
```

The second `sesha` means:

* **User:** sesha
* **Group:** sesha (primary group)

Check via command:

```
id sesha
```

Output example:

```
uid=1014(sesha) gid=1014(sesha) groups=1014(sesha)
```

---

# ✅ **2. To change the primary group name**

### ✔️ **Step 1: Create the new group**

Example: you want primary group → `developers`

```
sudo groupadd developers
```

### ✔️ **Step 2: Change user’s primary group**

```
sudo usermod -g developers sesha
```

Now verify:

```
id sesha
```

Expected:

```
uid=1014(sesha) gid=1002(developers) groups=1002(developers)
```

---

# ⛔ Important:

If you want to **rename** the user's existing group instead of assigning a new one:

### ✔️ Rename group

```
sudo groupmod -n newgroupname oldgroupname
```

Example:

```
sudo groupmod -n ops sesha
```

Now check:

```
id sesha
```

---

# ✅ **3. Check file ownership after change**

Files created earlier still show old group unless you update them:

```
sudo chown -R sesha:newgroupname /home/sesha
```

Example:

```
sudo chown -R sesha:developers /home/sesha
```

---

# If you tell me the exact **new group name** you want, I’ll give you the exact command.


To change the **primary group** of a user in Linux, you need to modify the user’s **GID (primary group ID)**.

Let me show you the steps.

---

# ✅ **1. Check current primary group**

You already saw:

```
-rw-r--r-- 1 sesha sesha 0 Dec  7 03:18 example.txt
```

The second `sesha` means:

* **User:** sesha
* **Group:** sesha (primary group)

Check via command:

```
id sesha
```

Output example:

```
uid=1014(sesha) gid=1014(sesha) groups=1014(sesha)
```

---

# ✅ **2. To change the primary group name**

### ✔️ **Step 1: Create the new group**

Example: you want primary group → `developers`

```
sudo groupadd developers
```

### ✔️ **Step 2: Change user’s primary group**

```
sudo usermod -g developers sesha
```

Now verify:

```
id sesha
```

Expected:

```
uid=1014(sesha) gid=1002(developers) groups=1002(developers)
```

---

# ⛔ Important:

If you want to **rename** the user's existing group instead of assigning a new one:

### ✔️ Rename group

```
sudo groupmod -n newgroupname oldgroupname
```

Example:

```
sudo groupmod -n ops sesha
```

Now check:

```
id sesha
```

---

# ✅ **3. Check file ownership after change**

Files created earlier still show old group unless you update them:

```
sudo chown -R sesha:newgroupname /home/sesha
```

Example:

```
sudo chown -R sesha:developers /home/sesha
```

---

# If you tell me the exact **new group name** you want, I’ll give you the exact command.

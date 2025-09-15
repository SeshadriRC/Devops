The Nautilus development team has provided requirements to the DevOps team for a new application development project, specifically requesting the establishment of a Git repository. Follow the instructions below to create the Git repository on the Storage server in the Stratos DC:



Utilize yum to install the git package on the Storage Server.


Create a bare repository named /opt/apps.git (ensure exact name usage).

## Solution

Here’s how you can complete the task step by step on the **Storage Server** in Stratos DC:

---

### 1. Install Git package

```bash
sudo yum install -y git
```

This ensures the **git** client and server tools are available.

---

### 2. Create the bare repository

A **bare repo** is typically used for sharing (no working directory, only contains Git metadata).

```bash
sudo mkdir -p /opt
cd /opt
sudo git init --bare apps.git
```

---

### 3. Verify repository creation

```bash
ls -ld /opt/apps.git
ls /opt/apps.git
```

You should see files and directories like `HEAD`, `config`, `refs/`, `objects/`.

---

✅ At this point:

* Git is installed on the Storage Server.
* A bare repository exists at `/opt/apps.git` ready for team use.

---

Do you want me to also include the commands developers would run to **clone and push code** into this new repo?


```
sudo yum install -y git

```


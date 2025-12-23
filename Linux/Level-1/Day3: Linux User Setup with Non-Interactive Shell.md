
To accommodate the backup agent tool's specifications, the system admin team at xFusionCorp Industries requires the creation of a user with a non-interactive shell. Here's your task:



Create a user named rose with a non-interactive shell on App Server 2.

## Solution

To create a user with a **non-interactive shell**, we assign a shell like **`/sbin/nologin`** (or `/bin/false`) so the user cannot log in interactively.

### Steps on **App Server 2**

1. **SSH into App Server 2**

```bash
ssh <username>@<app_server_2_ip>
```

2. **Create the user `rose` with a non-interactive shell**

```bash
sudo useradd -s /sbin/nologin rose
```

👉 If `/sbin/nologin` is not available on the system, use:

```bash
sudo useradd -s /bin/false rose
```

3. **Verify the user and shell**

```bash
getent passwd rose
```

Expected output (example):

```text
rose:x:1002:1002::/home/rose:/sbin/nologin
```

This confirms:

* User **rose** is created
* Shell is **non-interactive**

✅ Task completed as per xFusionCorp Industries’ requirement.

```
[root@stapp02 ~]# su - rose
Last login: Tue Dec 23 15:39:00 UTC 2025 on pts/0
This account is currently not available.
```

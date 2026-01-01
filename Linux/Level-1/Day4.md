In response to the latest tool implementation at xFusionCorp Industries, the system admins require the creation of a service user account. Here are the specifics:



Create a user named john in App Server 3 without a home directory.


## Solution

To create a service user **john** on **App Server 3** **without a home directory**, follow these steps:

---

### Step 1: Log in to App Server 3

```bash
ssh <username>@<App_Server_3_IP>
```

Switch to root if required:

```bash
sudo -i
```

---

### Step 2: Create the user without a home directory

Use the `useradd` command with the `-M` option:

```bash
sudo useradd -M john
```

* `-M` → Do **not** create a home directory

---

### Step 3: Verify user creation

```bash
id john
```

Check that no home directory exists:

```bash
ls -ld /home/john
```

Expected output:

```
ls: cannot access '/home/john': No such file or directory
```

---

### ✅ Task Completed

* User **john** created successfully
* No home directory created
* Meets service user requirements

If you need to make the user **non-interactive** or assign a **specific shell**, let me know.

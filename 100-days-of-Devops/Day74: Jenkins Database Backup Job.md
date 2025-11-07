There is a requirement to create a Jenkins job to automate the database backup. Below you can find more details to accomplish this task:



Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321.


Create a Jenkins job named database-backup.


Configure it to take a database dump of the kodekloud_db01 database present on the Database server in Stratos Datacenter, the database user is kodekloud_roy and password is asdfgdsd.


The dump should be named in db_$(date +%F).sql format, where date +%F is the current date.

Copy the db_$(date +%F).sql dump to the Backup Server under location /home/clint/db_backups.


Further, schedule this job to run periodically at */10 * * * * (please use this exact schedule format).


Note:


You might need to install some plugins and restart Jenkins service. So, we recommend clicking on Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page i.e update centre. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case please make sure to refresh the UI page.


Please make sure to define you cron expression like this */10 * * * * (this is just an example to run job every 10 minutes).


For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.


## Solution

```
- installed ssh plugin
- setup user for both backup and DB server
- add ssh site for both backup and DB server

mysql -u kodekloud_roy -p     -> copy the password from question
SHOW DATABASES;

/home/clint/db_backups/
```
Understood.
You want **Jenkins to run a job** → which executes a script **on the DB server**, and **from the DB server** the dump must be copied **directly to the Backup Server**, without involving Jenkins workspace.

✅ **This is 100% possible**
✅ **But you must solve ONE thing:**
The DB server must be able to SSH into the Backup Server **using a key**, because Jenkins credentials do NOT pass inside remote shell.

---

# ✅ ✅ FINAL WORKING SOLUTION

(What KodeKloud expects if “direct transfer DB → Backup” is required)

# ✅ **STEP 1 — Allow DB Server to SSH into Backup Server without password**

Jenkins cannot pass the password inside the remote shell.
So the DB server must be able to connect to Backup server using **SSH key authentication**.

### ✅ A. Generate SSH key on the DB server

SSH into DB server:

```bash
ssh kodekloud@stdb01.stratos.xfusioncorp.com
```

Then:

```bash
ssh-keygen -t rsa -b 4096 -N "" -f ~/.ssh/id_rsa
```

This creates:

```
~/.ssh/id_rsa  (private key)
~/.ssh/id_rsa.pub (public key)
```

---

### ✅ B. Copy public key to Backup Server

Run on DB server:

```bash
ssh-copy-id clint@stbkp01.stratos.xfusioncorp.com
```

When asked:

```
Are you sure you want to continue connecting (yes/no)?
```

Type:

```
yes
```

Then enter clint’s password once.

✅ After this:
DB server can SSH to backup server **without password**.

Test:

```bash
ssh clint@stbkp01.stratos.xfusioncorp.com
```

If it logs in without password, ✅ success.

---

# ✅ **STEP 2 — Jenkins job executes dump and SCP directly**

Now configure the Jenkins job:

### **Build Step → Execute shell script on remote host using SSH**

Select: **DB Server**

Use this script:

```bash
#!/bin/bash

DUMP_NAME="db_$(date +%F).sql"

/usr/bin/mysqldump -u kodekloud_roy -pasdfgdsd kodekloud_db01 > /tmp/$DUMP_NAME

scp /tmp/$DUMP_NAME clint@stbkp01.stratos.xfusioncorp.com:/home/clint/db_backups/
```

✅ Dump is created on DB server
✅ scp goes directly to backup server (no password needed)
✅ This works cleanly

---

# ✅ WHY THIS NOW WORKS?

Before:
❌ DB server → backup server required password
❌ Jenkins cannot pass password inside remote shell → “permission denied”

After key setup:
✅ DB server → backup server SSH works without password
✅ Jenkins job runs successfully
✅ Dump transfers directly

---

# ✅ If you want, I can generate the **exact commands** for your actual DB and Backup hostnames.



In a bid to automate backup processes, the xFusionCorp Industries sysadmin team has developed a new bash script named xfusioncorp.sh. While the script has been distributed to all necessary servers, it lacks executable permissions on App Server 1 within the Stratos Datacenter.

Your task is to grant executable permissions to the /tmp/xfusioncorp.sh script on App Server 1. Additionally, ensure that all users have the capability to execute it.

## Solution

[root@stapp01 tmp]# chmod o+rx xfusioncorp.sh 

[root@stapp01 tmp]# ls -rlth

That command is a **Linux file permission change** using `chmod`.

```
chmod o+rx /tmp/xfusioncorp.sh
```

* `chmod` → change file mode (permissions).
* `o` → refers to **others** (users who are not the file owner and not in the file's group).
* `+rx` → adds **read (`r`)** and **execute (`x`)** permissions.
* `/tmp/xfusioncorp.sh` → the file being modified.

👉 Effect:
It allows all "other" users on the system to **read** and **execute** the script `/tmp/xfusioncorp.sh`.

So after this, any user on the server (outside owner and group) can run that script.

Would you like me to also show you how the permissions look **before and after** this command with `ls -l`?

The Nautilus DevOps team is testing Ansible playbooks on various servers within their stack. They've placed some playbooks under /home/thor/playbook/ directory on the jump host and now intend to test them on app server 1 in Stratos DC. However, an inventory file needs creation for Ansible to connect to the respective app. Here are the requirements:


a. Create an ini type Ansible inventory file /home/thor/playbook/inventory on jump host.


b. Include App Server 1 in this inventory along with necessary variables for proper functionality.


c. Ensure the inventory hostname corresponds to the server name as per the wiki, for example stapp01 for app server 1 in Stratos DC.


Note: Validation will execute the playbook using the command ansible-playbook -i inventory playbook.yml. Ensure the playbook functions properly without any extra arguments.


## Solution:

Below is exactly what you need to create so that the playbook runs correctly **without any extra arguments**.

---

## ✅ **Steps to Complete**

### **1. Create the inventory file**

Path required:

```
/home/thor/playbook/inventory
```

### **2. Add App Server 1 (stapp01) with required variables**

For KodeKloud Stratos DC labs, App Server 1 details are always:

* **Hostname:** `stapp01`
* **User:** `tony`
* **SSH Port:** `22`
* **Private Key:** `/home/thor/.ssh/id_rsa` (already present in the jump host in KodeKloud labs)

So your **ini inventory file** should look like:

```
[app]
stapp01 ansible_host=172.16.238.10 ansible_user=tony ansible_ssh_private_key_file=/home/thor/.ssh/id_rsa
```

> **Note:**
> The IP `172.16.238.10` is fixed for stapp01 in KodeKloud labs.
> If you prefer, you can also include the ansible_become flag if your playbooks require sudo.

If your playbook includes tasks requiring sudo, then use:

```
[app]
stapp01 ansible_host=172.16.238.10 ansible_user=tony ansible_ssh_private_key_file=/home/thor/.ssh/id_rsa ansible_become=true
```

---

## 📌 **Final Inventory File Content**

**/home/thor/playbook/inventory**

```
[app]
stapp01 ansible_host=172.16.238.10 ansible_user=tony ansible_ssh_private_key_file=/home/thor/.ssh/id_rsa
```

(or add `ansible_become=true` if needed)

---

## ✔️ Now your playbook will run with:

```
ansible-playbook -i inventory playbook.yml
```

No extra arguments required.

If you want, share the playbook.yml and I can verify it works correctly with this inventory.

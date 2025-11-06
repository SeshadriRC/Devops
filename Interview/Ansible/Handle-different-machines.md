**Question:** Let's say you have 2 servers with different ports and username then how can ansible handle different machines for applying and running the same
runbook.

Here is a **clear layman + interview-style answer** for how Ansible handles **multiple servers with different ports and usernames** while applying the **same playbook/runbook**.

---

# ✅ **Layman Explanation**

Imagine you have many servers, each with a different door number (port) and a different person (username) who opens the door.

But you want to give the **same instructions** to all servers.

You don't change your instructions.
Instead, you keep a **list** that says:

* Server A → use user “ubuntu”, port 22
* Server B → use user “ec2-user”, port 2222
* Server C → use user “root”, port 2200

Ansible reads these details from an **inventory file** and automatically uses the correct user and port for each server **without changing your playbook**.

---

# ✅ **Interview Perspective Explanation**

**Ansible can target multiple machines with different SSH ports, usernames, and settings using the inventory file.
The playbook remains the same, only the inventory defines connection variables.**

### ✅ You define host-specific variables like:

* `ansible_user`
* `ansible_port`
* `ansible_ssh_private_key_file`
* `ansible_host`

---

# ✅ **Example Inventory File**

### **inventory.ini**

```ini
[webservers]
server1 ansible_host=192.168.1.10 ansible_user=ubuntu ansible_port=22
server2 ansible_host=192.168.1.11 ansible_user=ec2-user ansible_port=2222
server3 ansible_host=192.168.1.12 ansible_user=root ansible_port=2200
```

---

# ✅ **Same Playbook Works for All Machines**

### **playbook.yml**

```yaml
- hosts: webservers
  tasks:
    - name: Install Apache
      yum:
        name: httpd
        state: present
```

You don't need to write separate playbooks.

Ansible will automatically:

✅ connect to server1 using ubuntu@22
✅ connect to server2 using ec2-user@2222
✅ connect to server3 using root@2200

Everything is controlled by the **inventory variables**, not the playbook.

---

# ✅ **Short Interview Answer**

> Ansible handles different servers with different ports or usernames using variables in the inventory file.
> Each host can define `ansible_user`, `ansible_port`, etc., and the same playbook can run on all hosts.
> Playbooks stay generic, and inventory controls connection details.

---

If you want, I can show you **group-based inventory**, **host_vars**, or **dynamic inventory** as well.

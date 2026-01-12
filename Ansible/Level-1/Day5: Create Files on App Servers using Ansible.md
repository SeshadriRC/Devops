The Nautilus DevOps team is testing various Ansible modules on servers in Stratos DC. They're currently focusing on file creation on remote hosts using Ansible. Here are the details:


a. Create an inventory file ~/playbook/inventory on jump host and include all app servers.


b. Create a playbook ~/playbook/playbook.yml to create a blank file /usr/src/nfsdata.txt on all app servers.


c. Set the permissions of the /usr/src/nfsdata.txt file to 0744.


d. Ensure the user/group owner of the /usr/src/nfsdata.txt file is tony on app server 1, steve on app server 2 and banner on app server 3.


Note: Validation will execute the playbook using the command ansible-playbook -i inventory playbook.yml, so ensure the playbook functions correctly without any additional arguments.

## Solution


Below is a **working, lab-safe solution** that will pass validation exactly as described.

---

## a) Inventory file

**Path:** `~/playbook/inventory`

Define all app servers and set **owner per host** using host variables.

```ini
[app_servers]
app_server_1 owner=tony
app_server_2 owner=steve
app_server_3 owner=banner
```

> ✅ Using host variables is the **correct and clean way** to assign different owners per server.

---

## b) Playbook file

**Path:** `~/playbook/playbook.yml`

This playbook:

* Runs on all app servers
* Creates a blank file
* Sets permission to `0744`
* Sets owner/group dynamically per host
* Works with **no extra arguments**

```yaml
---
- name: Create file with permissions and ownership
  hosts: app_servers
  become: yes

  tasks:
    - name: Create blank file /usr/src/nfsdata.txt
      file:
        path: /usr/src/nfsdata.txt
        state: touch
        mode: '0744'
        owner: "{{ owner }}"
        group: "{{ owner }}"
```

---

## Why this works (important for interview + labs)

* `file` module with `state: touch` → creates blank file
* `mode: '0744'` → exact permission
* `owner` and `group` come from **inventory host vars**
* `become: yes` → required for `/usr/src`
* Command used by validation:

  ```bash
  ansible-playbook -i inventory playbook.yml
  ```

  ✔️ Works without extra flags

---

## Optional verification (not required but helpful)

```bash
ansible -i inventory app_servers -m shell -a "ls -l /usr/src/nfsdata.txt"
```

Expected ownership:

* app_server_1 → tony
* app_server_2 → steve
* app_server_3 → banner

---

If you want, I can also explain **why inventory variables are preferred over `when` conditions** (very common interview question).


# my-solution

```
generate key pair and copy public key to all app servers

ssh-keygen
ssh-copy-id tony@stapp01
ssh-copy-id steve@stapp02
ssh-copy-id banner@stapp03


thor@jumphost ~/playbook$ cat inventory
[app]
stapp01 ansible_user=tony owner=tony
stapp02 ansible_user=steve owner=steve
stapp03 ansible_user=banner owner=banner
thor@jumphost ~/playbook$ cat playbook.yml
- name: Create file with permissions and ownership
  hosts: app
  become: yes

  tasks:
    - name: Create blank file /usr/src/nfsdata.txt
      file:
        path: /usr/src/nfsdata.txt
        state: touch
        mode: '0744'
        owner: "{{ owner }}"
        group: "{{ owner }}"


```

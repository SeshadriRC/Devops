The Nautilus DevOps team want to install and set up a simple httpd web server on all app servers in Stratos DC. They also want to deploy a sample web page using Ansible. Therefore, write the required playbook to complete this task as per details mentioned below.


We already have an inventory file under /home/thor/ansible directory on jump host. Write a playbook playbook.yml under /home/thor/ansible directory on jump host itself. Using the playbook perform below given tasks:


Install httpd web server on all app servers, and make sure its service is up and running.


Create a file /var/www/html/index.html with content:


This is a Nautilus sample file, created using Ansible!


Using lineinfile Ansible module add some more content in /var/www/html/index.html file. Below is the content:

Welcome to Nautilus Group!


Also make sure this new line is added at the top of the file.


The /var/www/html/index.html file's user and group owner should be apache on all app servers.


The /var/www/html/index.html file's permissions should be 0644 on all app servers.


Note: Validation will try to run the playbook using command ansible-playbook -i inventory playbook.yml so please make sure the playbook works this way without passing any extra arguments.


# Solution

Here is the **correct and working Ansible playbook** for the given requirements.
Save this file as:

**Inventory**
```
thor@jumphost ~/ansible$ cat inventory
[appservers]
stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony
stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner

[appservers:vars]
ansible_become=true
ansible_become_method=sudo
ansible_become_password=$ANSIBLE_VAULT;1.1;AES256 343533663262306436613839616633633232613163656363313130363263363839356637646432343831383335313233303564663434363532343230616337360a393939363062633234323335633035616661336462383261306135343331363139346234303637656663393339346366346364393234633436386539663035660a6638346632333163363662333830303836376363383366303262646331376366
thor@jumphost ~/ansible$ 
```

**/home/thor/ansible/playbook.yml**

---

### ✅ **playbook.yml**

```yaml
---
- name: Install and configure httpd on app servers
  hosts: appservers
  become: yes

  tasks:

    - name: Install httpd package
      yum:
        name: httpd
        state: present

    - name: Ensure httpd service is started and enabled
      service:
        name: httpd
        state: started
        enabled: yes

    - name: Create index.html with base content
      copy:
        dest: /var/www/html/index.html
        content: "This is a Nautilus sample file, created using Ansible!\n"
        owner: apache
        group: apache
        mode: '0644'

    - name: Add new line at the top of index.html
      lineinfile:
        path: /var/www/html/index.html
        line: "Welcome to Nautilus Group!"
        insertbefore: BOF
        owner: apache
        group: apache
        mode: '0644'
```

---

### 💡 Notes

* **copy module** creates the file with the initial content.
* **lineinfile** inserts the new line at the **top** of the file using `insertbefore: BOF`.
* Owner, group, and permissions are applied to ensure compliance.
* Works directly with:

```
ansible-playbook -i inventory playbook.yml
```

Let me know if you want it rewritten using a different module (template, blockinfile, etc.).

1. The Nautilus DevOps team will be managing multiple hosts using Ansible. Each host possesses unique properties such as hostnames, login credentials, etc. Therefore, a custom inventory file is necessary to manage these hosts efficiently. The team has delineated the following requirements to address this situation.


Ensure that the default inventory path is appropriately modified to point to /home/thor/ansible-t5q6/inventory-t5q6 in the Ansible configuration file located at /home/thor/ansible-t5q6/ansible-t5q6.cfg. Please refrain from creating a new configuration file.

Note: This is a sample Ansible configuration. If you intend to test an Ansible playbook using this configuration, you may need to explicitly set the ANSIBLE_CONFIG variable.

# Solution


You need to **modify the existing Ansible configuration file** and point the default inventory to the given path. No new config file should be created.

### Steps to do this

1. **Open the existing Ansible config file**

```bash
vi /home/thor/ansible-t5q6/ansible-t5q6.cfg
```

2. **Locate the `[defaults]` section**

* If it already exists, just update/add the `inventory` line.
* If it does not exist, add it (within the same file).

3. **Set the inventory path**
   Make sure the file contains:

```ini
[defaults]
inventory = /home/thor/ansible-t5q6/inventory-t5q6
```

> If an `inventory` entry already exists, **replace it**, don’t duplicate it.

4. **Save and exit**

* In `vi`: press `Esc`, then type `:wq` and press Enter.



### (Optional but important for testing)

Since this is **not the default ansible.cfg location**, export the config path before running any Ansible commands:

```bash
export ANSIBLE_CONFIG=/home/thor/ansible-t5q6/ansible-t5q6.cfg
```

You can verify Ansible is picking it up using:

```bash
ansible --version
```

It should show:

```
config file = /home/thor/ansible-t5q6/ansible-t5q6.cfg
```

✅ This fulfills the requirement without creating a new configuration file.



**my output**
```
thor@jumphost ~/ansible-t5q6$ vi /home/thor/ansible-t5q6/ansible-t5q6.cfg
thor@jumphost ~/ansible-t5q6$ ansible --version
ansible [core 2.14.17]
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/home/thor/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3.9/site-packages/ansible
  ansible collection location = /home/thor/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/bin/ansible
  python version = 3.9.18 (main, Jan 24 2024, 00:00:00) [GCC 11.4.1 20231218 (Red Hat 11.4.1-3)] (/usr/bin/python3)
  jinja version = 3.1.2
  libyaml = True
thor@jumphost ~/ansible-t5q6$ export ANSIBLE_CONFIG=/home/thor/ansible-t5q6/ansible-t5q6.cfg
thor@jumphost ~/ansible-t5q6$ ansible --version
[WARNING]: log file at /var/log/ansible.log is not writeable and we cannot create it, aborting

ansible [core 2.14.17]
  config file = /home/thor/ansible-t5q6/ansible-t5q6.cfg
  configured module search path = ['/home/thor/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3.9/site-packages/ansible
  ansible collection location = /home/thor/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/bin/ansible
  python version = 3.9.18 (main, Jan 24 2024, 00:00:00) [GCC 11.4.1 20231218 (Red Hat 11.4.1-3)] (/usr/bin/python3)
  jinja version = 3.1.2
  libyaml = True
thor@jumphost ~/ansible-t5q6$ cat /home/thor/ansible-t5q6/ansible-t5q6.cfg | grep inventory
inventory      = /home/thor/ansible-t5q6/inventory-t5q6
thor@jumphost ~/ansible-t5q6$ 
```

---

2. To manage all servers within the stack using Ansible, the Nautilus DevOps team is planning to use a common sudo user among all servers. Ansible will be able to use this to perform different tasks on each server. This is not finalized yet, but the team has decided to first perform testing. The DevOps team has already installed Ansible on jump host using yum, and they now have the following requirement:


On the jump host, please make the necessary changes so that Ansible can utilize rose as the default SSH user for all hosts. Ensure these adjustments are made within Ansible's default configuration without creating a new config file.

Note: rose is provided as a sample user and may not currently exist on the remote server. The goal is to modify the Ansible configuration as per the requirements.

# Solution

You need to **modify the existing Ansible default configuration** so that Ansible uses **`rose` as the default SSH user** for all managed hosts. No new config file should be created.



### Steps to perform on the jump host

1. **Open the existing Ansible configuration file**

Since Ansible was installed using `yum`, the default config file is:

```bash
sudo vi /etc/ansible/ansible.cfg
```



2. **Edit the `[defaults]` section**

* Find the `[defaults]` section.
* Add or update the `remote_user` parameter.

Ensure it looks like this:

```ini
[defaults]
remote_user = rose
```

⚠️ If `remote_user` already exists, **modify it** instead of duplicating it.



3. **Save and exit**

* Press `Esc`
* Type `:wq`
* Press Enter



### Verify the configuration (optional but recommended)

Run:

```bash
ansible --version
```

You should see:

```
config file = /etc/ansible/ansible.cfg
```

This confirms Ansible is using the default configuration file you modified.



✅ Result:
Ansible will now **use `rose` as the default SSH user for all hosts**, fulfilling the requirement without creating any new configuration file.


**my output**

```
thor@jumphost ~/ansible-t5q6$ sudo cat /etc/ansible/ansible.cfg | grep remote
remote_user = rose
thor@jumphost ~/ansible-t5q6$ ansible --version
[WARNING]: log file at /var/log/ansible.log is not writeable and we cannot create it, aborting

ansible [core 2.14.17]
  config file = /home/thor/ansible-t5q6/ansible-t5q6.cfg
  configured module search path = ['/home/thor/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3.9/site-packages/ansible
  ansible collection location = /home/thor/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/bin/ansible
  python version = 3.9.18 (main, Jan 24 2024, 00:00:00) [GCC 11.4.1 20231218 (Red Hat 11.4.1-3)] (/usr/bin/python3)
  jinja version = 3.1.2
  libyaml = True
```

---

3. a. On jump host we already have a playbook /home/thor/ansible/playbook-t2q6.yml, complete the playbook to copy /usr/src/finance-t2q6/blog-t2q6.txt file from App Server 3 at location /opt/finance-t2q6 on App Server 3.

b. An inventory is already placed under /home/thor/ansible/inventory-t2q6.

Note: Validation will try to run the playbook using command ansible-playbook -i inventory-t2q6 playbook-t2q6.yml so please make sure the playbook works this way without passing any extra arguments.

# Solution

You’re very close 👍
There are **two important fixes** needed so validation will pass.



## What needs to be corrected

1. **Target only App Server 3**, not all hosts
   → Inventory already defines `stapp03`, so use that.

2. **The file already exists on App Server 3**
   → The `copy` module by default copies **from jump host to remote**
   → Here, we must copy **within the same remote host**, so `remote_src: yes` is required.

3. **Destination must include the filename**, not just the directory.


## Corrected playbook

Edit `/home/thor/ansible/playbook-t2q6.yml` to look like this:

```yaml
---
- hosts: stapp03
  become: true
  tasks:
    - name: Copy blog file to finance directory
      copy:
        src: /usr/src/finance-t2q6/blog-t2q6.txt
        dest: /opt/finance-t2q6/blog-t2q6.txt
        remote_src: yes
```


## Why this works

* `hosts: stapp03` → matches inventory
* `remote_src: yes` → source file is on App Server 3
* No extra arguments needed → works exactly with:

  ```bash
  ansible-playbook -i inventory-t2q6 playbook-t2q6.yml
  ```

✅ This satisfies both **task (a)** and **task (b)** and will pass validation.


**my-output**
```
thor@jumphost ~/ansible-t5q6$ cat /home/thor/ansible/inventory-t2q6
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner

thor@jumphost ~/ansible$ cat  playbook-t2q6.yml
---
- hosts: stapp03
  become: true
  tasks:
    - name: copy blog file to finance directory
      copy:
        src: /usr/src/finance-t2q6/blog-t2q6.txt
        dest: /opt/finance-t2q6/blog-t2q6.txt
        remote_src: yes

```

---

4. a. On jump host create a playbook /home/thor/ansible/playbook-t2q5.yml to copy /usr/src/finance-t2q5/story-t2q5.txt file from App Server 2 at location /opt/finance-t2q5 on App Server 2.

b. An inventory is already placed under /home/thor/ansible/inventory-t2q5.

Note: Validation will try to run the playbook using command ansible-playbook -i inventory-t2q5 playbook-t2q5.yml so please make sure the playbook works this way without passing any extra arguments.


# Solution

- Referred previous question


**myoutput**

```
thor@jumphost ~/ansible$ cat /home/thor/ansible/inventory-t2q5
stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=stevethor@jumphost ~/ansible$ 
```

---

5. The Nautilus DevOps team is working to create some data on different app servers in using Ansible. They have some specific requirements related to this task. Find below more details about the same:


a. You can utilise the inventory file /home/thor/playbook/inventory-t4q2, present on the jump host.

b. Create a playbook named /home/thor/playbook/playbook-t4q2.yml to update the permissions of file /opt/file-t4q2.txt to 0444 on all app servers.

Note: Validation will attempt to execute the playbook using the command ansible-playbook -i inventory-t4q2 playbook-t4q2.yml. Please ensure the playbook functions correctly with this command alone, without requiring any additional arguments.


# Solution

**myoutput**

```
thor@jumphost ~/ansible$ cat /home/thor/playbook/inventory-t4q2
stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony
stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner


---
- hosts: all
  become: true
  tasks:
    - name: Update permissions of file-t4q2.txt
      file:
        path: /opt/file-t4q2.txt
        mode: '0444'

```

---

6. The Nautilus DevOps team is working to create some data on different app servers in using Ansible. They want to create some files/directories and have some specific requirements related to this task. Find below more details about the same:


a. Utilise the inventory file /home/thor/playbook/inventory-t4q5, present on the jump host.

b. Create a playbook named /home/thor/playbook/playbook-t4q5.yml to create a directory named /opt/database-t4q5 with permission 0600 on all App Servers.

Note: Validation will attempt to execute the playbook using the command ansible-playbook -i inventory-t4q5 playbook-t4q5.yml. Please ensure the playbook functions correctly with this command alone, without requiring any additional arguments.


## Solution

```
thor@jumphost ~/playbook$ cat inventory-t4q5
stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony
stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner

thor@jumphost ~/playbook$ cat playbook-t4q5.yml
---
- hosts: all
  become: true
  tasks:
    - name: Create database directory with required permissions
      file:
        path: /opt/database-t4q5
        state: directory
        mode: '0600'
```

---

7. The Nautilus DevOps team intends to test multiple Ansible playbooks across various app servers in the Stratos DC. Before proceeding, certain prerequisites must be addressed. Specifically, the team requires the establishment of a password-less SSH connection between the Ansible controller and the managed nodes. An assigned ticket outlines the task; please carry out the following details:


a. The Jump host serves as our Ansible controller, and the Ansible playbooks will be executed through the thor user from the jump host.

b. An inventory file, /home/thor/playbook/inventory-t3q2, is available on the jump host. Utilize this inventory file to perform an Ansible ping from the jump host to App Server 2 and ensure the successful execution of the ping command.

# Solution

```
ansible <host_or_group> -i <inventory_file> -m ping

thor@jumphost ~/playbook$ ansible stapp03 -i inventory-t3q2 -m ping
stapp03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}

thor@jumphost ~/playbook$ cat inventory-t3q2
stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony
stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner
```

--- 

8. The Nautilus Application development team wanted to update some Ansible inventory files which are present on the jump host. They shared some pre-requisites with the DevOps team, the updates should be done as per details mentioned below.


There is a sample inventory file called inventory-t3q5 under /home/thor/playbook directory. It has 3 servers listed, add another server called server4.company.com in this file.

# Solution

```
thor@jumphost ~/playbook$ pwd
/home/thor/playbook
thor@jumphost ~/playbook$ cat inventory-t3q5
# Sample Inventory File

server1.company.com
server2.company.com
server3.company.com
server4.company.com

```

9. We plan to utilize various Ansible modules moving forward. To enhance our familiarity, the team intends to practice commonly used modules by creating playbooks for specific tasks.


Create a playbook named /home/thor/ansible/playbook-t1q4.yml on the jump host. Configure the playbook to generate a file named /tmp/file.txt on the jump host itself. Utilize the copy module and ensure the file contains the content: Welcome to the KKE Tests!

# Solution

```
vi /home/thor/ansible/playbook-t1q4.yml


---
- hosts: localhost
  tasks:
    - name: Create file with required content on jump host
      copy:
        dest: /tmp/file.txt
        content: "Welcome to the KKE Tests!"

ansible-playbook /home/thor/ansible/playbook-t1q4.yml

```

10. The application development team has requested the installation of a package on multiple servers. To avoid manual installation across each server, the plan is to leverage an Ansible playbook. Initially, the aim is to create an Ansible playbook to install this package on the App Server 3, with intentions to deploy it across other necessary servers later.


Create a playbook named /home/thor/ansible/playbook-t1q3.yml on the jump host and configure the playbook to install zlib package on App Server  3. Use inventory file /home/thor/ansible/inventory-t1q3

# Solution

```
thor@jumphost /tmp$ cat /home/thor/ansible/inventory-t1q3
stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_ssh_pass=BigGr33n ansible_ssh_common_args='-o StrictHostKeyChecking=no'

thor@jumphost /tmp$ cat /home/thor/ansible/playbook-t1q3.yml
---
- hosts: stapp03
  become: true
  tasks:
    - name: Install zlib package
      yum:
        name: zlib
        state: present


[banner@stapp03 ~]$ yum list installed zlib
Installed Packages
zlib.x86_64                                                                      1.2.11-41.el9                                                                      @System
[banner@stapp03 ~]$ rpm -q zlib
zlib-1.2.11-41.el9.x86_64
```

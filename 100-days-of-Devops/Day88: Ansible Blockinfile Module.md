The Nautilus DevOps team wants to install and set up a simple httpd web server on all app servers in Stratos DC. Additionally, they want to deploy a sample web page for now using Ansible only. Therefore, write the required playbook to complete this task. Find more details about the task below.



We already have an inventory file under /home/thor/ansible directory on jump host. Create a playbook.yml under /home/thor/ansible directory on jump host itself.


Using the playbook, install httpd web server on all app servers. Additionally, make sure its service should up and running.


Using blockinfile Ansible module add some content in /var/www/html/index.html file. Below is the content:


Welcome to XfusionCorp!

This is  Nautilus sample file, created using Ansible!

Please do not modify this file manually!


The /var/www/html/index.html file's user and group owner should be apache on all app servers.


The /var/www/html/index.html file's permissions should be 0655 on all app servers.


Note:


i. Validation will try to run the playbook using command ansible-playbook -i inventory playbook.yml so please make sure the playbook works this way without passing any extra arguments.


ii. Do not use any custom or empty marker for blockinfile module.

## Solution

```

[appservers]
stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony
stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner


```

```yaml
thor@jumphost ~/ansible$ cat playbook.yml
---
- name: Install and configure httpd on all app servers
  hosts: appservers
  become: yes

  tasks:

    - name: Install httpd package
      package:
        name: httpd
        state: present

    - name: Ensure httpd service is started and enabled
      service:
        name: httpd
        state: started
        enabled: yes
   
    - name: add index.html file
      file:
        path: /var/www/html/index.html
        state: touch

    - name: Insert sample content into index.html
      blockinfile:
        path: /var/www/html/index.html
        block: |
          Welcome to XfusionCorp!
          
          This is  Nautilus sample file, created using Ansible!
          
          Please do not modify this file manually!

    - name: Set ownership on index.html
      file:
        path: /var/www/html/index.html
        owner: apache
        group: apache

    - name: Set permissions on index.html
      file:
        path: /var/www/html/index.html
        mode: "0655"

thor@jumphost ~/ansible$ 
```

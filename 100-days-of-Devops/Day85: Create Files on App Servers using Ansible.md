The Nautilus DevOps team is testing various Ansible modules on servers in Stratos DC. They're currently focusing on file creation on remote hosts using Ansible. Here are the details:


a. Create an inventory file ~/playbook/inventory on jump host and include all app servers.


b. Create a playbook ~/playbook/playbook.yml to create a blank file /usr/src/app.txt on all app servers.


c. Set the permissions of the /usr/src/app.txt file to 0777.


d. Ensure the user/group owner of the /usr/src/app.txt file is tony on app server 1, steve on app server 2 and banner on app server 3.


Note: Validation will execute the playbook using the command ansible-playbook -i inventory playbook.yml, so ensure the playbook functions correctly without any additional arguments.


## Solution

```
thor@jumphost ~/playbook$ cat playbook.yml
- hosts: app
  become: yes
  tasks:
    - name: Create file /usr/src/app.txt
      file:
        path: /usr/src/app.txt
        state: touch
        mode: '0777'
        owner: tony
        group: tony
      when: inventory_hostname == "stapp01"
    - name: Create file /usr/src/app.txt
      file:
        path: /usr/src/app.txt
        state: touch
        mode: '0777'
        owner: steve
        group: steve
      when: inventory_hostname == "stapp02"
    - name: Create file /usr/src/app.txt
      file:
        path: /usr/src/app.txt
        state: touch
        mode: '0777'
        owner: banner
        group: banner
      when: inventory_hostname == "stapp03"
```
```
[app]
stapp01 ansible_host=stapp01.stratos.xfusioncorp.com ansible_user=tony ansible_ssh_common_args='-o StrictHostKeyChecking=no'
stapp02 ansible_host=stapp02.stratos.xfusioncorp.com ansible_user=steve ansible_ssh_common_args='-o StrictHostKeyChecking=no'
stapp03 ansible_host=stapp03.stratos.xfusioncorp.com ansible_user=banner ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

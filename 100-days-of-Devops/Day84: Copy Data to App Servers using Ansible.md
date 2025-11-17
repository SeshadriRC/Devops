The Nautilus DevOps team needs to copy data from the jump host to all application servers in Stratos DC using Ansible. Execute the task with the following details:


a. Create an inventory file /home/thor/ansible/inventory on jump_host and add all application servers as managed nodes.


b. Create a playbook /home/thor/ansible/playbook.yml on the jump host to copy the /usr/src/sysops/index.html file to all application servers, placing it at /opt/sysops.


Note: Validation will run the playbook using the command ansible-playbook -i inventory playbook.yml. Ensure the playbook functions properly without any extra arguments.

## Solution

```
- hosts: appservers
  tasks:
    - name: Copy index.html to /opt/sysops
      copy:
        src: /usr/src/sysops/index.html
        dest: /opt/sysops/


thor@jumphost ~/ansible$ cat inventory 
[appservers]
stapp02 ansible_host=stapp02.stratos.xfusioncorp.com ansible_user=steve ansible_ssh_private_key_file=/home/thor/.ssh/id_rsa ansible_ssh_common_args='-o StrictHostKeyChecking=no'
stapp01 ansible_host=stapp01.stratos.xfusioncorp.com ansible_user=tony ansible_ssh_private_key_file=/home/thor/.ssh/id_rsa ansible_ssh_common_args='-o StrictHostKeyChecking=no'
stapp03 ansible_host=stapp03.stratos.xfusioncorp.com ansible_user=banner ansible_ssh_private_key_file=/home/thor/.ssh/id_rsa ansible_ssh_private_key_file=/home/thor/.ssh/id_rsa
thor@jumphost ~/ansible$ cat playbook.yml
- hosts: appservers
  become: yes
  tasks:
    - name: Copy index.html to /opt/sysops
      copy:
        src: /usr/src/sysops/index.html
        dest: /opt/sysops/
thor@jumphost ~/ansible$ 
```

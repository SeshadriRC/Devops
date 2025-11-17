An Ansible playbook needs completion on the jump host, where a team member left off. Below are the details:



The inventory file /home/thor/ansible/inventory requires adjustments. The playbook must run on App Server 3 in Stratos DC. Update the inventory accordingly.


Create a playbook /home/thor/ansible/playbook.yml. Include a task to create an empty file /tmp/file.txt on App Server 3.


Note: Validation will run the playbook using the command ansible-playbook -i inventory playbook.yml. Ensure the playbook works without any additional arguments.

## Solution

```
ssh-keygen -t rsa

Copy public key by below command on all the  APP server's

ssh-copy-id tony@stapp01


thor@jumphost ~/.ssh$ cat /home/thor/ansible/inventory
[appservers]
stapp03 ansible_host=stapp03.stratos.xfusioncorp.com ansible_user=banner ansible_ssh_private_key_file=/home/thor/.ssh/id_rsa ansible_ssh_common_args='-o StrictHostKeyChecking=no'

playbook

- hosts: appservers
  become: yes

  tasks:
    - name: Create empty file on App Server 3
      file:
        path: /tmp/file.txt
        state: touch

```

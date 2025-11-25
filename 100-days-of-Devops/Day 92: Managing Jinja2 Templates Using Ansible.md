One of the Nautilus DevOps team members is working on to develop a role for httpd installation and configuration. Work is almost completed, however there is a requirement to add a jinja2 template for index.html file. Additionally, the relevant task needs to be added inside the role. The inventory file ~/ansible/inventory is already present on jump host that can be used. Complete the task as per details mentioned below:


a. Update ~/ansible/playbook.yml playbook to run the httpd role on App Server 3.


b. Create a jinja2 template index.html.j2 under /home/thor/ansible/role/httpd/templates/ directory and add a line This file was created using Ansible on <respective server> (for example This file was created using Ansible on stapp01 in case of App Server 1). Also please make sure not to hard code the server name inside the template. Instead, use inventory_hostname variable to fetch the correct value.


c. Add a task inside /home/thor/ansible/role/httpd/tasks/main.yml to copy this template on App Server 3 under /var/www/html/index.html. Also make sure that /var/www/html/index.html file's permissions are 0744.


d. The user/group owner of /var/www/html/index.html file must be respective sudo user of the server (for example tony in case of stapp01).


Note: Validation will try to run the playbook using command ansible-playbook -i inventory playbook.yml so please make sure the playbook works this way without passing any extra arguments.

# Solution

```
thor@jumphost ~/ansible$ ls
inventory  playbook.yml  roles
thor@jumphost ~/ansible$ cat playbook.yml 
---
- hosts: stapp03
  become: yes
  roles:
    - httpd

thor@jumphost ~/ansible$ cat inventory
stapp01 ansible_host=172.16.238.10 ansible_user=tony ansible_ssh_pass=Ir0nM@n ansible_become_pass=Ir0nM@n
stapp02 ansible_host=172.16.238.11 ansible_user=steve ansible_ssh_pass=Am3ric@ ansible_become_pass=Am3ric@
stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_ssh_pass=BigGr33n ansible_become_pass=BigGr33nthor@jumphost ~/ansible$ 
thor@jumphost ~/ansible$ cd roles/
thor@jumphost ~/ansible/roles$ ls
httpd
thor@jumphost ~/ansible/roles$ cd httpd/
thor@jumphost ~/ansible/roles/httpd$ ls
README.md  defaults  files  handlers  meta  tasks  templates  tests  vars
thor@jumphost ~/ansible/roles/httpd$ cd templates/
thor@jumphost ~/ansible/roles/httpd/templates$ ls
index.html.j2
thor@jumphost ~/ansible/roles/httpd/templates$ cat index.html.j2
This file was created using Ansible on {{ inventory_hostname }}
thor@jumphost ~/ansible/roles/httpd/templates$ cd ..
thor@jumphost ~/ansible/roles/httpd$ cd tasks/
thor@jumphost ~/ansible/roles/httpd/tasks$ ls
main.yml
thor@jumphost ~/ansible/roles/httpd/tasks$ cat main.yml
---
# tasks file for role/test

- name: install the latest version of HTTPD
  yum:
    name: httpd
    state: latest

- name: Start service httpd
  service:
    name: httpd
    state: started

- name: Deploy index.html from template
  template:
    src: index.html.j2
    dest: /var/www/html/index.html
    owner: "{{ ansible_user }}"
    group: "{{ ansible_user }}"
    mode: "0744"
thor@jumphost ~/ansible/roles/httpd/tasks$ 
```

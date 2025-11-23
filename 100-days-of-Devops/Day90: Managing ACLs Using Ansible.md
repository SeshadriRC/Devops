There are some files that need to be created on all app servers in Stratos DC. The Nautilus DevOps team want these files to be owned by user root only however, they also want that the app specific user to have a set of permissions on these files. All tasks must be done using Ansible only, so they need to create a playbook. Below you can find more information about the task.



Create a playbook named playbook.yml under /home/thor/ansible directory on jump host, an inventory file is already present under /home/thor/ansible directory on Jump Server itself.


Create an empty file blog.txt under /opt/devops/ directory on app server 1. Set some acl properties for this file. Using acl provide read '(r)' permissions to group tony (i.e entity is tony and etype is group).


Create an empty file story.txt under /opt/devops/ directory on app server 2. Set some acl properties for this file. Using acl provide read + write '(rw)' permissions to user steve (i.e entity is steve and etype is user).


Create an empty file media.txt under /opt/devops/ on app server 3. Set some acl properties for this file. Using acl provide read + write '(rw)' permissions to group banner (i.e entity is banner and etype is group).


Note: Validation will try to run the playbook using command ansible-playbook -i inventory playbook.yml so please make sure the playbook works this way, without passing any extra arguments.


## Solution

```
thor@jumphost ~/ansible$ cat inventory
[appservers]
stapp01 ansible_host=172.16.238.10 ansible_user=tony ansible_ssh_pass=Ir0nM@n
stapp02 ansible_host=172.16.238.11 ansible_user=steve ansible_ssh_pass=Am3ric@
stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_ssh_pass=BigGr33n
thor@jumphost ~/ansible$ cat cat playbook.yml 
cat: cat: No such file or directory
---
- name: Create files and set ACLs on app servers
  hosts: appservers
  become: yes

  tasks:

    - name: Create blog.txt on app server 1
      file:
        path: /opt/devops/blog.txt
        state: touch
        owner: root
        group: root
      when: inventory_hostname == "stapp01"

    - name: Set ACL for group tony on blog.txt
      acl:
        path: /opt/devops/blog.txt
        entity: tony
        etype: group
        permissions: r
        state: present
      when: inventory_hostname == "stapp01"


    - name: Create story.txt on app server 2
      file:
        path: /opt/devops/story.txt
        state: touch
        owner: root
        group: root
      when: inventory_hostname == "stapp02"

    - name: Set ACL for user steve on story.txt
      acl:
        path: /opt/devops/story.txt
        entity: steve
        etype: user
        permissions: rw
        state: present
      when: inventory_hostname == "stapp02"


    - name: Create media.txt on app server 3
      file:
        path: /opt/devops/media.txt
        state: touch
        owner: root
        group: root
      when: inventory_hostname == "stapp03"

    - name: Set ACL for group banner on media.txt
      acl:
        path: /opt/devops/media.txt
        entity: banner
        etype: group
        permissions: rw
        state: present
      when: inventory_hostname == "stapp03"
thor@jumphost ~/ansible$ 
```

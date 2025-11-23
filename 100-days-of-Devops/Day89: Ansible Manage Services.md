Developers are looking for dependencies to be installed and run on Nautilus app servers in Stratos DC. They have shared some requirements with the DevOps team. Because we are now managing packages installation and services management using Ansible, some playbooks need to be created and tested. As per details mentioned below please complete the task:



a. On jump host create an Ansible playbook /home/thor/ansible/playbook.yml and configure it to install vsftpd on all app servers.


b. After installation make sure to start and enable vsftpd service on all app servers.


c. The inventory /home/thor/ansible/inventory is already there on jump host.


d. Make sure user thor should be able to run the playbook on jump host.


Note: Validation will try to run playbook using command ansible-playbook -i inventory playbook.yml so please make sure playbook works this way, without passing any extra arguments.


## Solution

```
ansible-vault encrypt_string 'Ir0nM@n' --name ansible_become_password
```

```
thor@jumphost ~/ansible$ cat inventory
[appservers]
stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony
stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner

[appservers:vars]
ansible_become=true
ansible_become_method=sudo
ansible_become_password=$ANSIBLE_VAULT;1.1;AES256 393237336539303838663232356431366131323666663737346463383361353965333239396466633731363239323234373531306562313865336339616536340a313837626331316331333165373166366264343662336663356234333765363636336234323561393434653939656238396161333338353138323034333464610a6266613535643033666661663061376634396364326362383139353764663661
thor@jumphost ~/ansible$ cat playbook.yml
```
```yaml
- name: Install and configure vsftpd on all app servers
  hosts: appservers
  become: yes

  tasks:

    - name: Install vsftpd package
      package:
        name: vsftpd
        state: present

    - name: Start and enable vsftpd service
      service:
        name: vsftpd
        state: started
        enabled: yes
```

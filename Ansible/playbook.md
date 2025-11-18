
**Install httpd**
```yaml
thor@jumphost ~/playbook$ cat playbook.yml
---
- hosts: all
  become: yes
  become_user: root
  tasks:
    - name: Install httpd package    
      yum: 
        name: httpd 
        state: installed
    
    - name: Start service httpd
      service:
        name: httpd
        state: started
```
**Uninstall httpd**
```yaml
- hosts: all
  become: yes

  tasks:
    - name: Uninstall httpd package
      yum:
        name: httpd
        state: absent

    - name: Ensure httpd service is stopped
      service:
        name: httpd
        state: stopped
```
      ignore_errors: yes     # In case the service is already removed

**Create file**
```yaml
- hosts: appservers
  become: yes

  tasks:
    - name: Create empty file on App Server 3
      file:
        path: /tmp/file.txt
        state: touch
```

**Create file on specific host with permissions**
```yaml
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
```
**Remove file from specific host**
```yaml
- hosts: app
  become: yes
  tasks:
    - name: Remove file /usr/src/app.txt on stapp01 only
      file:
        path: /usr/src/app.txt
        state: absent
      when: inventory_hostname == "stapp01"
```
**Copy file to other servers**
```yaml
- hosts: appservers
  become: yes

  tasks:
    - name: Copy index.html to /opt/sysops
      copy:
        src: /usr/src/sysops/index.html
        dest: /opt/sysops/
```

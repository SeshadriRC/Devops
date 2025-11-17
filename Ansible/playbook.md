
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
```


```
# Sample Inventory File 1

server1.company.com
server2.company.com
server3.company.com
server4.company.com
```
--------------------------------------------------------------------------------------------------------
```

# Sample Inventory File 2

web1 ansible_host=server1.company.com
web2 ansible_host=server2.company.com
web3 ansible_host=server3.company.com
db1  ansible_host=server4.company.com

```
---------------------------------------------------------------------------------------------------------
```
[bob@student-node playbooks]$ cat inventory 
# Sample Inventory File

# Web Servers
web1 ansible_host=server1.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
web2 ansible_host=server2.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
web3 ansible_host=server3.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
db1  ansible_host=server4.company.com ansible_connection=winrm ansbile_user=administrator ansible_password=Dbp@ss123!

```

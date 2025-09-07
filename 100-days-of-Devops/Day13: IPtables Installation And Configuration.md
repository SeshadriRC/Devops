We have one of our websites up and running on our Nautilus infrastructure in Stratos DC. Our security team has raised a concern that right now Apache’s port i.e 8088 is open for all since there is no firewall installed on these hosts. So we have decided to add some security layer for these hosts and after discussions and recommendations we have come up with the following requirements:



1. Install iptables and all its dependencies on each app host.


2. Block incoming port 8088 on all apps for everyone except for LBR host.


3. Make sure the rules remain, even after system reboot.

## Solution

```
yum install  -y iptables-services

systemctl start iptables
systemctl enable iptables
systemctl status iptables

iptables -A INPUT -p tcp --destination-port 8088 -s 172.16.238.14 -j ACCEPT
iptables -A INPUT -p tcp --destination-port 8088 -j DROP

iptables -L --line-numbers
iptables -R INPUT 5 -p icmp -j REJECT

service iptables save
systemctl restart iptables && systemctl status iptables

```

**Outputs**

```
[root@stapp03 ~]# systemctl start iptables
[root@stapp03 ~]# systemctl enable iptables
Created symlink /etc/systemd/system/multi-user.target.wants/iptables.service → /usr/lib/systemd/system/iptables.service.
[root@stapp03 ~]# systemctl status iptables
● iptables.service - IPv4 firewall with iptables
     Loaded: loaded (/usr/lib/systemd/system/iptables.service; enabled; preset: disabled)
     Active: active (exited) since Sun 2025-09-07 05:12:49 UTC; 11s ago
   Main PID: 3009 (code=exited, status=0/SUCCESS)

Sep 07 05:12:49 stapp03.stratos.xfusioncorp.com systemd[1]: Starting IPv4 firewall with iptables...
Sep 07 05:12:49 stapp03.stratos.xfusioncorp.com systemd[3009]: iptables.service: Executing: /usr/libexec/iptables/iptables.init start
Sep 07 05:12:49 stapp03.stratos.xfusioncorp.com iptables.init[3009]: iptables: Applying firewall rules: [  OK  ]
Sep 07 05:12:49 stapp03.stratos.xfusioncorp.com systemd[1]: iptables.service: Child 3009 belongs to iptables.service.
Sep 07 05:12:49 stapp03.stratos.xfusioncorp.com systemd[1]: iptables.service: Main process exited, code=exited, status=0/SUCCESS (success)
Sep 07 05:12:49 stapp03.stratos.xfusioncorp.com systemd[1]: iptables.service: Changed start -> exited
Sep 07 05:12:49 stapp03.stratos.xfusioncorp.com systemd[1]: iptables.service: Job 290 iptables.service/start finished, result=done
Sep 07 05:12:49 stapp03.stratos.xfusioncorp.com systemd[1]: Finished IPv4 firewall with iptables.
Sep 07 05:12:49 stapp03.stratos.xfusioncorp.com systemd[1]: iptables.service: Failed to send unit change signal for iptables.service: Connection reset by peer
Sep 07 05:12:55 stapp03.stratos.xfusioncorp.com systemd[1]: iptables.service: Changed dead -> exited
[root@stapp03 ~]# iptables -A INPUT -p tcp --destination-port 8088 -s 172.16.238.14 -j ACCEPT
[root@stapp03 ~]# iptables -A INPUT -p tcp --destination-port 8088 -j DROP
[root@stapp03 ~]# iptables -L --line-numbers
Chain INPUT (policy ACCEPT)
num  target     prot opt source               destination         
1    ACCEPT     all  --  anywhere             anywhere             state RELATED,ESTABLISHED
2    ACCEPT     icmp --  anywhere             anywhere            
3    ACCEPT     all  --  anywhere             anywhere            
4    ACCEPT     tcp  --  anywhere             anywhere             state NEW tcp dpt:ssh
5    REJECT     all  --  anywhere             anywhere             reject-with icmp-host-prohibited
6    ACCEPT     tcp  --  stlb01               anywhere             tcp dpt:radan-http
7    DROP       tcp  --  anywhere             anywhere             tcp dpt:radan-http

Chain FORWARD (policy ACCEPT)
num  target     prot opt source               destination         
1    REJECT     all  --  anywhere             anywhere             reject-with icmp-host-prohibited

Chain OUTPUT (policy ACCEPT)
num  target     prot opt source               destination         
[root@stapp03 ~]# iptables -R INPUT 5 -p icmp -j REJECT
[root@stapp03 ~]# iptables -L --line-numbers
Chain INPUT (policy ACCEPT)
num  target     prot opt source               destination         
1    ACCEPT     all  --  anywhere             anywhere             state RELATED,ESTABLISHED
2    ACCEPT     icmp --  anywhere             anywhere            
3    ACCEPT     all  --  anywhere             anywhere            
4    ACCEPT     tcp  --  anywhere             anywhere             state NEW tcp dpt:ssh
5    REJECT     icmp --  anywhere             anywhere             reject-with icmp-port-unreachable
6    ACCEPT     tcp  --  stlb01               anywhere             tcp dpt:radan-http
7    DROP       tcp  --  anywhere             anywhere             tcp dpt:radan-http

Chain FORWARD (policy ACCEPT)
num  target     prot opt source               destination         
1    REJECT     all  --  anywhere             anywhere             reject-with icmp-host-prohibited

Chain OUTPUT (policy ACCEPT)
num  target     prot opt source               destination         
[root@stapp03 ~]#

```

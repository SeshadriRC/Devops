Our monitoring tool has reported an issue in Stratos Datacenter. One of our app servers has an issue, as its Apache service is not reachable on port 5000 (which is the Apache port). The service itself could be down, the firewall could be at fault, or something else could be causing the issue.



Use tools like telnet, netstat, etc. to find and fix the issue. Also make sure Apache is reachable from the jump host without compromising any security settings.

Once fixed, you can test the same using command curl http://stapp01:5000 command from jump host.

Note: Please do not try to alter the existing index.html code, as it will lead to task failure.

## Solution

```
telnet stapp01.stratos.xfusioncorp.com 5000

login as root and check below
systemctl status httpd &&  systemctl start httpd


netstat -tulnp        [ to find port number which is used by process ]
sudo netstat -tulnp | grep 5000

Add below server entry in conf file
vi /etc/httpd/conf/httpd.conf
172.17.0.8:5000

Firewall check
sudo iptables -L -n
sudo iptables -I INPUT -p tcp --dport 8086 -j ACCEPT
sudo iptables -D INPUT 1
sudo iptables -L INPUT -n --line-numbers

```
**Errors**

```
1. (98)Address already in use: AH00072: make_sock: could not bind to address 0.0.0.0:5000
please add the address in conf file

2. If you tried in same server , you will get below error. --> assume service is not running

curl: (7) Failed to connect to stapp02 port 5000: Connection refused

if you tried from to -to server and port is correctly configured then you will get below error
no route exist

```
**Outputs**

```

[root@stapp01 ~]# sudo iptables -L -n
Chain INPUT (policy ACCEPT)
target     prot opt source               destination         
ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0            state RELATED,ESTABLISHED
ACCEPT     icmp --  0.0.0.0/0            0.0.0.0/0           
ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0           
ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            state NEW tcp dpt:22
REJECT     all  --  0.0.0.0/0            0.0.0.0/0            reject-with icmp-host-prohibited

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination         
REJECT     all  --  0.0.0.0/0            0.0.0.0/0            reject-with icmp-host-prohibited

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination         
# Warning: iptables-legacy tables present, use iptables-legacy to see them
[root@stapp01 ~]# sudo iptables -I INPUT -p tcp --dport 8086 -j ACCEPT
[root@stapp01 ~]# sudo iptables -L -n
Chain INPUT (policy ACCEPT)
target     prot opt source               destination         
ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:8086
ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0            state RELATED,ESTABLISHED
ACCEPT     icmp --  0.0.0.0/0            0.0.0.0/0           
ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0           
ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            state NEW tcp dpt:22
REJECT     all  --  0.0.0.0/0            0.0.0.0/0            reject-with icmp-host-prohibited

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination         
REJECT     all  --  0.0.0.0/0            0.0.0.0/0            reject-with icmp-host-prohibited

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination         
# Warning: iptables-legacy tables present, use iptables-legacy to see them

```

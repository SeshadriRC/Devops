That command is **adding a static route** to your system’s routing table. Let’s decode it:

```
sudo ip route add 172.16.238.0/24 via 172.16.239.1
```

* **sudo** → run as root (needed for networking changes).
* **ip route add** → add a new route.
* **172.16.238.0/24** → the destination network (all IPs from 172.16.238.0 to 172.16.238.255).
* **via 172.16.239.1** → the next-hop (gateway) IP to reach that network.

👉 Meaning:
“To reach the `172.16.238.0/24` network, send traffic to the router/gateway at `172.16.239.1`.”

⚠️ Notes:

* Like with `ip addr add`, this route is **temporary** — it disappears after reboot or network restart.
* To make it permanent, you must add it to your network configuration files (e.g., `/etc/sysconfig/network-scripts/route-eth0` on RHEL/CentOS or `/etc/netplan/` on Ubuntu).

Would you like me to show you the exact way to make this route permanent in **CentOS/RHEL** and **Ubuntu**?


**Outputs**

```
thor@app01 ~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
19: eth0@if20: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ac:10:ee:0b brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.16.238.15/24 scope global eth0
       valid_lft forever preferred_lft forever
23: eth1@if24: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1410 qdisc noqueue state UP group default 
    link/ether 02:42:ac:11:00:04 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.17.0.4/16 brd 172.17.255.255 scope global eth1
       valid_lft forever preferred_lft forever
```
```
thor@jumphost ~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
17: eth0@if18: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ac:10:ee:0a brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.16.238.10/24 brd 172.16.238.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet 172.16.239.10/24 scope global eth0
       valid_lft forever preferred_lft forever
29: eth1@if30: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1410 qdisc noqueue state UP group default 
    link/ether 02:42:ac:11:00:07 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.17.0.7/16 brd 172.17.255.255 scope global eth1
       valid_lft forever preferred_lft forever
```

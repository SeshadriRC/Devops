## Assume app is running on 5000

**On powershell as a admin**
ipconfig
192.168.1.2    [ipv4 wifi adapter address]

**connectaddress** -> type ```ip addr``` in ubuntu, you will get it

netsh interface portproxy add v4tov4 listenport=5000 listenaddress=0.0.0.0 connectport=5000 connectaddress=172.28.224.226
netsh advfirewall firewall add rule name="WSL Flask 5000" dir=in action=allow protocol=TCP localport=5000

checking:-
netsh interface portproxy show all
netsh advfirewall firewall show rule name="WSL Flask 5000"

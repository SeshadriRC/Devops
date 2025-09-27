```
sudo apt install -y openssh-server
sudo systemctl start ssh
sudo systemctl status ssh

sudo ufw allow ssh     [ check the status before allowing ]
sudo ufw reload
sudo ufw status

ip addr show eth0

try to ssh , you should able to login
```

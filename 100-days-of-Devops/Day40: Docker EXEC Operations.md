One of the Nautilus DevOps team members was working to configure services on a kkloud container that is running on App Server 1 in Stratos Datacenter. Due to some personal work he is on PTO for the rest of the week, but we need to finish his pending work ASAP. Please complete the remaining work as per details given below:


a. Install apache2 in kkloud container using apt that is running on App Server 1 in Stratos Datacenter.


b. Configure Apache to listen on port 8085 instead of default http port. Do not bind it to listen on specific IP or hostname only, i.e it should listen on localhost, 127.0.0.1, container ip, etc.


c. Make sure Apache service is up and running inside the container. Keep the container in running state at the end.

## Solution

```
docker exec -it  kkloud /bin/bash
apt-get update

dpkg -l | grep apache2
apache2 -v

apt-cache search apache2
apt-cache policy apache2
apt-get install -y apache2

service apache2 status
service apache2 start

apt-get install -y net-tools
netstat -tulnp
ps -ef|grep apache2

/etc/apache2/ports.conf
/etc/apache2/sites-available/000-default.conf

```

## Chatgpt

Got it ✅. Here’s how you can complete the task step by step for the **kkloud** container on **App Server 1**:

---

### **Step 1: Login to App Server 1**

```bash
ssh tony@stapp01
# use the given Nautilus password
```

---

### **Step 2: Access the container**

List containers first and then exec into `kkloud`:

```bash
docker ps
docker exec -it kkloud bash
```

---

### **Step 3: Update and install Apache**

Inside the container:

```bash
apt-get update
apt-get install -y apache2
```

---

### **Step 4: Change Apache port to 8085**

Apache listens by default on port **80**, so we need to update configs:

1. Edit **ports.conf**:

```bash
sed -i 's/Listen 80/Listen 8085/' /etc/apache2/ports.conf
```

2. Edit **default virtual host config**:

```bash
sed -i 's/<VirtualHost \*:80>/<VirtualHost *:8085>/' /etc/apache2/sites-available/000-default.conf
```

---

### **Step 5: Restart Apache**

```bash
service apache2 restart
```

Check status:

```bash
service apache2 status
```

---

### **Step 6: Verify Apache is listening on 8085**

```bash
netstat -tulnp | grep 8085
```

or

```bash
ss -tulnp | grep 8085
```

---

### **Step 7: Keep container running**

Make sure you **exit** the container without stopping it:

```bash
exit
```

Container will remain running with Apache inside.

---

✅ At this point:

* Apache is installed inside **kkloud**.
* It listens on **port 8085** (on all interfaces).
* The service is **running**.
* Container is kept **running**.

---

Do you want me to also give you a **curl command from host** to test Apache inside the container on port **8085**?

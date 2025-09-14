Thanks for sharing the output ✅

That tells us:

* You’re on **RHEL/CentOS/Rocky/Alma 9** (`el9`).
* The **AppStream repo** only goes up to **PHP 8.0.30**.
* That’s why you don’t see **PHP 8.2** — it’s not in the default repos.

To get **PHP 8.2**, you’ll need the **Remi repo**. On EL9 the steps are:

---

### **Step 1. Install Remi repo**

```bash
sudo yum install -y https://rpms.remirepo.net/enterprise/remi-release-9.rpm
```

---

### **Step 2. Reset the current PHP module**

```bash
sudo yum module reset php -y
```

---

### **Step 3. Enable PHP 8.2 stream**

```bash
sudo yum module enable php:remi-8.2 -y
```

---

### **Step 4. Install PHP-FPM 8.2**

```bash
sudo yum install -y php php-fpm
```

---

### **Step 5. Verify**

```bash
php -v
systemctl status php-fpm
```

You should now see **PHP 8.2.x** installed and `php-fpm` available.

---

👉 Do you want me to also give you a **one-shot uninstall + switch + reinstall** sequence, in case PHP 8.0 is already installed and you want to cleanly move to 8.2?

`nginx -t` is a **test command** for Nginx.

It checks the **syntax** and **validity** of the Nginx configuration files before you reload or restart the service.

---

### ✅ What it does:

* Scans `/etc/nginx/nginx.conf` and all included files.
* Reports if there are syntax errors or missing directives.
* If configuration is valid, it confirms:

  ```
  nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
  nginx: configuration file /etc/nginx/nginx.conf test is successful
  ```
* If there’s an error, it will show the file name and line number where the problem exists.

---

### 📌 Example:

```bash
sudo nginx -t
```

Output when correct:

```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

Output when error:

```
nginx: [emerg] unexpected end of file, expecting "}" in /etc/nginx/nginx.conf:45
nginx: configuration file /etc/nginx/nginx.conf test failed
```

---

Would you like me to also explain the difference between `nginx -t` and `systemctl restart nginx`?

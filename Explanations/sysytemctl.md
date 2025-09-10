what is systemctl reload nginx and systemctl restart nginx

Good question 👍 Here’s the difference between the two:

---

### 🔄 `systemctl reload nginx`

* Reloads the **configuration files** without stopping the service.
* Existing connections are **not interrupted**.
* Useful when you just update `/etc/nginx/nginx.conf` or virtual host files.
* Runs faster because it doesn’t restart worker processes from scratch.

👉 Equivalent to:

```bash
nginx -s reload
```

---

### 🔁 `systemctl restart nginx`

* Stops and then starts the Nginx service completely.
* All worker processes are killed and new ones are spawned.
* Any **active client connections** are dropped.
* Use this if the service is stuck, crashed, or you made changes that require a full restart (rare).

---

### 📌 Example:

```bash
sudo systemctl reload nginx   # Apply new config gracefully
sudo systemctl restart nginx  # Full stop and start
```

---

✅ In practice:

* **Use `reload`** after config changes (once `nginx -t` passes).
* **Use `restart`** only when Nginx is misbehaving or after upgrades.

---

Do you want me to also explain when a **reload** might fail and fall back to a **restart**?

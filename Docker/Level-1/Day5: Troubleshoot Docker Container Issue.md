An issue has arisen with a static website running in a container named nautilus on App Server 1. To resolve the issue, investigate the following details:



Check if the container's volume /usr/local/apache2/htdocs is correctly mapped with the host's volume /var/www/html.

Verify that the website is accessible on host port 8080 on App Server 1. Confirm that the command curl http://localhost:8080/ works on App Server 1.

## Solution

Here’s how you should **investigate and verify the issue step by step on App Server 1**.

---

## 1️⃣ Check if the container volume is correctly mapped

First, identify the container details.

```bash
docker ps
```

Confirm the container name is **nautilus**.
Then inspect its volume mapping:

```bash
docker inspect nautilus
```

Look for the **Mounts** section. You should see something like:

```json
"Mounts": [
  {
    "Source": "/var/www/html",
    "Destination": "/usr/local/apache2/htdocs",
    "Mode": "",
    "RW": true,
    "Propagation": "rprivate"
  }
]
```

### ✅ Expected result

* **Source (host):** `/var/www/html`
* **Destination (container):** `/usr/local/apache2/htdocs`

If this mapping is missing or incorrect, the website content will not load.

---

## 2️⃣ Verify Apache content on the host

Ensure the website files exist on the host:

```bash
ls -l /var/www/html
```

You should see files like:

```bash
index.html
```

(Optional test)

```bash
cat /var/www/html/index.html
```

---

## 3️⃣ Check container port mapping (8080)

Verify that port **8080** on the host is mapped to Apache’s port **80** in the container.

```bash
docker ps
```

Expected output (example):

```bash
0.0.0.0:8080->80/tcp
```

If not present, the website won’t be reachable via port 8080.

---

## 4️⃣ Test website access from App Server 1

Run the curl command on **App Server 1**:

```bash
curl http://localhost:8080/
```

### ✅ Expected result

* HTML content of the website is displayed
* OR raw HTML output from `index.html`

Example:

```html
<html>
  <h1>Welcome to Nautilus Static Website</h1>
</html>
```

---

## 5️⃣ If curl does NOT work (quick checks)

* Is the container running?

  ```bash
  docker ps
  ```
* Check container logs:

  ```bash
  docker logs nautilus
  ```
* Ensure Apache is running inside the container:

  ```bash
  docker exec -it nautilus httpd -v
  ```

---

## ✅ Final confirmation checklist

✔ Volume mapped: `/var/www/html → /usr/local/apache2/htdocs`
✔ Port mapped: `8080 → 80`
✔ `curl http://localhost:8080/` works on App Server 1

If any of these fail, that’s the root cause of the issue.

If you want, I can also provide **fix commands** if something is misconfigured.

**Error**

```
[root@stapp01 ~]# docker ps -a CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES 1e6ee6e54a5e httpd "httpd-foreground" 6 minutes ago Exited (0) 6 minutes ago nautilus [root@stapp01 ~]# curl http://localhost:8080/ curl: (7) Failed to connect to localhost port 8080: Connection refused [root@stapp01 ~]#
```


Good, this output clearly shows the **root cause** 👍
Let’s break it down and fix it logically.

---

## 🔍 What the current state tells us

### 1️⃣ Container status

```bash
STATUS: Exited (0)
PORTS: (empty)
```

➡️ **The container is NOT running**, so:

* No service is listening
* No port 8080 is bound
* `curl` fails with *connection refused* (expected)

---

## ❌ Why curl is failing

```bash
curl http://localhost:8080/
```

This fails because:

* Container `nautilus` is **stopped**
* Port **8080 is not exposed/mapped**
* Apache inside the container is not running

---

## ✅ Step-by-step fix

### 1️⃣ Start the container

Try starting it first:

```bash
docker start nautilus
```

Then check status:

```bash
docker ps
```

If it **stays running**, move to Step 3.
If it **exits again**, continue to Step 2.

---

### 2️⃣ Check why the container exited

View logs:

```bash
docker logs nautilus
```

This usually reveals:

* Port binding issues
* Missing volume
* Apache config errors

---

### 3️⃣ Verify port mapping (IMPORTANT)

Your `docker ps -a` output shows **no PORTS**, which means the container was likely started **without `-p 8080:80`**.

If port mapping is missing, you must **recreate** the container.

#### Stop and remove old container:

```bash
docker rm nautilus
```

#### Re-run container with correct config:

```bash
docker run -d \
  --name nautilus \
  -p 8080:80 \
  -v /var/www/html:/usr/local/apache2/htdocs \
  httpd
```

---

## 4️⃣ Verify everything

### Check container is running

```bash
docker ps
```

Expected:

```text
0.0.0.0:8080->80/tcp
```

### Check website files

```bash
ls /var/www/html
```

Make sure `index.html` exists.

### Test website

```bash
curl http://localhost:8080/
```

✅ You should now see the HTML output.

---

## ✅ Final Root Cause Summary (for exams / tickets)

* ❌ Container `nautilus` was **exited**
* ❌ No port **8080:80** mapping existed
* ✅ Recreated container with correct **port and volume mapping**
* ✅ Website accessible via `curl http://localhost:8080/`

---

If you want, I can also explain **why the container exited immediately** once you paste the `docker logs nautilus` output.


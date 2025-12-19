A container named kke-container was created by one of the Nautilus project developers on App Server 1. It was solely for testing purposes and now requires deletion. Execute the following task:



Delete the kke-container on App Server 1 in Stratos DC.

## Solution

To delete the **kke-container** on **App Server 1** in **Stratos DC**, follow these steps:

---

### 1️⃣ SSH into App Server 1

```bash
ssh <username>@app-server-1
```

(Use the credentials provided for the lab)

---

### 2️⃣ Verify the container exists

```bash
docker ps -a | grep kke-container
```

---

### 3️⃣ Delete the container

If the container is **running or stopped**, use:

```bash
docker rm -f kke-container
```

✅ `-f` ensures the container is stopped (if running) and removed.

---

### 4️⃣ Confirm deletion

```bash
docker ps -a | grep kke-container
```

No output means the container is successfully deleted.

---

### ✅ Task Completed

The **kke-container** has been deleted from **App Server 1**.

If this is a Nautilus / KodeKloud-style task and you want **minimal commands only**, the key command is:

```bash
docker rm -f kke-container
```


https://github.com/SeshadriRC/Devops/blob/main/100-days-of-Devops/Day54:%20Kubernetes%20Shared%20Volumes.md



Here’s the complete solution to create and verify the shared volume between two containers in a single pod:  

---

### **Step 1: Create the Pod manifest file**

Create a file named `volume-share-xfusion.yaml` with the following content:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: volume-share-xfusion
spec:
  containers:
    - name: volume-container-xfusion-1
      image: fedora:latest
      command: ["sleep", "3600"]
      volumeMounts:
        - name: volume-share
          mountPath: /tmp/ecommerce

    - name: volume-container-xfusion-2
      image: fedora:latest
      command: ["sleep", "3600"]
      volumeMounts:
        - name: volume-share
          mountPath: /tmp/apps

  volumes:
    - name: volume-share
      emptyDir: {}
```

---

### **Step 2: Create the Pod**
```bash
kubectl apply -f volume-share-xfusion.yaml
```

Check pod status:
```bash
kubectl get pods
```
Wait until the pod shows **Running**.

---

### **Step 3: Exec into the first container**
```bash
kubectl exec -it volume-share-xfusion -c volume-container-xfusion-1 -- bash
```

Inside the container, run:
```bash
cd /tmp/ecommerce
echo "Shared volume test" > ecommerce.txt
ls -l
cat ecommerce.txt
```
Then exit:
```bash
exit
```

---

### **Step 4: Verify from the second container**
```bash
kubectl exec -it volume-share-xfusion -c volume-container-xfusion-2 -- bash
```

Inside the container:
```bash
cd /tmp/apps
ls -l
cat ecommerce.txt
```

✅ You should see the same file (`ecommerce.txt`) and its content.  
That confirms both containers share the same **emptyDir volume**.

---

### **Summary**
- **Pod name:** `volume-share-xfusion`  
- **Volume name:** `volume-share` (type: `emptyDir`)  
- **Container 1 mount path:** `/tmp/ecommerce`  
- **Container 2 mount path:** `/tmp/apps`  
- **Verification:** File created in `/tmp/ecommerce` is visible in `/tmp/apps`.


Good question 👍

That section:

```yaml
volumes:
  - name: volume-share
    emptyDir: {}
```

defines a **Kubernetes volume** of type **`emptyDir`**.

Here’s what it means:

---

### 🔹 `volumes:`

This section lists one or more volumes that the **pod** can use.
All containers inside the pod can access these volumes (if mounted).

---

### 🔹 `name: volume-share`

This is the **logical name** of the volume — containers refer to it under their `volumeMounts` section using this name.

---

### 🔹 `emptyDir: {}`

This defines the **type of volume**.
`emptyDir` means:

* It’s an **empty directory** created on the node when the pod starts.
* It exists **as long as the pod is running**.
* All containers in the same pod can **share files** using this volume.
* When the pod is deleted, the data is **lost** (temporary storage only).

---

### 🧠 Example analogy:

Think of `emptyDir` as a **shared temporary folder** created on the same host where the pod runs.
All containers inside that pod can read/write to this shared folder — but once the pod is gone, so is the data.

---

So in your case:

```yaml
volumes:
  - name: volume-share
    emptyDir: {}
```

creates a **shared in-memory or node-disk-based folder** called `volume-share`,
which both containers mount (`/tmp/ecommerce` and `/tmp/apps`) to share files.



Perfect — in that case, you should **not use `emptyDir`**, because it’s **ephemeral** (data is deleted when the pod is removed).

To **retain data even if the pod is deleted**, you need to use a **PersistentVolume (PV)** and a **PersistentVolumeClaim (PVC)**.

Here’s how you can modify your YAML 👇

---

### ✅ **Updated Pod with Persistent Volume**

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-volume-share
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

Apply the PVC first:

```bash
kubectl apply -f pvc-volume-share.yaml
```

Then create the pod:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: volume-share-xfusion
spec:
  containers:
    - name: volume-container-xfusion-1
      image: fedora:latest
      command: ["sleep", "3600"]
      volumeMounts:
        - name: volume-share
          mountPath: /tmp/ecommerce

    - name: volume-container-xfusion-2
      image: fedora:latest
      command: ["sleep", "3600"]
      volumeMounts:
        - name: volume-share
          mountPath: /tmp/apps

  volumes:
    - name: volume-share
      persistentVolumeClaim:
        claimName: pvc-volume-share
```

---

### 🧠 Explanation

* **PersistentVolumeClaim (PVC):** Requests storage from the cluster’s storage backend (like AWS EBS, NFS, Ceph, etc.).
* **PersistentVolume (PV):** The actual physical storage. Kubernetes binds your PVC to a PV.
* **Mount in Pod:** Both containers mount the same PVC, so data persists and remains shared.

---

### 🔍 Behavior

| Action                  | Data Persistence |
| ----------------------- | ---------------- |
| Pod restarts            | ✅ Data retained  |
| Pod deleted & recreated | ✅ Data retained  |
| PVC deleted             | ❌ Data lost      |

---

So to summarize:

* Use **`emptyDir`** → temporary shared data (deleted with pod)
* Use **`PersistentVolumeClaim`** → persistent shared data (survives pod deletion)

Would you like me to show a **single YAML file** containing both the PVC and pod definitions together?




Good question 👏

In this line:

```yaml
command: ["sleep", "3600"]
```

you are **overriding the container’s default command** and telling it to run `sleep 3600`.

---

### 🔹 Breakdown

* **`command:`** — This defines what command the container should run when it starts.
  (It overrides the default `ENTRYPOINT` in the container image.)

* **`sleep`** — The Linux command that pauses for a specified number of seconds.

* **`3600`** — The number of seconds to sleep (3600 seconds = 1 hour).

So the container runs:

```bash
sleep 3600
```

That means it will **stay alive (idle)** for 1 hour instead of exiting immediately.

---

### 🧠 Why it’s used here

The **Fedora image** doesn’t have a long-running process by default.
If you just start a Fedora container, it exits immediately because there’s nothing to run.

By using:

```yaml
command: ["sleep", "3600"]
```

you keep the container **running and available**, so you can:

* `kubectl exec` into it,
* create or check files,
* test volume sharing.

---

### 💡 Tip

You can also use:

```yaml
command: ["sleep", "infinity"]
```

to keep it running **forever** — useful for testing environments.




Excellent — that’s an important distinction 👏

If you **don’t want the containers to share data**, you simply need to give **each container its own separate volume** (i.e., don’t use the same volume name).

Here’s how 👇

---

### ✅ Example: **Each container gets its own mount (not shared)**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: separate-volumes-xfusion
spec:
  containers:
    - name: container-one
      image: fedora:latest
      command: ["sleep", "3600"]
      volumeMounts:
        - name: volume-one
          mountPath: /tmp/ecommerce

    - name: container-two
      image: fedora:latest
      command: ["sleep", "3600"]
      volumeMounts:
        - name: volume-two
          mountPath: /tmp/apps

  volumes:
    - name: volume-one
      emptyDir: {}
    - name: volume-two
      emptyDir: {}
```

---

### 🔍 Explanation

| Container     | Volume Name | Mount Path     | Shared? |
| ------------- | ----------- | -------------- | ------- |
| container-one | volume-one  | /tmp/ecommerce | ❌ No    |
| container-two | volume-two  | /tmp/apps      | ❌ No    |

Each container now has its **own independent `emptyDir` volume**, so:

* Files created in `/tmp/ecommerce` (container-one) will **not** appear in `/tmp/apps` (container-two).
* Both volumes exist only while the pod is running (since they’re `emptyDir`).

---

### 🧠 Optional:

If you want these to be **persistent and separate**, use **two PVCs** instead of `emptyDir` volumes — exactly the same idea, just with storage that survives pod deletion.

Would you like me to show how that version (persistent, non-shared) would look?


Excellent follow-up, and this is key to understanding how Kubernetes storage works 👇

---

### 🔹 For `emptyDir` Volumes

When you use:

```yaml
emptyDir: {}
```

The storage is taken **from the node (worker machine)** where the pod is running.

Specifically:

* By default, data is stored on the **node’s local disk** (usually under `/var/lib/kubelet/pods/...`).
* The storage is **temporary** — it exists only while the pod runs on that node.
* When the pod is deleted, **the `emptyDir` and all its data are deleted too**.

---

### 🔹 Variants of `emptyDir`

You can optionally specify the medium:

```yaml
emptyDir:
  medium: Memory
```

* This uses **RAM (tmpfs)** instead of disk.
* Faster, but data is **lost on pod restart** and limited by node memory.

Example:

```yaml
volumes:
  - name: temp-memory
    emptyDir:
      medium: Memory
```

---

### 🔹 For PersistentVolume (PV) / PersistentVolumeClaim (PVC)

If you use:

```yaml
persistentVolumeClaim:
  claimName: my-pvc
```

Then the storage is taken from **a PersistentVolume**, which could come from:

* A **cloud storage backend** (EBS in AWS, Persistent Disk in GCP, Azure Disk, etc.)
* A **network storage system** (NFS, Ceph, GlusterFS, etc.)
* Or a **local disk** on a node (if configured as a local PV)

Kubernetes automatically **binds** your PVC to a matching PV that satisfies the requested size and access mode.

---

### 🧠 Summary Table

| Volume Type             | Storage Location               | Lifetime          | Shared Across Containers? | Persistent? |
| ----------------------- | ------------------------------ | ----------------- | ------------------------- | ----------- |
| `emptyDir`              | Node’s local disk              | Pod lifetime      | ✅ If same volume name     | ❌ No        |
| `emptyDir` (Memory)     | Node RAM                       | Pod lifetime      | ✅ If same volume name     | ❌ No        |
| `PersistentVolumeClaim` | Backend storage (EBS/NFS/etc.) | Until PVC deleted | ✅ If same PVC used        | ✅ Yes       |

---

So, in short:

* `emptyDir` → local, temporary space on the **node**.
* `PVC` → backed by **cluster storage**, persists even after pod deletion.

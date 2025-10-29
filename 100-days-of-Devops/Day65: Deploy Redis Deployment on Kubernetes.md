The Nautilus application development team observed some performance issues with one of the application that is deployed in Kubernetes cluster. After looking into number of factors, the team has suggested to use some in-memory caching utility for DB service. After number of discussions, they have decided to use Redis. Initially they would like to deploy Redis on kubernetes cluster for testing and later they will move it to production. Please find below more details about the task:


Create a redis deployment with following parameters:

Create a config map called my-redis-config having maxmemory 2mb in redis-config.

Name of the deployment should be redis-deployment, it should use
redis:alpine image and container name should be redis-container. Also make sure it has only 1 replica.

The container should request for 1 CPU.

Mount 2 volumes:

a. An Empty directory volume called data at path /redis-master-data.

b. A configmap volume called redis-config at path /redis-master.

c. The container should expose the port 6379.

Finally, redis-deployment should be in an up and running state.

## Solution
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-redis-config
data:
  redis-config: |
    maxmemory 2mb
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: redis-container
        image: redis:alpine
        ports:
        - containerPort: 6379
        resources:
          requests:
            cpu: "1"
        volumeMounts:
        - name: data
          mountPath: /redis-master-data
        - name: redis-config
          mountPath: /redis-master
      volumes:
      - name: data
        emptyDir: {}
      - name: redis-config
        configMap:
          name: my-redis-config

```

Perfect 👏 — that YAML you pasted is **exactly** the ConfigMap definition we used earlier.

Let’s go through what each line does clearly so it makes sense to you:

---

### 🧩 **Line-by-line breakdown**

```yaml
apiVersion: v1
kind: ConfigMap
```

➡️ This tells Kubernetes that we’re creating a **ConfigMap** resource.

---

```yaml
metadata:
  name: my-redis-config
```

➡️ This gives the ConfigMap a name — `my-redis-config`.
That’s the name you’ll later reference in your **Deployment** under the `volumes:` section.

---

```yaml
data:
  redis-config: |
    maxmemory 2mb
```

➡️ `data:` is where we define the key–value pairs that go inside the ConfigMap.

Here:

* The **key** is `redis-config` → it becomes the **filename** when mounted in the Pod.
* The **value** (the content under `|`) is the file content:

  ```
  maxmemory 2mb
  ```

So when this ConfigMap is mounted as a volume, inside the container it will look like this:

```
/redis-master/redis-config  → contains the text “maxmemory 2mb”
```

---

### 🧠 Think of it like this:

It’s as if you had created a file manually inside the container:

```bash
echo "maxmemory 2mb" > /redis-master/redis-config
```

…but instead of baking that into the image, you’re letting Kubernetes inject it dynamically.

---

### ✅ To verify after applying:

```bash
kubectl apply -f redis-config.yaml
kubectl get configmap my-redis-config -o yaml
```

You’ll see output showing your ConfigMap content:

```yaml
data:
  redis-config: "maxmemory 2mb\n"
```

---

Would you like me to show how to confirm **inside the running Redis Pod** that the file got mounted correctly (and where exactly it is)?

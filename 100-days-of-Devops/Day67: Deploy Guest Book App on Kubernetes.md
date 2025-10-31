The Nautilus Application development team has finished development of one of the applications and it is ready for deployment. It is a guestbook application that will be used to manage entries for guests/visitors. As per discussion with the DevOps team, they have finalized the infrastructure that will be deployed on Kubernetes cluster. Below you can find more details about it.


BACK-END TIER

Create a deployment named redis-master for Redis master.

a.) Replicas count should be 1.

b.) Container name should be master-redis-xfusion and it should use image redis.

c.) Request resources as CPU should be 100m and Memory should be 100Mi.

d.) Container port should be redis default port i.e 6379.

Create a service named redis-master for Redis master. Port and targetPort should be Redis default port i.e 6379.

Create another deployment named redis-slave for Redis slave.

a.) Replicas count should be 2.

b.) Container name should be slave-redis-xfusion and it should use gcr.io/google_samples/gb-redisslave:v3 image.

c.) Requests resources as CPU should be 100m and Memory should be 100Mi.

d.) Define an environment variable named GET_HOSTS_FROM and its value should be dns.

e.) Container port should be Redis default port i.e 6379.

Create another service named redis-slave. It should use Redis default port i.e 6379.

FRONT END TIER

Create a deployment named frontend.

a.) Replicas count should be 3.

b.) Container name should be php-redis-xfusion and it should use gcr.io/google-samples/gb-frontend@sha256:a908df8486ff66f2c4daa0d3d8a2fa09846a1fc8efd65649c0109695c7c5cbff image.

c.) Request resources as CPU should be 100m and Memory should be 100Mi.

d.) Define an environment variable named as GET_HOSTS_FROM and its value should be dns.

e.) Container port should be 80.

Create a service named frontend. Its type should be NodePort, port should be 80 and its nodePort should be 30009.

Finally, you can check the guestbook app by clicking on App button.

## Solution



**Deployment 1**
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-master
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis-backend
      role: master
  template:
    metadata:
      labels:
        app: redis-backend
        role: master
    spec:
      containers:
      - name: master-redis-xfusion
        image: redis
        ports:
        - containerPort: 6379
        resources:
          requests:
            memory: "100Mi"  # Request 64 MiB of memory
            cpu: "100m"    # Request 250 milliCPU (0.25 CPU cores)
```
**Service 1**
```
apiVersion: v1
kind: Service
metadata:
  name: redis-master
spec:
  selector:
   app: redis-backend
   role: master
  ports:
    - port: 6379
      targetPort: 6379
```

**Deployment 2**

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-slave
spec:
  replicas: 2
  selector:
    matchLabels:
      app: redis-backend
      role: slave
  template:
    metadata:
      labels:
        app: redis-backend
        role: slave
    spec:
      containers:
      - name: slave-redis-xfusion
        image: gcr.io/google_samples/gb-redisslave:v3
        ports:
        - containerPort: 6379
        env:
        - name: GET_HOSTS_FROM
          value: dns
        resources:
          requests:
            memory: "100Mi"  # Request 64 MiB of memory
            cpu: "100m"    # Request 250 milliCPU (0.25 CPU cores)
```

**Service 2**
```
apiVersion: v1
kind: Service
metadata:
  name: redis-slave
spec:
  selector:
   app: redis-backend
   role: slave
  ports:
    - port: 6379
      targetPort: 6379
```
**Deployment 3**
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: guestbook
      tier: frontend
  template:
    metadata:
      labels:
        app: guestbook
        tier: frontend
    spec:
      containers:
      - name: php-redis-xfusion
        image: gcr.io/google-samples/gb-frontend@sha256:a908df8486ff66f2c4daa0d3d8a2fa09846a1fc8efd65649c0109695c7c5cbff
        ports:
        - containerPort: 80
        env:
        - name: GET_HOSTS_FROM
          value: dns
        resources:
          requests:
            memory: "100Mi"  # Request 64 MiB of memory
            cpu: "100m"    # Request 250 milliCPU (0.25 CPU cores)

```
**Service 3**
```
apiVersion: v1
kind: Service
metadata:
  name: frontend
spec:
  type: NodePort
  selector:
   app: guestbook
   tier: frontend
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30009
```


Excellent question — and it’s one that shows you’re thinking **like a DevOps engineer**, not just applying YAMLs. 👏

Let’s break it down clearly.

---

### 🧩 1. How frontend connects to backend

The **frontend** (PHP app) connects to **Redis** using **Kubernetes Service DNS names** — *no manual linking needed*.

Each **Service** you create in Kubernetes automatically gets a **DNS name** inside the cluster.

So in your setup:

* `redis-master` → exposed as `redis-master.default.svc.cluster.local`
* `redis-slave` → exposed as `redis-slave.default.svc.cluster.local`

---

### ⚙️ 2. How the frontend knows those names

The frontend image (`gcr.io/google-samples/gb-frontend...`) is **preconfigured** to look for:

* `redis-master` as the primary Redis
* `redis-slave` as the replica

So when it starts up, it automatically tries to connect to:

```
redis-master:6379
redis-slave:6379
```

Those hostnames resolve automatically inside the Kubernetes cluster through CoreDNS.

---

### 🧠 3. Why `GET_HOSTS_FROM=dns`

That environment variable (`GET_HOSTS_FROM=dns`) tells the app and Redis slave containers **to resolve service names via DNS**, rather than using environment variables or IPs.

So:

* `redis-slave` pods connect to the `redis-master` service via DNS.
* `frontend` pods connect to both `redis-master` and `redis-slave` via DNS.

---

### 🧱 4. Example internal connection flow

```
frontend pod
   |
   ├── connects to redis-master:6379  (writes)
   └── connects to redis-slave:6379   (reads)
```

Both of those names are automatically resolvable because you created corresponding **Services** named `redis-master` and `redis-slave`.

---

### ✅ Summary

| Component   | Connects To               | How                      | Why It Works                              |
| ----------- | ------------------------- | ------------------------ | ----------------------------------------- |
| redis-slave | redis-master              | via DNS (`redis-master`) | Uses `GET_HOSTS_FROM=dns`                 |
| frontend    | redis-master, redis-slave | via DNS                  | Kubernetes internal DNS resolves Services |

---

If you want to confirm it inside the cluster:

```bash
kubectl exec -it <frontend-pod-name> -- nslookup redis-master
kubectl exec -it <frontend-pod-name> -- nslookup redis-slave
```

You’ll see both resolve to the correct **ClusterIP** of their respective Services.

Would you like me to show a small diagram of how the frontend, redis-master, and redis-slave communicate inside the cluster?

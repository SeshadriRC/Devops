The Nautilus DevOps team is planning to deploy some micro services on Kubernetes platform. The team has already set up a Kubernetes cluster and now they want to set up some namespaces, deployments etc. Based on the current requirements, the team has shared some details as below:

Create a namespace named dev and deploy a POD within it. Name the pod dev-nginx-pod and use the nginx image with the latest tag. Ensure to specify the tag as nginx:latest.

Note: The kubectl utility on jump_host is configured to operate with the Kubernetes cluster.


Got it 👍 Let’s do this step by step.

You need to:

1. Create a **namespace** `dev`.
2. Deploy a **pod** named `dev-nginx-pod` inside that namespace.
3. Use the **nginx\:latest** image.

---

### Solution

#### 1. Create namespace

```bash
kubectl create namespace dev
```

#### 2. Create the pod

You can either run `kubectl run` or use a YAML manifest.

**Option A – Direct command**

```bash
kubectl run dev-nginx-pod --image=nginx:latest -n dev
```

**Option B – YAML manifest (recommended)**
Create a file `dev-nginx-pod.yaml`:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: dev-nginx-pod
  namespace: dev
spec:
  containers:
  - name: nginx
    image: nginx:latest
```

Apply it:

```bash
kubectl apply -f dev-nginx-pod.yaml
```

---

#### 3. Verify

```bash
kubectl get pods -n dev
```

Expected output:

```
NAME             READY   STATUS    RESTARTS   AGE
dev-nginx-pod    1/1     Running   0          10s
```

---

✅ That completes the task.

Do you want me to write everything in a **single YAML manifest (namespace + pod)** so you can apply it in one go instead of two steps?

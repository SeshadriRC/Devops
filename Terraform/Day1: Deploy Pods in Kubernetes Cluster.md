The Nautilus DevOps team is diving into Kubernetes for application management. One team member has a task to create a pod according to the details below:


Create a pod named pod-httpd using the httpd image with the latest tag. Ensure to specify the tag as httpd:latest.

Set the app label to httpd_app, and name the container as httpd-container.

Note: The kubectl utility on jump_host is configured to operate with the Kubernetes cluster.

**Answer** 

Here’s the YAML you can use:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-httpd
  labels:
    app: httpd_app
spec:
  containers:
    - name: httpd-container
      image: httpd:latest
```

---

### Steps to apply it:

1. Create the YAML file:

   ```bash
   vi pod-httpd.yaml
   ```

   (Paste the above content and save.)

2. Apply the manifest:

   ```bash
   kubectl apply -f pod-httpd.yaml
   ```

3. Verify the pod:

   ```bash
   kubectl get pods -o wide
   ```

You should see a pod named **pod-httpd** running with image `httpd:latest`.

---

⚡ If you prefer to do it in **one command** without YAML:

```bash
kubectl run pod-httpd --image=httpd:latest --restart=Never --labels=app=httpd_app --overrides='
{
  "apiVersion": "v1",
  "spec": {
    "containers": [{
      "name": "httpd-container",
      "image": "httpd:latest"
    }]
  }
}'
```

Would you like me to give you the **short one-liner command** (quick way), or do you want to stick with the **YAML manifest** (best practice for real DevOps work)?


## Used commands

**To get the current namespace**
kubectl config get-contexts

**create a pod**
kubectl apply -f pod.yaml

The Nautilus DevOps team is delving into Kubernetes for app management. One team member needs to create a deployment following these details:


Create a deployment named nginx to deploy the application nginx using the image nginx:latest (ensure to specify the tag)


kubectl create deployment nginx --image=nginx:latest --dry-run=client -o yaml
kubectl create deployment nginx --image=nginx:latest

```yaml
thor@jumphost ~$ kubectl create deployment nginx --image=nginx:latest --dry-run=client -o yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: nginx
  name: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: nginx
    spec:
      containers:
      - image: nginx:latest
        name: nginx
        resources: {}
status: {}
```

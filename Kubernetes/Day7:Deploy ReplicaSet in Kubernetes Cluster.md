The Nautilus DevOps team is gearing up to deploy applications on a Kubernetes cluster for migration purposes. A team member has been tasked with creating a ReplicaSet outlined below:

Create a ReplicaSet using httpd image with latest tag (ensure to specify as httpd:latest) and name it httpd-replicaset.

Apply labels: app as httpd_app, type as front-end.

Name the container httpd-container. Ensure the replica count is 4.

Note: The kubectl utility on jump_host is set up to interact with the Kubernetes cluster.

## Solution

```yaml

apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: httpd-replicaset
  labels:
    app: httpd_app
    type: front-end
spec:
  replicas: 4 # Desired number of Pod replicas
  selector:
    matchLabels:
      app: httpd_app # Labels used to select Pods managed by this ReplicaSet
  template: # Pod template for creating new Pods
    metadata:
      labels:
        app: httpd_app
        type: front-end
    spec:
      containers:
      - name: httpd-container
        image: httpd:latest
```
```
kubectl create -f replica.yaml

```

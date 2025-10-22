Some of the Nautilus team developers are developing a static website and they want to deploy it on Kubernetes cluster. They want it to be highly available and scalable. Therefore, based on the requirements, the DevOps team has decided to create a deployment for it with multiple replicas. Below you can find more details about it:


Create a deployment using nginx image with latest tag only and remember to mention the tag i.e nginx:latest. Name it as nginx-deployment. The container should be named as nginx-container, also make sure replica counts are 3.

Create a NodePort type service named nginx-service. The nodePort should be 30011.

## Solution

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: my-app
spec:
  replicas: 3 # Desired number of pod replicas
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: nginx-container
        image: nginx:latest # Replace with your application image
        ports:
          - containerPort: 80

---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort # Or ClusterIP, LoadBalancer, ExternalName
  selector:
    app: my-app # Selects pods with this label
  ports:
    - port: 82
      targetport: 80 
      nodeport: 3011
  
```


Good question 👍

In your YAML:

```yaml
ports:
  - port: 82
    targetPort: 80 
    nodePort: 3011
```

Here’s what each field means:

| Field              | Description                                                                                                                              |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------- |
| **port: 82**       | The **port number exposed by the Service** *inside the cluster*. Other pods in the same cluster can access this Service using port `82`. |
| **targetPort: 80** | The **port on the container (Pod)** where the actual application (like Nginx) is listening.                                              |
| **nodePort: 3011** | The **port on each node’s IP** through which you can access the Service *from outside the cluster*.                                      |

### Example flow

If your pod runs Nginx on port `80`,

* Inside the cluster, other pods can call:
  `http://nginx-service:82`
* Outside the cluster (if NodePort is used), you can call:
  `http://<NodeIP>:3011`

So, **`port: 82` is the Service’s internal port**, not the container’s port.


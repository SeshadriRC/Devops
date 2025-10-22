The Nautilus DevOps teams is planning to set up a Grafana tool to collect and analyze analytics from some applications. They are planning to deploy it on Kubernetes cluster. Below you can find more details.



1.) Create a deployment named grafana-deployment-devops using any grafana image for Grafana app. Set other parameters as per your choice.


2.) Create NodePort type service with nodePort 32000 to expose the app.


You need not to make any configuration changes inside the Grafana app once deployed, just make sure you are able to access the Grafana login page.

## Solution

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: grafana-deployment-devops
  labels:
    app: my-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: grafana-deployment-container
        image: grafana/grafana:latest # Replace with your application image
        ports:
          - containerPort: 3000
```

```
apiVersion: v1
kind: Service
metadata:
  name: grafana-service
spec:
  type: NodePort # Or ClusterIP, LoadBalancer, ExternalName
  selector:
    app: my-app # Selects pods with this label
  ports:
    - port: 82
      targetPort: 3000
      nodePort: 32000
```

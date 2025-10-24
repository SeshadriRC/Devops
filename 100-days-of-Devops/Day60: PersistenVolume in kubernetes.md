The Nautilus DevOps team is working on a Kubernetes template to deploy a web application on the cluster. There are some requirements to create/use persistent volumes to store the application code, and the template needs to be designed accordingly. Please find more details below:


Create a PersistentVolume named as pv-datacenter. Configure the spec as storage class should be manual, set capacity to 5Gi, set access mode to ReadWriteOnce, volume type should be hostPath and set path to /mnt/security (this directory is already created, you might not be able to access it directly, so you need not to worry about it).

Create a PersistentVolumeClaim named as pvc-datacenter. Configure the spec as storage class should be manual, request 3Gi of the storage, set access mode to ReadWriteOnce.

Create a pod named as pod-datacenter, mount the persistent volume you created with claim name pvc-datacenter at document root of the web server, the container within the pod should be named as container-datacenter using image httpd with latest tag only (remember to mention the tag i.e httpd:latest).

Create a node port type service named web-datacenter using node port 30008 to expose the web server running within the pod.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.

## Solution
---
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-datacenter
spec:
  storageClassName: manual
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /mnt/security
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-datacenter
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 3Gi
---
apiVersion: v1
kind: Pod
metadata:
  name: pod-datacenter
  labels:
    app: datacenter    # ✅ Added label for service selector
spec:
  containers:
    - name: container-datacenter
      image: httpd:latest
      ports:
        - containerPort: 80
      volumeMounts:
        - name: datacenter-storage
          mountPath: /usr/local/apache2/htdocs
  volumes:
    - name: datacenter-storage
      persistentVolumeClaim:
        claimName: pvc-datacenter
---
apiVersion: v1
kind: Service
metadata:
  name: web-datacenter
spec:
  type: NodePort
  selector:
    app: datacenter     # ✅ Matches the Pod label
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30008

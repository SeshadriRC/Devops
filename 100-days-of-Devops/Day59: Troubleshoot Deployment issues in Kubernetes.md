Last week, the Nautilus DevOps team deployed a redis app on Kubernetes cluster, which was working fine so far. This morning one of the team members was making some changes in this existing setup, but he made some mistakes and the app went down. We need to fix this as soon as possible. Please take a look.



The deployment name is redis-deployment. The pods are not in running state right now, so please look into the issue and fix the same.


Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.

## Solution

```
k describe pod redis-deployment-5bcd4c7d64-g8gz4 -> config map not found error
k get deployments.apps redis-deployment -o yaml >> redis-dep.yaml
k replace -f redis-dep.yaml
kubectl create -f pod.yaml --dry-run=client
```

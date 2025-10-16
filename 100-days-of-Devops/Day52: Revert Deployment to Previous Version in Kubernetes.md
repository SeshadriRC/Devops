Earlier today, the Nautilus DevOps team deployed a new release for an application. However, a customer has reported a bug related to this recent release. Consequently, the team aims to revert to the previous version.


There exists a deployment named nginx-deployment; initiate a rollback to the previous revision.

Note: The kubectl utility on jump_host is configured to interact with the Kubernetes cluster.

## Solution

```
k rollout history deployment/nginx-deployment
k rollout status deployment/nginx-deployment
k rollout undo deployment/nginx-deployment
k rollout history deployment/nginx-deployment
```

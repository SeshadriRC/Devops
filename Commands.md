

- [Kubernetes](#kubernetes)
- [Linux](#linux)



## Kubernetes

**view current ns**
kubectl config view --minify --output 'jsonpath={..namespace}'  

**set to ns**
kubectl config set-context --current --namespace=<your-namespace-name>

**Deployment**
```
kubectl get deploy

kubectl get pods

kubectl set image deployment nginx-deployment nginx-container=nginx:1.19
- kubectl set image deployment <deployment-name> <container-name>=<new-container-name with tag>

kubectl rollout status deployment/nginx-deployment
```
## Linux

**Crontab**

```
crontab -e
*/5 * * * * echo hello > /tmp/cron_text

crontab -l

```
**Systemctl**

```
systemctl status crond.service

```

**Yum**

```
rpm -qa | grep selinux
yum -y install selinux* --skip-broken
yum -y install cronie

```


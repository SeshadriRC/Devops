

- [Kubernetes](#kubernetes)
- [Linux](#linux)


## Kubernetes

**Cronjob**

```
kubectl get cronjob
```

**Deployment**
```
kubectl get deploy

kubectl get pods

kubectl set image deployment nginx-deployment nginx-container=nginx:1.19
- kubectl set image deployment <deployment-name> <container-name>=<new-container-name with tag>

kubectl rollout undo deployment  nginx-deployment

kubectl rollout status deployment/nginx-deployment

```

**Namespace**

***view current ns***
```
kubectl config view --minify --output 'jsonpath={..namespace}'  
```
***set current ns***
```
kubectl config set-context --current --namespace=<your-namespace-name>
```

## Linux

**Crontab**

```
crontab -e
*/5 * * * * echo hello > /tmp/cron_text

crontab -l

```

**List**

```
ls
ls -ld   (for directory)
```

**SSH**

```
ssh-keygen -t rsa
ssh-copy-id tony@stapp01

```
**SCP**
```
scp /backup/xfusioncorp_blog.zip clint@stbkp01.stratos.xfusioncorp.com:/backup
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

**zip**

```
zip -r /backup/xfusioncorp_blog.zip /var/www/html/blog
unzip -l xfusioncorp_blog.zi
```



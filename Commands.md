

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
**Pod**

**Edit Pod**
```
kubectl edit pod <pod-name>
```

***Login to container***

```
kubectl exec -it -n <ns> <pod> -- /bin/bash                       [ if single container ]
kubectl exec -it -n <ns> <pod> -c <container-name> -- /bin/sh     [ if multiple container available ]
kubectl exec -n nautilus <ns> <pod> -- cat /opt/data/time/time-check.log  [ Execute a command ]

```

**Yaml**

```
kubectl get pod nginx-phpfpm -o yaml  > /tmp/nginx.yaml
```


## Linux

**Crontab**

```
crontab -e
*/5 * * * * echo hello > /tmp/cron_text

crontab -l

```

**Curl**

```
 curl -ivk http://stapp02:5001
```

**List**

```
ls
ls -ld   (for directory)
```

**Network**

```
telnet stapp01.stratos.xfusioncorp.com 5000
netstat -tulnp

Allow the specific port
========================
sudo iptables -L -n
sudo iptables -I INPUT -p tcp --dport 8086 -j ACCEPT
sudo iptables -D INPUT 1
sudo iptables -L INPUT -n --line-numbers
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
yum list available selinux\* --> list the available packages

```

**Zip**

```
zip -r /backup/xfusioncorp_blog.zip /var/www/html/blog
unzip -l xfusioncorp_blog.zi
```



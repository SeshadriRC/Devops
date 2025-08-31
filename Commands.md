
- [Crontab](#crontab)
- [Kubernetes](#kubernetes)
- [Linux](#linux)

## Crontab

```
crontab -l
crontab -e
```

## Kubernetes

**view current ns**
kubectl config view --minify --output 'jsonpath={..namespace}'  

**set to ns**
kubectl config set-context --current --namespace=<your-namespace-name>

## Linux

**Yum**

```
rpm -qa | grep selinux
yum -y install selinux* --skip-broken
yum -y install cronie

```

**Systemctl**

```
systemctl status crond.service

```



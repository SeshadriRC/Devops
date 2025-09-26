

- [Docker](#docker)
- [Kubernetes](#kubernetes)
- [Linux](#linux)
- [Git](#git)
- [Ansible](#ansible)

## Docker

**Docker Attach**
```
docker attach <container-id>
```
**Docker Build**
```
docker build . -t <accountname>/<appname>
```
**Docker exec**
```
docker exec <container-id> cat /etc/*release*    [run the command inside running container]
```
**Docker Host**
```
docker -H=10.123.2.1:2375 run nginx
```
**Docker Images**
```
docker images
```
**Docker Inspect**
```
docker inspect <container-name>      [ it will show the details of the container ]
```
**Docker Link**
```
docker run -p 38080:8080 --name webapp -e DB_Host=mysql-db -e DB_Password=db_pass123 --network=wp-mysql-network --link mysql-db:mysql-db -d kodekloud/simple-webapp-mysql            [ here first mysql-db -> name of the container, second mysql-db -> hostname ]
```
**Docker Login**
```
docker login       [then automatically it will ask username and password]
docker login private-registry.io
```
**Docker Logs**
```
docker logs <container-name>
```
**Docker Network**
```
docker network inspect bridge
docker network ls
docker run --name alpine-2 --network=none alpine
docker network create --driver bridge --subnet 182.18.0.0/24 --gateway 182.18.0.1 wp-mysql-network
```



**Docker Push/Pull**
```
docker push <accountname>/<appname>
docker pull <image-name>
```
**Docker ps**
```
docker ps
docker ps -a

```

**Docker Run**
```
docker run <image-name>
docker run -d <image> sleep 20000    [ it is in seconds, -d indicates its in background ]
docker run -d --name <container-name> nginx:1.14-alpine    [name the container]
docker run python:3.6 cat /etc/*release*
docker run -it <image> bash       [it will login inside container]
docker run -v /opt/datadir:/var/lib/mysql mysql          [ , here /opt/datadir is outside container directory and /var/lib is inside container this is called bind mounting where /opt/datadir is other location not default docker location ]
docker run --mount type=bind, source=/data/mysql, target=/var/lib/mysql mysql       [this is called new method ]
docker run -p 80:5000 <image-name>    [ port mapping, 80 - host port, 5000 - container port ]
docker run -p 38282:8080 --name blue-app -e APP_COLOR=blue -d kodekloud/simple-webapp
```
**Docker Remove**
```
docker rm <docker-id>      [removing containers]
docker rm <docker-id> <docker-id1>

docker rmi <image-name>    [removing image]
```
**Docker Stop**
```
docker stop <container-name> 
docker stop <container-name1> <container-name2> <container-name3>
```
**Docker Volume**
```
docker volume create datadir-1      [ this is called bind mounting, it will get created in default location -  /var/lib/docker/volumes ]

```
## Kubernetes

**Cluster Info**
```
kubectl cluster-info
```
**Cronjob**

```
kubectl get cronjob
```

**Deployment**
```
kubectl get deploy

kubectl edit deploy <deploy-name> --record

kubectl set image deployment nginx-deployment nginx-container=nginx:1.19
- kubectl set image deployment <deployment-name> <container-name>=<image with tag>


kubectl rollout undo deployment/nginx-deployment

kubectl rollout status deployment/nginx-deployment
kubectl rollout history deployment/<deploy-name>

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
**ReplicaSet**
```
kubectl get rs
kubectl delete replicaset <replica-set-name>
kubectl replace -f <filename>
kubectl scale replicaset <replicaset-name> --replicas=6 
```
**Pod**

**create**
```
kubectl create -f pod.yaml
kubectl create -f deploy.yaml --record
kubectl apply -f pod.yaml
```

**describe**
```
kubectl describe pod <pod-name>
```

**delete**
```
kubectl delete pod <pod-name>
```
***get pods***

```
kubectl get pod,svc
kubectl get pods -o wide
kubectl get pods -l app=orage-app-t4q6      [using label]
```

**svc**

```
kubectl get svc
```

***Edit Pod***
```
kubectl edit pod <pod-name>
```

**kubectl run**
```
kubectl run webapp --image=nginx
kubectl run redis --image=redis123 --dry-run=client -o yaml > redis-definition.yaml

```

***Login to container***

```
kubectl exec -it -n <ns> <pod> -- /bin/bash                       [ if single container ]
kubectl exec -it -n <ns> <pod> -c <container-name> -- /bin/sh     [ if multiple container available ]
kubectl exec -n nautilus <ns> <pod> -- cat /opt/data/time/time-check.log  [ Execute a command ]

```

***copy file to container***

```
kubectl cp  /dir1/file.txt <pod-name>:/dir2/dir3 -c <container-name>
 --kubectl cp  /home/thor/index.php  nginx-phpfpm:/var/www/html -c nginx-container

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
- [Add-ip](https://github.com/SeshadriRC/Devops/blob/main/Explanations/Adding%20a%20static%20route.md)

```
telnet stapp01.stratos.xfusioncorp.com 5000
netstat -tulnp
(or)
ss -tulnp|grep httpd

Allow the specific port
========================
sudo iptables -L -n
sudo iptables -I INPUT -p tcp --dport 8086 -j ACCEPT
sudo iptables -D INPUT 1
sudo iptables -L INPUT -n --line-numbers

Route table
==========
route

show the ip address
==================
ip a

assign ip address to host
=========================
sudo ip addr add 172.16.238.15/24 dev eth0

Add ip to route table
=====================
sudo ip route add 172.16.238.0/24 via 172.16.239.1
[https://github.com/SeshadriRC/Devops/blob/main/Explanations/Adding%20a%20static%20route.md]

```

**Nginx**

- [Nginx (-t)](https://github.com/SeshadriRC/Devops/blob/main/Explanations/nginx.md#nginx-t)


```
nginx -t

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

- [Systemctl](https://github.com/SeshadriRC/Devops/blob/main/Explanations/systemctl.md)

```
systemctl status crond.service

```


**RPM and Yum**

```
rpm -qa | grep selinux
sudo rpm -i <package-name>      [install]
sudo rpm -e <package-name>      [uninstall]
yum -y install selinux* --skip-broken

yum -y install cronie
sudo yum remove -y <package>
yum list available selinux\* --> list the available packages

```

**Zip**

```
zip -r /backup/xfusioncorp_blog.zip /var/www/html/blog
unzip -l xfusioncorp_blog.zi
```

**Sudo**
```
getent group sudo
sudo su -
```

## Git

**Git branch**
```
git branch
git branch branch-1
git branch -vv
git branch -d branch-1 branch-2
```
**Git cherrypick**
```
git cherry-pick <commit-hash>
```
**Git clone**
```
git clone /opt/blog.git /usr/src/kodekloudrepos/blog
```
**Git config**
```
git config user.email
git config user.name "max"
```

**Git Fetch**
```
git fetch origin master
git merge origin/master
```

**Git remote**
```
git remote -v
git remote add dev_apps /opt/xfusioncorp_apps.git
git push -u dev_apps  master
git push origin master --force

```

**Git reset**
```
git reset --hard <commit-hash>            [it will do reset until that commit-hash, so that commit has and below that commits will be present]
```

**Git log**
```
git log --name-only
git log --graph --decorate
git log --oneline --graph --all
git log feature --oneline
git log origin/master --oneline

```
**Git stash**
```
git stash list
git stash apply stash@{1}
```

## Ansible

**playbook**
```
ansible-playbook <file.yaml>
ansible-playbook <file.yaml> --check

ansible-playbook configure_database.yml --check --diff
ansible-playbook --syntax-check configure_database.yml
ansible-lint database_setup.yml

ansible-playbook -i inventory update-service.yml
```

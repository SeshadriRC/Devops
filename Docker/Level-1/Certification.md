1. The Nautilus DevOps team is testing some applications deployment on some of the application servers. They need to deploy a nginx container on Application Server 3. Please complete the task as per details given below:


On Application Server 3 create a container named nginx_3 using image nginx with alpine tag and make sure container is in running state.

# Solution

```
[banner@stapp03 ~]$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[banner@stapp03 ~]$ docker run -d --name nginx_3 nginx:alpine
Unable to find image 'nginx:alpine' locally
alpine: Pulling from library/nginx
1074353eec0d: Pull complete 
25f453064fd3: Pull complete 
567f84da6fbd: Pull complete 
da7c973d8b92: Pull complete 
33f95a0f3229: Pull complete 
085c5e5aaa8e: Pull complete 
0abf9e567266: Pull complete 
d145d80cb745: Pull complete 
Digest: sha256:c083c3799197cfff91fe5c3c558db3d2eea65ccbbfd419fa42a64d2c39a24027
Status: Downloaded newer image for nginx:alpine
e6e7c9447ba2c81730ddd22b6b982259c0d98d54e4816aada25b42a3eb1dc9da
[banner@stapp03 ~]$ docker ps
```
---

2. The Nautilus team wants to create a debug container on Application Server 3. However, they had some specific requirements related to the CMD. Please complete the task as per details given below:


a. On Application Server 3 create a container named debug_3 using image ubuntu/apache2:latest.

b. Overwrite the default CMD with command sleep 1000.

c. Make sure the container is in running state.

# Solution

- Any command specified after the image name in docker run overrides the image’s default CMD.
  
```
[banner@stapp03 ~]$ docker run -d --name debug_3 ubuntu/apache2:latest sleep 1000
Unable to find image 'ubuntu/apache2:latest' locally
latest: Pulling from ubuntu/apache2
bf1561367389: Pull complete 
c7e8d27e04b0: Pull complete 
8a4d5fdfb6a1: Pull complete 
Digest: sha256:bd68b3b35b01aacb11148ad9073c538a6cf59e5bc1dbfc9fe03b2f8212cbd963
Status: Downloaded newer image for ubuntu/apache2:latest
c3135077bac134510d5bab04f428b6b95565ef888857e71adc838c13e201a2bd
```

---

3. We received a request to copy some of the data from one of the docker containers to the docker host. The container is running on App Server 3 in Stratos Datacenter. Below are more details about the task:


On App Server 3 in Stratos Datacenter copy an encrypted file /tmp/test.txt.gpg from development_3 docker container to the docker host in /tmp location. Please do not try to modify this file in any way.

# Solution

```
[banner@stapp03 ~]$ docker cp development_3:/tmp/test.txt.gpg /tmp
Successfully copied 2.05kB to /tmp
[banner@stapp03 ~]$ cd /tmp
[banner@stapp03 tmp]$ ls test.txt.gpg
test.txt.gpg
```

---

4. The Nautilus DevOps team has some confidential data present on App Server 3 in Stratos Datacenter. There is a container ubuntu_latest running on the same server. We received a request to copy some of the data from the docker host to the container. Below are more details about the task:



On App Server 3 in Stratos Datacenter copy an encrypted file /tmp/nautilus.txt.gpg from docker host to ubuntu_latest container (running on same server) in /home/ location (create this location if doesn't exit). Please do not try to modify this file in any way.

# Solution

- Ensure the destination directory exists inside the container
  
```
[banner@stapp03 tmp]$ docker exec -it ubuntu_latest /bin/bash
root@2424c8c6472f:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@2424c8c6472f:/# cd home/
root@2424c8c6472f:/home# ls
ubuntu
root@2424c8c6472f:/home# pwd
/home

docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/home/nautilus.txt.gpg
```

---

5. The DevOps team is performing some cleanup on all app servers in Stratos DC. They want to clean up some unwanted docker images from these servers, some images might be in use by some docker containers, but those containers are not in use so we need to clean those containers and the images. Below are the images that need to be deleted from Application Server 3:


a. alpine:3.18.4

b. sebp/lighttpd:latest

# Solution

- Docker images cannot be removed if containers depend on them, so containers must be stopped and removed first.
- ancestor = the image a container is based on

- Check containers using the target images (optional but safe)
  
```
docker ps -a --filter ancestor=alpine:3.18.4
docker ps -a --filter ancestor=sebp/lighttpd:latest

[banner@stapp03 tmp]$ docker ps -a --filter ancestor=alpine:3.18.4
docker ps -a --filter ancestor=sebp/lighttpd:latest
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
CONTAINER ID   IMAGE                  COMMAND      CREATED         STATUS                       PORTS     NAMES
3d86ffe4c166   sebp/lighttpd:latest   "start.sh"   3 minutes ago   Exited (255) 3 minutes ago             lighttpd
[banner@stapp03 tmp]$ 
```

- Stop containers using these images

```
docker stop $(docker ps -aq --filter ancestor=alpine:3.18.4)
docker stop $(docker ps -aq --filter ancestor=sebp/lighttpd:latest)

3d86ffe4c166   sebp/lighttpd:latest   "start.sh"   3 minutes ago   Exited (255) 3 minutes ago             lighttpd
[banner@stapp03 tmp]$ docker stop $(docker ps -aq --filter ancestor=alpine:3.18.4)
"docker stop" requires at least 1 argument.
See 'docker stop --help'.

Usage:  docker stop [OPTIONS] CONTAINER [CONTAINER...]

Stop one or more running containers
[banner@stapp03 tmp]$ docker stop $(docker ps -aq --filter ancestor=sebp/lighttpd:latest)
3d86ffe4c166

```

- Remove containers using these images

```
docker rm $(docker ps -aq --filter ancestor=alpine:3.18.4)
docker rm $(docker ps -aq --filter ancestor=sebp/lighttpd:latest)

[banner@stapp03 tmp]$ docker rm $(docker ps -aq --filter ancestor=alpine:3.18.4)
"docker rm" requires at least 1 argument.
See 'docker rm --help'.

Usage:  docker rm [OPTIONS] CONTAINER [CONTAINER...]

Remove one or more containers
[banner@stapp03 tmp]$ docker rm $(docker ps -aq --filter ancestor=sebp/lighttpd:latest)
3d86ffe4c166
```

- Remove the Docker images

```
docker rmi alpine:3.18.4
docker rmi sebp/lighttpd:latest

[banner@stapp03 tmp]$ docker rmi alpine:3.18.4
docker rmi sebp/lighttpd:latest
Untagged: alpine:3.18.4
Untagged: alpine@sha256:eece025e432126ce23f223450a0326fbebde39cdf496a85d8c016293fc851978
Deleted: sha256:8ca4688f4f356596b5ae539337c9941abc78eda10021d35cbc52659c74d9b443
Deleted: sha256:cc2447e1835a40530975ab80bb1f872fbab0f2a0faecf2ab16fbbb89b3589438
Untagged: sebp/lighttpd:latest
Untagged: sebp/lighttpd@sha256:43d2a1c6d6a2ef85cc1914daa7dbb16776b2ec8bd0e0e7b824caa9bc5079773c
Deleted: sha256:fbbc9e0f56f7dabf00f0053a26635af09476c6baa0fa120f25044273e8d2b39a
Deleted: sha256:b2d27870bbf07b793e052b009e13a0ce96ad28166e5bf3a08f515019309c4297
Deleted: sha256:ee2ee4421825006b870763bfa63dcada0d9959be8ba4e47d9d8bc598e3965cd3
Deleted: sha256:db639f2ad8b2a4866e264776f72d77cd6c0ea14daf0a62d43a88bb57c76cbb55
Deleted: sha256:24302eb7d9085da80f016e7e4ae55417e412fb7e0a8021e95e3b60c67cde557d
[banner@stapp03 tmp]$ 
```

- Verify cleanup

```
[banner@stapp03 tmp]$ docker images | grep -E "alpine|sebp"
nginx            alpine    b9d44994d8ad   2 days ago    61.9MB
httpd            alpine    a505311a65f7   3 weeks ago   67.1MB
```

6. The Nautilus DevOps team was testing a custom container on Application Server 3 in Stratos DC. They were able to configure it as per their requirements, now they wanted to create an image from this container.


a. The name of the container is alpine_nautilus.

b. Create an image alpine:nautilus from this container.

# Solution

- docker commit takes a snapshot of a container’s filesystem and configuration and saves it as a new image.

- Verify the container exists and is running
  
```
docker ps -a | grep alpine_nautilus
```

- Commit the container to a new image

```
docker commit <container_name> <image_name:tag>

alpine_nautilus → container you are creating the image from

alpine:nautilus → new image name and tag

[banner@stapp03 tmp]$ docker commit alpine_nautilus alpine:nautilus
sha256:de57765cbc4c21a7e13b724a13a93aaf7b29090261de4fbe087c48b06a0be4c
```

- Verify the new image

```
docker images | grep alpine

[banner@stapp03 tmp]$ docker images | grep alpine
alpine           nautilus   de57765cbc4c   58 seconds ago   8.44MB
```

---
7. The Nautilus DevOps team is planning to do some cleanup on App Server 3 in Stratos Datacenter, some old and unused docker networks need to be deleted. Find below more details:


Delete a docker network named php-network from App Server 3 in Stratos Datacenter.

# Solution

- Check if the network exists
  
```
docker network ls | grep php-network

[banner@stapp03 tmp]$ docker network ls | grep php-network
54c1b41907bd   php-network   bridge    local
```

- Remove the network

  - If the network is in use by any container, Docker will return an error.
  - Make sure all containers using the network are stopped or disconnected first.
  - Docker networks must not be in use when deleting. If containers are attached, use docker network disconnect <network> <container> or stop/remove the containers first.

```
[banner@stapp03 tmp]$ docker network rm php-network
php-network

[banner@stapp03 tmp]$ docker network ls | grep php-network
```

---

8. The Nautilus DevOps team is planning to setup/create some docker containers on App Server 3 in Stratos Datacenter, some prerequisites are needs to be done on this server. Find below more details:


Create a new network named mysql-network using the bridge driver. Allocate subnet 182.18.0.0/24, configure Gateway 182.18.0.1.

# Solution

- Create the network with subnet and gateway
  
```
docker network create \
  --driver bridge \
  --subnet 182.18.0.0/24 \
  --gateway 182.18.0.1 \
  mysql-network

```

- verify

```

[banner@stapp03 tmp]$ docker network ls | grep mysql-network
d368aeeb44b4   mysql-network   bridge    local
```

- check the details. You should see:
```
"Subnet": "182.18.0.0/24",
"Gateway": "182.18.0.1"

```

```
[banner@stapp03 tmp]$ docker network inspect mysql-network
[
    {
        "Name": "mysql-network",
        "Id": "d368aeeb44b48500500fd436535aafc81918182141966844c8855ad4e338daa3",
        "Created": "2026-01-12T14:54:11.453971534Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "182.18.0.0/24",
                    "Gateway": "182.18.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]
```

---

9. There were some containers created by the DevOps team on App Server 3 in Stratos DC, and those were running fine till yesterday. Team found that those containers were exited somehow today, look into the issue and make sure all containers are in running state. Below is the name of two containers which were exited:


a. lab1_container
b. lab2_container

# Solution

- check the exited containers
  
```
[banner@stapp03 tmp]$ docker ps -a | grep lab
ae2c9bdd79bb   nginx:1-alpine3.18-slim   "/docker-entrypoint.…"   52 seconds ago   Exited (0) 50 seconds ago             lab2_container
3a76b816a92e   nginx:1-alpine3.18-slim   "/docker-entrypoint.…"   54 seconds ago   Exited (0) 50 seconds ago             lab1_container
```

- Start the exited containers

```
docker start lab1_container
docker start lab2_container

[banner@stapp03 tmp]$ docker ps | grep lab
ae2c9bdd79bb   nginx:1-alpine3.18-slim   "/docker-entrypoint.…"   4 minutes ago    Up 23 seconds   80/tcp    lab2_container
3a76b816a92e   nginx:1-alpine3.18-slim   "/docker-entrypoint.…"   4 minutes ago    Up 13 seconds   80/tcp    lab1_container
```

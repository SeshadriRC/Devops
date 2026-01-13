Nautilus project developers are planning to start testing on a new project. As per their meeting with the DevOps team, they want to test containerized environment application features. As per details shared with DevOps team, we need to accomplish the following task:


a. Pull busybox:musl image on App Server 1 in Stratos DC and re-tag (create new tag) this image as busybox:local.






# Solution

```
[tony@stapp01 ~]$ docker images
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
busybox      musl      ff7d91a4de4f   15 months ago   1.51MB
[tony@stapp01 ~]$ docker tag busybox:musl busybox:local
[tony@stapp01 ~]$ docker images
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
busybox      local     ff7d91a4de4f   15 months ago   1.51MB
busybox      musl      ff7d91a4de4f   15 months ago   1.51MB
```

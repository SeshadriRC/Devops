The DevOps team at xFusionCorp Industries is initiating the setup of CI/CD pipelines and has decided to utilize Jenkins as their server. Execute the task according to the provided requirements:



1. Install Jenkins on the jenkins server using the yum utility only, and start its service.

If you face a timeout issue while starting the Jenkins service, refer to this.
2. Jenkin's admin user name should be theadmin, password should be Adm!n321, full name should be Siva and email should be siva@jenkins.stratos.xfusioncorp.com.


Note:

1. To access the jenkins server, connect from the jump host using the root user with the password S3curePass.

2. After Jenkins server installation, click the Jenkins button on the top bar to access the Jenkins UI and follow on-screen instructions to create an admin user.

## Solution

https://www.jenkins.io/doc/book/installing/linux/ -> install jenkins on linux
https://www.jenkins.io/doc/book/installing/linux/#red-hat-centos -> install jenkins on centos

```
ssh root@jenkins

take the password from lab

ignore below steps , i did wrongly on jump host
journalctl -xeu jenkins.service

got below error, so changing port of jenkins will help
vi /etc/default/jenkins

however for this task im killing the process
netstat -tulnp| grep 8080
kill -9 pid

Nov 01 06:10:24 jumphost.stratos.xfusioncorp.com jenkins[12019]: Caused: java.io.IOException: Failed to bind to 0.0.0.0/0.0.0.0:8080
Nov 01 06:10:24 jumphost.stratos.xfusioncorp.com jenkins[12019]:         at Jenkins Main ClassLoader//org.eclipse.jetty.server.ServerConnector.openAcceptChannel(ServerConnector.java:349)

[root@jumphost ~]# netstat -tulnp | grep 8080
tcp        0      0 0.0.0.0:8080            0.0.0.0:*               LISTEN      614/ttyd

systemctl stop ttyd
ttyd -p 8090 bash

```

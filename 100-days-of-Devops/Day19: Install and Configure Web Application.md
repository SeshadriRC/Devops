xFusionCorp Industries is planning to host two static websites on their infra in Stratos Datacenter. The development of these websites is still in-progress, but we want to get the servers ready. Please perform the following steps to accomplish the task:



a. Install httpd package and dependencies on app server 1.


b. Apache should serve on port 8082.


c. There are two website's backups /home/thor/beta and /home/thor/apps on jump_host. Set them up on Apache in a way that beta should work on the link http://localhost:8082/beta/ and apps should work on link http://localhost:8082/apps/ on the mentioned app server.


d. Once configured you should be able to access the website using curl command on the respective app server, i.e curl http://localhost:8082/beta/ and curl http://localhost:8082/apps/

## Solution

```
yum install -y httpd
vi /etc/httpd/conf/httpd.conf
change Listen port to 8082 

scp -r beta tony@stapp01.stratos.xfusioncorp.com:/tmp
scp -r apps tony@stapp01.stratos.xfusioncorp.com:/tmp

curl http://localhost:8082
```

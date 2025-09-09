The system admins team of xFusionCorp Industries needs to deploy a new application on App Server 1 in Stratos Datacenter. They have some pre-requites to get ready that server for application deployment. Prepare the server as per requirements shared below:



1. Install and configure nginx on App Server 1.

2. On App Server 1 there is a self signed SSL certificate and key present at location /tmp/nautilus.crt and /tmp/nautilus.key. Move them to some appropriate location and deploy the same in Nginx.

3. Create an index.html file with content Welcome! under Nginx document root.

4. For final testing try to access the App Server 1 link (either hostname or IP) from jump host using curl command. For example curl -Ik https://<app-server-ip>/.

## Solution

https://www.nbtechsupport.co.in/2021/01/setup-ssl-for-nginx-kodekloud-engineer.html

```
1. Login to the respective app server 1 and install nginx
2. Edit Nginx conf & do the changes as below

vi /etc/nginx/nginx.conf
add server name, then location of /tmp/nautilus.crt and /nautilus.key and make sure you are uncommenting. watch the video

ssl_certificate "/etc/pki/CA/certs/nautilus.crt";
ssl_certificate_key "/etc/pki/CA/private/nautilus.key";

3. Once the configuration is saved then copy the certificates and key

cp /tmp/nautilus.crt /etc/pki/CA/certs/ and cp /tmp/nautilus.key /etc/pki/CA/private/

4. Create an index.html with the word Welcome! on Nginx document root

5. Start  & check the status of  the Nginx service

   systemctl start nginx
   systemctl status nginx

6. Validate the task  from JUMP server

   curl -Ik https://172.16.238.10

thor@jumphost ~$ curl -Ik https://stapp01/
HTTP/2 200 
server: nginx/1.20.1
date: Tue, 09 Sep 2025 14:18:14 GMT
content-type: text/html
content-length: 9
last-modified: Tue, 09 Sep 2025 14:02:58 GMT
etag: "68c03392-9"
accept-ranges: bytes




```

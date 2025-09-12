xFusionCorp Industries is planning to host a WordPress website on their infra in Stratos Datacenter. They have already done infrastructure configuration—for example, on the storage server they already have a shared directory /vaw/www/html that is mounted on each app host under /var/www/html directory. Please perform the following steps to accomplish the task:

a. Install httpd, php and its dependencies on all app hosts.


b. Apache should serve on port 8088 within the apps.


c. Install/Configure MariaDB server on DB Server.


d. Create a database named kodekloud_db9 and create a database user named kodekloud_gem identified as password ksH85UJjhb. Further make sure this newly created user is able to perform all operation on the database you created.


e. Finally you should be able to access the website on LBR link, by clicking on the App button on the top bar. You should see a message like App is able to connect to the database using user kodekloud_gem

## Solution

- https://www.nbtechsupport.co.in/2021/01/install-configure-web-application.html?m=1

```
503 Service Unavailable
No server is available to handle this request.

Try these steps:

App servers:
yum install -y net-tools mc
yum install -y httpd php.x86_64 php-mysqlnd.x86_64
systemctl restart httpd
systemctl enable --now httpd
systemctl status httpd

vi /etc/httpd/conf/httpd.conf
Listen 6100

DB Server:
DB installlation:
yum install mariadb-server -y
systemctl enable mariadb
systemctl status mariadb

mysql -u root -p

CREATE DATABASE kodekloud_db7;
CREATE USER ‘kodekloud_roy’@‘%’ IDENTIFIED BY ‘dCV3szSGNA’;
GRANT ALL PRIVILEGES ON kodekloud_db7.* TO ‘kodekloud_roy’@‘%’;
FLUSH PRIVILEGES;
EXIT;

systemctl restart mariadb

Verification:
from the LBR host you can curl to app server


```




The Nautilus application development team is planning to launch a new PHP-based application, which they want to deploy on Nautilus infra in Stratos DC. The development team had a meeting with the production support team and they have shared some requirements regarding the infrastructure. Below are the requirements they shared:



a. Install nginx on app server 3 , configure it to use port 8092 and its document root should be /var/www/html.


b. Install php-fpm version 8.2 on app server 3, it must use the unix socket /var/run/php-fpm/default.sock (create the parent directories if don't exist).


c. Configure php-fpm and nginx to work together.


d. Once configured correctly, you can test the website using curl http://stapp03:8092/index.php command from jump host.

NOTE: We have copied two files, index.php and info.php, under /var/www/html as part of the PHP-based application setup. Please do not modify these files.

## Solution

```
yum install -y nginx
systemctl enable nginx

edit config file of nginx
vi /etc/nginx/nginx.conf

systemctl restart nginx
sudo yum install php php-fpm php-cli -y

sudo yum remove -y php php-fpm

Configure PHP-FPM
Edited /etc/php-fpm.d/www.conf:
- listen = /var/run/php-fpm/default.sock
- listen.owner = apache
- listen.group = apache
- listen.mode = 0660


```

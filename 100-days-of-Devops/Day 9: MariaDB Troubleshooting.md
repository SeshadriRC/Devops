There is a critical issue going on with the Nautilus application in Stratos DC. The production support team identified that the application is unable to connect to the database. After digging into the issue, the team found that mariadb service is down on the database server.



Look into the issue and fix the same.

## Solution

https://blog.devops.dev/day-9-of-100-days-of-devops-troubleshooting-mariadb-in-production-b12958a61477

```
systemctl status mariadb
systemctl start mariadb

cat /var/log/mariadb/mariadb.log | tail -30

chown -R mysql:mysql /run/mariadb
chmod 755 /run/mariadb

systemctl start mariadb

```

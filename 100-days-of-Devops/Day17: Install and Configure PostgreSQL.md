The Nautilus application development team has shared that they are planning to deploy one newly developed application on Nautilus infra in Stratos DC. The application uses PostgreSQL database, so as a pre-requisite we need to set up PostgreSQL database server as per requirements shared below:



PostgreSQL database server is already installed on the Nautilus database server.


a. Create a database user kodekloud_gem and set its password to 8FmzjvFU6S.


b. Create a database kodekloud_db6 and grant full permissions to user kodekloud_gem on this database.


Note: Please do not try to restart PostgreSQL server service.

## Solution
- https://www.nbtechsupport.co.in/2021/01/install-and-configure-postgresql.html
```
1. At first login  to the database server & Switch to the root user
2. Check postgresql installed or not
   yum -y install postgresql-server postgresql-contrib
postgresql-setup initdb
sudo -u postgres psql

CREATE USER kodekloud_gem WITH PASSWORD '8FmzjvFU6S';
CREATE DATABASE kodekloud_db6;
GRANT ALL PRIVILEGES ON DATABASE "kodekloud_db6" to kodekloud_gem;

vi /var/lib/pgsql/data/postgresql.conf

Edit another config file pg_hba.conf  &  configure the md5 at bottom of the config 

Validate the task by below commands

psql -U kodekloud_gem -d kodekloud_db6 -h 127.0.0.1 -W
psql -U kodekloud_gem -d kodekloud_db6 -h localhost -W

```

```

The Nautilus application development team recently finished the beta version of one of their Java-based applications, which they are planning to deploy on one of the app servers in Stratos DC. After an internal team meeting, they have decided to use the tomcat application server. Based on the requirements mentioned below complete the task:

a. Install tomcat server on App Server 2.

b. Configure it to run on port 8084.

c. There is a ROOT.war file on Jump host at location /tmp.

Deploy it on this tomcat server and make sure the webpage works directly on base URL i.e curl http://stapp02:8084

## Solution

```
scp ROOT.war steve@stapp02.stratos.xfusioncorp.com:/tmp
yum install -y tomcat
rpm -qa | grep tom
cat /usr/share/tomcat/conf/server.xml |grep port    --> edit the Connector port of non-ssl
cp /tmp/ROOT.war /usr/share/tomcat/webapps

systemctl start tomcat
systemctl status tomcat

curl -ivk http://stapp02:8084
```

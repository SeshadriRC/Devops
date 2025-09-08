The production support team of xFusionCorp Industries has deployed some of the latest monitoring tools to keep an eye on every service, application, etc. running on the systems. One of the monitoring systems reported about Apache service unavailability on one of the app servers in Stratos DC.



Identify the faulty app host and fix the issue. Make sure Apache service is up and running on all app hosts. They might not have hosted any code yet on these servers, so you don’t need to worry if Apache isn’t serving any pages. Just make sure the service is up and running. Also, make sure Apache is running on port 8084 on all app servers.

## Solution

https://www.nbtechsupport.co.in/2021/01/linux-process-troubleshooting-kodekloud.html

```
1. At first identify the app server which has issue by running the below commands

telnet stapp01 8087

2. login on app server which has connection refuse error & switch to root

3. Run the Below command to check the existing Apache HTTPd service

systemctl start httpd
systemctl status httpd

4. With the help of  netstat check which service is using your port and whats is PID and kill it

netstat -tulnp
```

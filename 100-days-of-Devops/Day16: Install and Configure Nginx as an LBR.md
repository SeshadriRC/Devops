Day by day traffic is increasing on one of the websites managed by the Nautilus production support team. Therefore, the team has observed a degradation in website performance. Following discussions about this issue, the team has decided to deploy this application on a high availability stack i.e on Nautilus infra in Stratos DC. They started the migration last month and it is almost done, as only the LBR server configuration is pending. Configure LBR server as per the information given below:



a. Install nginx on LBR (load balancer) server.


b. Configure load-balancing with the an http context making use of all App Servers. Ensure that you update only the main Nginx configuration file located at /etc/nginx/nginx.conf.


c. Make sure you do not update the apache port that is already defined in the apache configuration on all app servers, also make sure apache service is up and running on all app servers.


d. Once done, you can access the website using StaticApp button on the top bar.

## Solution

https://tundedamian.medium.com/day-16-of-100-days-of-devops-nginx-load-balancer-setup-in-a-high-availability-environment-b2080969d6be

```
1. SSH into the LBR Host, Installed and Started Nginx

yum install -y nginx
systemctl start nginx
systemctl enable nginx

ss -tulnp|grep httpd

upstream app_servers {
server stapp01.stratos.xfusioncorp.com:8086;
server stapp02.stratos.xfusioncorp.com:8086;
server stapp03.stratos.xfusioncorp.com:8086;
}

location / {
proxy_pass http://app_servers;
}

nginx -t

```

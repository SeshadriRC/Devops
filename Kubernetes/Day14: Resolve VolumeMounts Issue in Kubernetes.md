We encountered an issue with our Nginx and PHP-FPM setup on the Kubernetes cluster this morning, which halted its functionality. Investigate and rectify the issue:



The pod name is nginx-phpfpm and configmap name is nginx-config. Identify and fix the problem.


Once resolved, copy /home/thor/index.php file from the jump host to the nginx-container within the nginx document root. After this, you should be able to access the website using Website button on the top bar.


Note: The kubectl utility on jump_host is configured to operate with the Kubernetes cluster.

## Solution

```
1. check the shared volume path in existing config map 
kubectl describe configmap nginx-config

you can see below:
root /var/www/html

(or)

k get cm nginx-config -o yaml
 # Set nginx to serve files from the shared volume!
        root /var/www/html;
        index  index.html index.htm index.php;
        server_name _;
        location / {
          try_files $uri $uri/ =404;

2. Get the configuration in the YAML file from the running pod
kubectl get pod nginx-phpfpm -o yaml  > /tmp/nginx.yaml

3. Edit the nginx.yaml file and changed ‘/usr/share/nginx/html’ to ‘/var/www/html’ in 2 places.

4. Replace the pod
k replace -f /tmp/nginx.yaml --force

5. copy the index.php file as per the task
kubectl cp  /home/thor/index.php  nginx-phpfpm:/var/www/html -c nginx-container

6. Validate by login to the link which given or login to container and run below curl
curl http://localhost:8089


```


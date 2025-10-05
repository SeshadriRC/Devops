As per recent requirements shared by the Nautilus application development team, they need custom images created for one of their projects. Several of the initial testing requirements are already been shared with DevOps team. Therefore, create a docker file /opt/docker/Dockerfile (please keep D capital of Dockerfile) on App server 2 in Stratos DC and configure to build an image with the following requirements:



a. Use ubuntu:24.04 as the base image.


b. Install apache2 and configure it to work on 3002 port. (do not update any other Apache configuration settings like document root etc).

## Solution

```
Dockerfile

# Use Ubuntu 24.04 as base image
FROM ubuntu:24.04

# Install apache2
RUN apt-get update && apt-get install -y apache2 && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Change Apache to listen on port 3002
RUN sed -i 's/80/3002/g' /etc/apache2/ports.conf && \
    sed -i 's/*:80/*:3002/g' /etc/apache2/sites-available/000-default.conf

# Expose port 3002
EXPOSE 3002

# Start apache2 in foreground
CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]



docker build -t naut_custom:latest .

docker run -d -p 5000:3002 naut_custom:latest

docker inspect fe0 |grep -A 3 Port

```

```
# Use Ubuntu 24.04 as base image
FROM ubuntu:24.04

# Install apache2
RUN apt-get update && apt-get install -y apache2

# Expose the port
EXPOSE 3002

# Setup an app user so the container doesn't run as the root user
RUN useradd app
USER app

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8080"]
```

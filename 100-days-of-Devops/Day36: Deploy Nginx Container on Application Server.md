The Nautilus DevOps team is conducting application deployment tests on selected application servers. They require a nginx container deployment on Application Server 2. Complete the task with the following instructions:


On Application Server 2 create a container named nginx_2 using the nginx image with the alpine tag. Ensure container is in a running state.

## Solution

```
docker run --name nginx_2 -d nginx:alpine
```

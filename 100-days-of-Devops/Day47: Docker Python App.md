A python app needed to be Dockerized, and then it needs to be deployed on App Server 2. We have already copied a requirements.txt file (having the app dependencies) under /python_app/src/ directory on App Server 2. Further complete this task as per details mentioned below:



Create a Dockerfile under /python_app directory:

Use any python image as the base image.
Install the dependencies using requirements.txt file.
Expose the port 8084.
Run the server.py script using CMD.

Build an image named nautilus/python-app using this Dockerfile.


Once image is built, create a container named pythonapp_nautilus:

Map port 8084 of the container to the host port 8095.

Once deployed, you can test the app using curl command on App Server 2.


curl http://localhost:8095/


## Solution

```
Dockerfile

# Use python as base image
FROM python:alpine3.21

# Set working directory inside the container
WORKDIR /app

# Copy requirements.txt from the given path
COPY src/requirements.txt .

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy rest of the files
COPY src/ .

# Expose the port
EXPOSE 8084

# Run the script using CMD
CMD ["python","server.py"]

docker build -t nautilus/python-app .
docker images
ps  -ef|grep 8095
docker run -d --name pythonapp_nautilus -p 8095:8084 nautilus/python-app

curl http://localhost:8095/
```

docker run -d -p 5000:3002 naut_custom:latest

docker inspect fe0 |grep -A 3 Port

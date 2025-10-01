The Nautilus DevOps team possesses confidential data on App Server 1 in the Stratos Datacenter. A container named ubuntu_latest is running on the same server.



Copy an encrypted file /tmp/nautilus.txt.gpg from the docker host to the ubuntu_latest container located at /opt/. Ensure the file is not modified during this operation.

## Solution

```
docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/opt/
```

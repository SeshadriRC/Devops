The Nautilus DevOps team is crafting jobs in the Kubernetes cluster. While they're developing actual scripts/commands, they're currently setting up templates and testing jobs with dummy commands. Please create a job template as per details given below:


Create a job named countdown-nautilus.

The spec template should be named countdown-nautilus (under metadata), and the container should be named container-countdown-nautilus

Utilize image ubuntu with latest tag (ensure to specify as ubuntu:latest), and set the restart policy to Never.

Execute the command sleep 5

Note: The kubectl utility on jump_host is set up to operate with the Kubernetes cluster.

## Solution
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: countdown-nautilus
spec:
  template:
    metadata:
      name: countdown-nautilus
    spec:
      containers:
        - name: container-countdown-nautilus
          image: ubuntu:latest
          command: ["sleep", "5"]
      restartPolicy: Never

```

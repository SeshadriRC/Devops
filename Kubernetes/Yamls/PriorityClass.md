```yaml
apiVersion: scheduling.k8s.io/v1
kind: PriorityClass
metadata:
  name: high-priority
value: 100000
globalDefault: false
description: "This priority class is used for high-priority pods."
preemptionPolicy: PreemptLowerPriority
```

**Example Pod**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: busybox-priority
spec:
  priorityClassName: high-priority
  containers:
  - name: busybox
    image: busybox
    command: ['sleep', '3600']
```

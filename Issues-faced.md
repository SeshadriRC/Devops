

[Openshift issue](#Openshift-issue)
- [Node](#Node)

[Database issue](#Database-issue)

[RRT](#RRT)

## Openshift issue

**Connection timeouts**

we will ask app team to increase a ha proxy annotation timeout

**Cluster Upgrade**
- Nodes are in scheduling disabled state, so upgrade not progressing. to address this issue we have set the node to scheduling enabled, then upgrade got processed. then again we set the node to scheduling disabled state

**Image Pull back off**

We will ask the app team to use correct image

**Probe Failures**

- we will ask the application team to configure probes properly
- liveness and readiness -> set initial delay seconds to 10

**Pod crashloopback off**

- We observed a CrashLoopBackOff in the pod due to an OOMKilled event. We informed the application team and instructed them to allocate sufficient memory.

**Quota issue**

The application team was unable to deploy the application as it required a large resource quota, while the namespace was allocated with a medium quota. To address this, we increased the quota from medium to large. After the update, the issue was resolved.

### Node

**Insufficient Node CPU**

- We will increase the machineset count to fix this issue

**Node stuck after draining**

- To fix this issue we need to delete the pods which are not properly deleted

### Pod

**Pod restart reasons**

1. LivenessProbe failure
2. StartupProbe failure
3. Resourcelimits exceeded(Memory/CPU)
4. Deployment or Statefulset updates
5. Node failure
6. Credential Refresh failure
7. Kubernetes or Node maintanence

- We noticed a CPU utilization by the Pod which created by cronjob, so to address this issue we have suspended the cronjob post that issue got fixed
- One pod alone throwing timeout 500 error, please refer RRT 1
- Socket timeout exception in pod log, so we asked user to increase the memory limits in deployment and save the deployment. post that this issue got fixed, but in pod events    we don't see any error

## Database issue

- DB will not get rebooted if we modify the storage
- 
**HugePages**

The database was initially running on a db.t4g.large instance type. Due to low activity, Turbonomic automatically scaled it down to a db.t4g.medium instance. Following the scale-down, the database became inaccessible due to an incompatible parameter state—hugepages was set to ON, which is not supported on the medium instance type. To resolve the issue, we set the hugepages parameter to OFF followed by a database reboot. After the reboot, the database became accessible.

https://aws.amazon.com/rds/instance-types/

https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/PostgreSQL.Concepts.General.FeatureSupport.HugePages.html

**Not able to take DB snapshot**

We were unable to take a snapshot of the database as the snapshot limit (100) had already been reached. There were 100 snapshots across various databases. To resolve this, we increased the quota in Service

**Storage issue**

The database became inaccessible due to full storage. To resolve the issue, we increased the allocated storage, after which the database became accessible.

**Subscription issue after upgrade of database**

To fix this issue we have suggested user to re-subscribe the replication

**Turbonomics activity**

user initially allocated a **db.t4g.2xlarge** instance type to the database, anticipating high resource consumption. However, as the resource usage remained low over time, Turbonomic automatically downgraded the instance type to **db.t4g.large**.

We will be checking the yaml of postgres CR


## RRT

**RRT 1**

Cause

One of the pods "creditlinedecisionapi-prod2-250904-1336-c5f4f49cb-mfh7r" out of 4 pods for creditlinedecision in aws-useast1-apps-prod-2 is throwing 500 server error when trying to access the application AccountNumberLookUp through a GTM route which points to the application in aws-useast1-apps-prod-1 & aws-useast1-apps-prod-2.

This issue was only observed in the above specific pod.

Resolution/Update

We troubleshooted and we were able to find that the Auth token that the pod was using to access the destination was invalid. User said that she will loook from the application end and to resolve this issue immediately asked us to restart the source pod but we explained that it has to be done from their end as per protocol. They tried to restart that specific pod but they faced an issue with their call alter. Their entire team were facing an issue with call alter. Since the business hour were nearing and it was an emergency we restarted it for them with their permission.
---

**RRT 2**

Cause 
Job is not running due to socket exception timeout

Resolution
We checked the deployment yaml, memory limit is set to low, so we asked user to increase it. then he submitted job again. post that issue got fixed

Cause

Pods were throwing out connectivity exception error.

Resolution/Update

OCP-OPS were paged and we did the basic troubleshooting steps. After tunneling down step by step the application team found out that the source application in the on-prem had less timeout set which was 2 minutes. The application team update that to 4 minutes and redeployed it and tested it once again and they didn't face any issue.

Destination:

cluster: aws-aps1-apps-prod-1

namespace: epp-dci-offshore-storage

deployment: dci-cloud-save-prod

database: pgsql-ec764c3b-0344-4d0e-9b32-bd66df58b9c5

---



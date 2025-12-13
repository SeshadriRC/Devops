etcdctl --endpoints=https://[127.0.0.1]:2379 \
--cacert=/etc/kubernetes/pki/etcd/ca.crt \
--cert=/etc/kubernetes/pki/etcd/server.crt \
--key=/etc/kubernetes/pki/etcd/server.key \
snapshot save /opt/snapshot-pre-boot.db


etcdutl snapshot restore /opt/snapshot-pre-boot.db --data-dir /var/lib/etcd-from-backup


First, using the etcdutl command, restore the snapshot:

etcdutl snapshot restore /opt/snapshot-pre-boot.db --data-dir /var/lib/etcd-from-backup
Example output:

etcdutl snapshot restore /opt/snapshot-pre-boot.db --data-dir /var/lib/etcd-from-backup
2025-04-24T09:38:07Z    info    snapshot/v3_snapshot.go:265     restoring snapshot      {"path": "/opt/snapshot-pre-boot.db", "wal-dir": "/var/lib/etcd-from-backup/member/wal", "data-dir": "/var/lib/etcd-from-backup", "snap-dir": "/var/lib/etcd-from-backup/member/snap", "initial-memory-map-size": 10737418240}
2025-04-24T09:38:07Z    info    membership/store.go:141 Trimming membership information from the backend...
2025-04-24T09:38:07Z    info    membership/cluster.go:421       added member    {"cluster-id": "cdf818194e3a8c32", "local-member-id": "0", "added-peer-id": "8e9e05c52164694d", "added-peer-peer-urls": ["http://localhost:2380"]}
2025-04-24T09:38:07Z    info    snapshot/v3_snapshot.go:293     restored snapshot       {"path": "/opt/snapshot-pre-boot.db", "wal-dir": "/var/lib/etcd-from-backup/member/wal", "data-dir": "/var/lib/etcd-from-backup", "snap-dir": "/var/lib/etcd-from-backup/member/snap", "initial-memory-map-size": 10737418240}
Note: In this case, we are restoring the snapshot to a different directory which is in the same server where we took the backup (the controlplane node). As a result, the only required option for the restore command is the --data-dir.

Next, we need to update the /etc/kubernetes/manifests/etcd.yaml to point to the newly restored directory, which is /var/lib/etcd-from-backup. The only change that we need to make to the YAML file, is to change the hostPath for the volume called etcd-data from old directory /var/lib/etcd to the new directory /var/lib/etcd-from-backup:

  ...
  volumes:
  - hostPath:
      path: /var/lib/etcd-from-backup # Newly restored backup directory
      type: DirectoryOrCreate
    name: etcd-data
With this change, /var/lib/etcd on the container points to /var/lib/etcd-from-backup on the controlplane.

When this file is updated, the ETCD pod is automatically re-created as this is a static pod placed under the /etc/kubernetes/manifests directory. This may take a few minutes, and it is expected that kube-controller-manager and kube-scheduler will also restart. To check the containers being restarted:

watch crictl ps
Once the updated etcd container and the kube-apiserver containers are up, you can verify that the missing deployments (2 deployments) and services (3 services) are restored again:

kubectl get deployments,services

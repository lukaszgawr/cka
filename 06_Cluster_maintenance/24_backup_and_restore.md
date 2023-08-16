# Backup & restore

Backup candidates:
1. Resource configuration - store manifests of resources in git, save also all resources with:   
```kubectl get all --all-namespaces -o yaml > all-deploy-services-backup.yaml```
2. ETCD Cluster 
3. Persistent Volumes

3rd party tools: Velero (formerly called ARK by HeptIO)

## ETCD backup

### Option 1 - Backup data-dir
E.g. --data-dir=/var/lib/etcd - just save the content

### Option 2 - Make snapshot
```ETCDCTL_API=3 etcdctl snapshot save snapshot.db```   

to check the status:
``` ETCDCTL_API=3 etcdctl snapshot status snapshot.db```

#### Restore from snapshot

1. Stop kube-apiserver:
```service kube-apiserver stop``` - not necesarily
2. Restore snapshot:
```ETCDCTL_API=3 etcdctl snapshot restore snapshot.db --data-dir /var/lib/etcd-from-backup ```
3. Change --data-dir in etcd service to --data-dir=/var/lib/etcd-from-backup OR change the volume's hostPath. If etcd is deployed as service then change data-dir in e.g. /etc/systemd/system/etcd.service
4. ```systemctl daemon-reload```
5. ```service etcd restart```
6. ```service kube-apiserver start```

It is recommended to restart controlplane components (e.g. kube-scheduler, kube-controller-manager, kubelet) to ensure that they don't rely on some stale data

> You might also need to pass to etcdctl the following arguments: 
> * --endpoints - to point to etcd server, default is 127.0.0.1:2379 so _--endpoints=https://[127.0.0.1]:2379_
> * --cacert
> * --cert
> * --key

Example:
```
 ETCDCTL_API=3 etcdctl --endpoints=https://[127.0.0.1]:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key \
  snapshot save /opt/snapshot-pre-boot.db
```

## ETCD external vs stacked

Normally when you see etcd deployed as pod in kube-system namespace it indicates it's stacked version deployed in the cluster.

But sometimes ETCD can be external. In this case you won't see the etcd pods, but when you ssh to the master node and do:
```
ps -ef|grep etcd
```
You will see reference to external etcd in kube-apiserver process.

The same can be seen if you describe kube-apiserver:
```
cluster2-controlplane ~ ➜  kubectl -n kube-system describe pod kube-apiserver-cluster2-controlplane 
Name:                 kube-apiserver-cluster2-controlplane
Namespace:            kube-system
Priority:             2000001000
Priority Class Name:  system-node-critical
Node:                 cluster2-controlplane/10.1.127.3
Start Time:           Wed, 31 Aug 2022 05:03:45 +0000
Labels:               component=kube-apiserver
                      tier=control-plane
Annotations:          kubeadm.kubernetes.io/kube-apiserver.advertise-address.endpoint: 10.1.127.3:6443
                      kubernetes.io/config.hash: 9bd4c04b38b27661e9e7f8b0fc1237b8
                      kubernetes.io/config.mirror: 9bd4c04b38b27661e9e7f8b0fc1237b8
                      kubernetes.io/config.seen: 2022-08-31T05:03:28.843162256Z
                      kubernetes.io/config.source: file
                      seccomp.security.alpha.kubernetes.io/pod: runtime/default
Status:               Running
IP:                   10.1.127.3
IPs:
  IP:           10.1.127.3
Controlled By:  Node/cluster2-controlplane
Containers:
  kube-apiserver:
    Container ID:  containerd://cc64f3649222f24d3fd2eb7d5f0f17db5fca76eb72dc4c17295fb4842c045f1b
    Image:         k8s.gcr.io/kube-apiserver:v1.24.0
    Image ID:      k8s.gcr.io/kube-apiserver@sha256:a04522b882e919de6141b47d72393fb01226c78e7388400f966198222558c955
    Port:          <none>
    Host Port:     <none>
    Command:
      kube-apiserver
      --advertise-address=10.1.127.3
      --allow-privileged=true
      --authorization-mode=Node,RBAC
      --client-ca-file=/etc/kubernetes/pki/ca.crt
      --enable-admission-plugins=NodeRestriction
      --enable-bootstrap-token-auth=true
      --etcd-cafile=/etc/kubernetes/pki/etcd/ca.pem
      --etcd-certfile=/etc/kubernetes/pki/etcd/etcd.pem
      --etcd-keyfile=/etc/kubernetes/pki/etcd/etcd-key.pem
      --etcd-servers=https://10.1.127.10:2379
--------- End of Snippet---------
```
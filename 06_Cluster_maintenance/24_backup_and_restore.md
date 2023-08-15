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
```service kube-apiserver stop```
2. Restore snapshot:
```ETCDCTL_API=3 etcdctl snapshot restore snapshot.db --data-dir /var/lib/etcd-from-backup ```
3. Change --data-dir in etcd service to --data-dir=/var/lib/etcd-from-backup
4. ```systemctl daemon-reload```
5. ```service etcd restart```
6. ```service kube-apiserver start```

> You might also need to pass to etcdctl the following arguments: 
> * --endpoints - to point to etcd server, default is 127.0.0.1:2379
> * --cacert
> * --cert
> * --key
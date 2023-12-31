# Cluster upgrade process

Only one major version at a time. First masters, then workers.

K8s can be installed "The hard way" - using systemd services (e.g. /etc/systemd/system/kube-apiserver.service) or kubeadm way, where kube-apiserver is deployed as static pod in /etc/kubernetes/manifests/kube-apiserver.yaml. Same for other controlplane services.

## Upgrading master nodes
0. Drain if it has kubelet
1. ```apt-get upgrade -y kubeadm=1.12.0-00```
2. ```kubeadm upgrade plan```
3. ```kubeadm upgrade apply v1.12.0```
4. Upgrade kubelet (if kubelet is present on master node): ```apt-get upgrade -y kubelet=1.12.0-00```
5. ```systemctl restart kubelet```  
6. Uncordon master node, and if needed remove taint


## Upgrading worker nodes
Upgrade one at the time.
1. Drain the node: ```kubectl drain node-1 --ignore-daemonsets```, ssh to node
2. ```apt-get upgrade -y kubeadm=1.12.0-00```
3. ```kubeadm upgrade node ```
4. ```apt-get upgrade -y kubelet=1.12.0-00```
5. ```systemctl restart kubelet```
6. ssh to controlplane node, and do: ```kubectl uncordon node-1```

## Ubuntu tricks
### Madison to check available versions of a package
```
apt-cache madison kubeadm
```
### Holding/unholding packages
If on ubuntu inspect if apt holds the kube* package:
```
apt-mark showhold
```
To unhold:  
```
apt-mark unhold <package>
```  
After upgrading hold again:  
```
apt-mark hold <package>
```

## Inspecting service logs
If k8s is set up "the hard way" (with systemd services) we can view the logs with:
``` journalctl -u etcd.service -l ```  

If k8s was setup with kubeadm view the logs with ```kubectl logs etcd-master -n kube-system```  

If kube-apiserver/etcd server is down you need to go one level down - ssh to node and do ``` docker ps -a ``` and then ``` docker logs <container_id> ```

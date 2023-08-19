# Services

NodePort can only allocate ports higher than 30000.

![services](../images/47_services.png)
![services2](../images/47_services2.png)
Default mode of kube-proxy is iptables.

IP range of services is defined in kube-api-server in _--service-cluster-ip-range_

## Iptables in kube-proxy
ClusterIP:
![services3](../images/47_iptables.png)
NodePort:
![services4](../images/47_iptables_nodeport.png)

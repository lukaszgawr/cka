# OS Upgrades

If there was only 1 replica of the pod and node (which hosts that pod) goes down, and downtime is more than 5 minutes (default for _pod-eviction-timeout_ on kube-controller-manager) pod is considered as dead and k8s tries to recreate it on different node (if part of replica set). If pod was not part of any replica set it would just die.

## Drain & cordon
Pods are gracefully terminated from the node and recreated on another node.
```
kubectl drain node-1
```
When drained, node is also cordoned (made unschedulable).

After node comes back you need to uncordon it:
```
kubectl uncordon node-1
```
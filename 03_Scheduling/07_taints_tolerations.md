# Taints & tolerations
Placing PODs with tolerations on the tainted nodes.

Tainting a node:
```console
kubectl taint nodes node-name key=value:target-effect
```
To untaint:
```console
kubectl taint nodes node-name key=value:target-effect-
```

_target-effect_ - what happens to PODs that DO NOT TOLERATE this taint: 
* **NoSchedule** - PODs will not be scheduled on the node
* **PreferNoSchedule** - k8s will try not to schedule on the node (it's not guaranteed)
* **NoExecute** - PODs will not be scheduled + evicts pods that are already on the node

> By default Master nodes are tainted with NoSchedule!
```console
kubectl describe node kube-master|grep Taint
```

## Tolerations
```yaml
apiVersion: v1
kind: Pod
metadata:
  app: myapp
spec:
  containers:
  - name: nginx
    image: nginx
  tolerations:
  - key: "app"
    operator: "Equal"
    value: "blue"
    effect: "NoSchedule"
```

> Double quotes in _tolerations_ section are mandatory!

Taints & tolerations does NOT guarantee that the pods will ONLY prefer these tainted nodes! So sometimes combindation of taint/tolerations & Node Affinity is required.

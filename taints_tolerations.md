# Taints & tolerations

Tainting a node:
```console
kubectl taint nodes node-name key=value:target-effect
```

_target-effect_ options:
* NoSchedule
* PreferNoSchedule
* NoExecute - evicts pods that are already on the node

> By default Master nodes are tainted with NoSchedule!

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
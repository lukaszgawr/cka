# Node Selectors

Node selector = just labels placed on nodes.  

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-name
  labels:
    app: myapp
spec:
  containers:
  - name: nginx-server
    image: nginx
  nodeSelector:
    size: Large
```
To label the node:
```console
kubectl label nodes <node_name> size=Large
```

Node selectors are limited - you cannot use OR or NOT like this:
- Large OR Medium
- NOT Small  

For this you must use node affinity.
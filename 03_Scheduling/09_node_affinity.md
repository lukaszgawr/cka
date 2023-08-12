# Node Affinity

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
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: size
            operator: In
            values:
            - Large
            - Medium
```

Operator _In_ ensures that POD will be placed on any node whose size label value belongs to ANY of the values specified in the list of values.

Other operator: 
* **NotIn**
* **Exists** - you don't have to specify value - it just checks for the key

Node affinity types:
* **requiredDuringSchedulingIgnoredDuringExecution**
* **preferredDuringSchedulingIgnoredDuringExecution** - "try your best to find the node with the selector"
* **requiredDuringSchedulingRequiredDuringExecution** - planned

DuringExecution - means that pod was already running, but the node label changed. If ignored, it means that it will not affect already scheduled pods even if the label changes. If requried - the pod will be evicted.
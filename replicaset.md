# ReplicaSet

ReplicationController + selectors

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-rs
  labels:
    app: myapp
spec:
  template:
    metadata:
      name: pod-name
      labels:
        app: myapp
    spec:
      containers:
      - name: nginx-server
        image: nginx
  replicas: 3
  selector:
    matchLabels:
      app: myapp
```
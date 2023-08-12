# ReplicationController


```yaml
apiVersion: apps/v1
kind: ReplicationController
metadata:
  name: myapp-rc
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
  ```
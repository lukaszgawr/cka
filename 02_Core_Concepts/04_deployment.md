# Deployment
Tak jak ReplicaSet tylko kind Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
  labels:
    app: myapp
    type: front-end
spec:
  template:
    metadata:
      name: pod-name
      labels:
        app: myapp
        type: frontend
    spec:
      containers:
      - name: nginx-server
        image: nginx
  replicas: 3
  selector:
    matchLabels:
      type: front-end
```

```console
kubectl create deployment --image=nginx [dry-run=client] [--replicas=6] myapp-deployment [-o yaml]
```

To set image imperatively:
```
kubectl set image deployment/myapp container-name=container-image:version
```
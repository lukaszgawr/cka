# Logging & Monitoring
To view the logs from specific container:
```
kubectl logs [-f] pod-name container_name
```
For all containers:
```
kubectl logs pod --all-containers
```

## Viewing resource consumption from metrics-server
```
kubectl top nodes
kubectl top pods
```
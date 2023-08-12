# Exam Shortcuts

```
k run pod --image=nginx --dry-run=client -o yaml > pod_def.yaml
k get pods --watch
k create deploy nginx --replicas=3 --image=nginx --dry-run=client -o yaml
kubectl get ds -A
```

## Create daemonset
1. Make a template out of deployment:
```
k create deployment nginx --image=nginx --dry-run=client -o yaml > nginx.yaml
```
2. Change kind to DaemonSet in yaml
3. Remove replicas, strategy and status from yaml
4. Apply
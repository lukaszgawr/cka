# Namespaces

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev
```

```console
kubectl config set-context $(kubectl config current-context) --namespace=dev
```

## ResourceQuotas

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-quota
  namespace: dev
spec:
  hard:
    pods: "10"
    requests.cpu: "4"
    requests.memory: 5Gi
    limits.cpu: "10"
    limits.memory: 10Gi
```

* _limits.cpu_ -	Across all pods in a non-terminal state, the sum of CPU limits cannot exceed this value.
* _limits.memory_ -	Across all pods in a non-terminal state, the sum of memory limits cannot exceed this value.
* _requests.cpu_ -	Across all pods in a non-terminal state, the sum of CPU requests cannot exceed this value.
* _requests.memory_ -	Across all pods in a non-terminal state, the sum of memory requests cannot exceed this value.

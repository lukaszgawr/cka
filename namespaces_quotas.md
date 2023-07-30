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
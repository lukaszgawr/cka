# RBAC

Roles and RoleBindings are namespaced.
ClusterRole and ClusterRoleBindings are cluster-wide.

## Role

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: developer
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["list", "get", "create", "update", "delete"]
  resourceNames: ["concrete-pod"]
- apiGroups: [""]
  resources: ["ConfigMap"]
  verbs: ["create"]
```
_resourceNames_ is optional.

## RoleBinding

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: devuser-developer-binding
subjects:
- kind: User
  name: dev-user
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: developer
  apiGroup: rbac.authorization.k8s.io
```

# Check Access
```
kubectl auth can-i create deployments [--as dev-user]
kubectl auth can-i delete nodes [--as user]
```
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

## Imperatively
```
kubectl create role foo --verb=get,list,watch --resource=pods
kubectl create clusterrole foo --verb=get,list,watch --resource=replicasets.apps
```

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

Within the namespace "acme", grant the permissions in the "admin" ClusterRole to a user named "bob":
```
kubectl create rolebinding bob-admin-binding --clusterrole=admin --user=bob --namespace=acme
```

Across the entire cluster, grant the permissions in the "cluster-admin" ClusterRole to a user named "root":
```
kubectl create clusterrolebinding root-cluster-admin-binding --clusterrole=cluster-admin --user=root
```

# Check Access
```
kubectl auth can-i create deployments [--as dev-user]
kubectl auth can-i delete nodes [--as user]
```

# PODs

## Manifest

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
  nodeName: worker-node02
```
## Imperatively
```console
$ kubectl run nginx --image=nginx [--dry-run=client] [-o yaml] [-l key=value] [--port=8080] [--expose]
```

## POD Binding

```yaml
apiVersion: v1
kind: Binding
metadata:
  name: nginx
target:
  apiVersion: v1
  kind: Node
  name: worker-node02
```
```console
curl --header "Content-Type: application/json" --request POST --data '{"apiVersion": "v1", "kind": "Binding", "metadata": { "name": "nginx" }, "target": { "apiVersion": "v1", "kind": "Node", "name": "worker01" }}' http://localhost:8080/api/v1/namespaces/default/pods/nginx/binding
```

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
## Editing a POD
Remember, you CANNOT edit specifications of an existing POD other than the below:

* spec.containers[*].image

* spec.initContainers[*].image

* spec.activeDeadlineSeconds

* spec.tolerations

For example you cannot edit the environment variables, service accounts, resource limits (all of which we will discuss later) of a running pod.
To edit other things you need to edit deployment instead of specific POD.

# Running commands
```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: static-busybox
  name: static-busybox
spec:
  containers:
  - image: busybox
    name: static-busybox
    command: ["/bin/sh"]
    args: ["-c", "while true; do echo hello; sleep 10;done"]
    resources: {}
```
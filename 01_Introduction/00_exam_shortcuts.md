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

## Changing env variable in pod
1. Edit the pod
```
kubectl edit pod mypod
```
2. Change the env vaiable and save
3. It will fail, exit
4. Notice the file that was created e.g.: /tmp/kubectl-edit-1231.yaml
5. Make a reploace:
```
kubectl replace --force -f /tmp/kubectl-edit-1231.yaml
```

## Running Busybox (no curl there)

```kubectl run busybox --image=busybox -- sleep 7200```

Then you can apply and do e.g.: ```kubectl exec busybox -- ip route``` or get shell: ``` k exec -it busybox -- /bin/sh```

## Running alpine (and installing curl)

```
kubectl run alpine --image alpine -- sleep 7200
kubectl exec alpine -- apk add curl
```

## Set namespace
```
kubectl config set-context --current --namespace=dev
```

## Setting selector imperatively on a service
```
kubectl set selector svc svc-name 'key=value'
```

## Setting image imperatively
```
kubectl set image deploy deploy-name image-name
```

## Annotating imperatively
```
kubectl anotate deploy web-app kubernetes.io/change-cause="Changed to nginx:1.23.1" [--overwrite=true]
```
overwrite means to overwrite if annotation already exists

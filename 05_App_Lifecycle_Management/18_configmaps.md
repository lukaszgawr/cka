# Configmaps

## Imperatively
Imperatively from literal:
```
kubectl create configmap myconfigmap --from-literal=key=value [--from-literal=key2=value2]
```

Imperatively from file:
```
kubectl create configmap myconfigmap --from-file=file.properties
```

## Declaratively
AKMD

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: myconfigmap
data:
  APP_COLOR: blue
  APP_MODE: prod
```

## Injecting

To inject whole configmap in the pod:
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
    envFrom:
      - configMapRef:
          name: myconfigmap
```

You can also inject single vars from configmap:
```yaml
env:
  - name: APP_COLOR
    valueFrom:
      configMapKeyRef: 
        name: myconfigmap
        key: APP_COLOR
```

Injecting as volume:
```yaml
volumes:
- name: app-config-volume
  configMap:
    name: app-config
```
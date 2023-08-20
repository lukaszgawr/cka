# Services

```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
  labels:
    app: myapp
spec:
  type: NodePort
  ports:
    - targetPort: 80
      port: 80
      nodePort: 30008
  selector:
    app: myapp
    type: frontend
```

Porty:
* _port_ - port w serwisie  
* _targetPort_ - port w podzie  
* _nodePort_ - port na worker nodzie  

dla type:ClusterIP nie ma tylko nodePort a reszta tak samo

## Exposing resources
```console
kubectl expose rc/pod/rs/deploy name --port 80 --target-port=8080 [--type=ClusterIP] [--name=name-svc]
```

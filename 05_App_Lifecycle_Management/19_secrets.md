# Secrets

## Imperatively
From literal:
```
kubectl create secret generic mysecret --from-literal=key=value [--from-literal=key2=value2]
```
From file:
```
kubectl create secret generic mysecret --from-file=app_secret.properties
```

## Declaratievely
AKMD
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
data:
  DB_User: bX1zcWw=
  DB_Pass: cm9vdA==
```

## Injecting

To inject secrets as Env variables:
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
      - secretRef:
          name: myconfigmap
```

You can also inject single vars from secret:
```yaml
env:
  - name: DB_User
    valueFrom:
      secretKeyRef: 
        name: mysecret
        key: DB_User
```

Injecting as volume:
```yaml
volumes:
- name: app-secret-volume
  secret:
    secretName: mysecret
```
Each secret will be stored as file in /opt/app-secre-volume/

## Best practices
* Secrets are not encrypted at rest!!!  Only encoded. To configure enc at rest: [Link](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/)  
* Anyone able to create pods/deployments in the same namespace can access the secrets. Remedy - configure least-privilege access to Secrets - RBAC
* Consider 3rd party secrets store providers - GCP SM, Vault, etc.

## How kubernetes handles secrets internally

A secret is only sent to a node if a pod on that node requires it.

Kubelet stores the secret into a tmpfs so that the secret is not written to disk storage.

Once the Pod that depends on the secret is deleted, kubelet will delete its local copy of the secret data as well.

Read about the [protections](https://kubernetes.io/docs/concepts/configuration/secret/#protections) and [risks](https://kubernetes.io/docs/concepts/configuration/secret/#risks) of using secrets [here](https://kubernetes.io/docs/concepts/configuration/secret/#risks)


Having said that, there are other better ways of handling sensitive data like passwords in Kubernetes, such as using tools like Helm Secrets, HashiCorp Vault. 
# Environment variables

Variables can be taken from:
1. Plain key value
```yaml
env:
  - name: APP_COLOR
    value: pink
```
2. Configmap
```yaml
env:
  - name: APP_COLOR
    valueFrom:
      configMapKeyRef: 
```
3. Secrets
```yaml
env:
  - name: APP_COLOR
    valueFrom:
      secretKeyRef: 
```

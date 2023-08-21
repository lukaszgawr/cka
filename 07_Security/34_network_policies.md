# Network policies

By default k8s is allow-all. When you install cni that supports netpols it's also at first moment allow all. BUT the moment you apply first netpol on a set of pods, it becomes deny-all allowing only what's in the policy.

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: db-policy
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
    - Ingress
    - Egress
  ingress:
    - from:
        - ipBlock:
            cidr: 192.168.5.10/32
        - namespaceSelector:
            matchLabels:
              name: prod
          podSelector:
            matchLabels:
              name: api-pod
      ports:
        - protocol: TCP
          port: 3306
  egress:
    - to:
        - ipBlock:
            cidr: 192.168.5.10/32
        ports:
        - protocol: TCP
          port: 80
```

> NOTE: notice it's ipBlock OR ( namespaceSelector AND podSelector). Dashes are important!!

We can have multiple "tos":
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: internal-policy
  namespace: default
spec:
  podSelector:
    matchLabels:
      name: internal
  policyTypes:
  - Egress
  - Ingress
  ingress:
    - {}
  egress:
  - to:
    - podSelector:
        matchLabels:
          name: mysql
    ports:
    - protocol: TCP
      port: 3306

  - to:
    - podSelector:
        matchLabels:
          name: payroll
    ports:
    - protocol: TCP
      port: 8080

  - ports:
    - port: 53
      protocol: UDP
    - port: 53
      protocol: TCP
```

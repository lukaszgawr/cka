# Rolling updates & rollbacks

To view status of the rollout:
```
kubectl rollout status deployment/myapp
```

To view the history of deployments&rollouts:
```
kubectl rollout history deployment/myapp [--revision=2]
```

## Deployment strategies
1. Recreate - involves downtime
2. Rolling update - default

To check what strategy is set do:
```
kubectl describe deployment myapp|grep StrategyType
```

## Rollback
```
kubectl rollout undo deployment/myapp [--to-revision=2]
```
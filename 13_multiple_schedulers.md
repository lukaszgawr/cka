# Multiple schedulers

## Configuration of scheduler.
my-kybe-scheduler.yaml:
```yaml
apiVersion: kubescheduler.config.k8s.io/v1
kind: KubeSchedulerConfiguration
profiles:
- schedulerName: my-scheduler
```

## Deploying additional scheduler
1. Download kube-scheduler binary:
```bash
wget https://storage.googleapis.com/kubernetes-release/release/v1.12.0/bin/linux/amd64/kube-scheduler
```
2. Put scheduler manifest in _--config_ argument: --config=/etc/kubernetes/config/my-kube-scheduler.yaml during startging kube-scheduler service

# Deploy additional scheduler as a Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-custom-scheduler
  namespace: kube-system
spec:
  containers:
  - command:
    - kube-scheduler
    - --address=127.0.0.1
    - --kubeconfig=/etc/kubernetes/scheduler.conf
    - --config=/etc/kubernetes/my-kube-scheduler-config.yaml

    image: k8s.gcr.io/kube-scheduler-amd64:v1.11.3
    name: kube-scheduler
```

my-kube-scheduler-config.yaml:
```yaml
apiVersion: kubescheduler.config.k8s.io/v1
kind: KubeSchedulerConfiguration
profiles:
- schedulerName: my-scheduler
leaderElection:
  leaderElect: true
  resourceNamespace: kube-system
  resourceName: lock-object-my-scheduler
```

leaderElection - HA on master nodes (multiple copies on few master nodes and electing the leader)

## Deploying scheduler as Deployment
[Link](https://kubernetes.io/docs/tasks/extend-kubernetes/configure-multiple-schedulers/)

## View schedulers
```bash
kubectl get pods -n kube-system|grep scheduler
```

## Using custom scheduler
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
  schedulerName: my-custom-scheduler
```

## View events to check the scheduling

```bash
kubectl get events -o wide
```
Look for REASON column with "Scheduled" reason.
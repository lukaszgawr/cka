# Commands & arguments


## Running commands
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

If in the container image ENTRYPOINT is defined, then you can omit _command_ and give only _args_.

### Relation between Dockerfile and YAML definition:
* ENTRYPOINT - overrides _command_ 
* CMD - overrides _args_ from YAML
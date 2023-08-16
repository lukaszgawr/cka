# Kubeconfig

Normally without kubeconfig you would have to use the certs like this:

For admin user:  
``` curl https://master-node:6443/api/v1/pods --key admin.key --cert admin.crt --cacert ca.crt ```  
or like this:  
``` kubectl get pods --server master-node:6443 --client-key admin.key --client-certificate admin.crt --certificate-authority ca.crt ```  

It's tedious to work like this so we need kubeconfig

## KubeConfig file

It has 3 sections:
1. Clusters
2. Contexts - binds clusters to users
3. Users

```yaml
apiVersion: v1
kind: Config
current-context: my-kube-admin@my-kube-playground
clusters:

- name: my-kube-playground
  cluster:
    certificate-authority: ca.crt
    server: https://my-kube-playground:6443

contexts:

- name: my-kube-admin@my-kube-playground
  context:
    cluster: my-kube-playground
    user: my-kube-admin
    namespace: finance

users:

- name: my-kube-admin
  user:
    client-certificate: admin.crt
    client-key: admin.key
```

### Viewing kubeconfig
``` kubectl config view ```

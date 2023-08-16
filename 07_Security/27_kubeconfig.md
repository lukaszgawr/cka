# Kubeconfig

Normally without kubeconfig you would have to use the certs like this:

For admin user:  
``` curl https://master-node:6443/api/v1/pods --key admin.key --cert admin.crt --cacert ca.crt ```  
or like this:  
``` kubectl get pods --server master-node:6443 --client-key admin.key --client-certificate admin.crt --certificate-authority ca.crt ```  

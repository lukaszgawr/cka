# Certificates API

All certificate related stuff is handled by kube-controller-manager. It has controllers: CSR-APPROVING & CSR_SIGNING.  
CA that will be used for signing is configurable by _--cluster-signing-cert-file_ and _--cluster-signing-key-file_ in kube-controller-manager.

## Create CSR

``` openssl genrsa -out jane.key 2048 ```  
``` openssl req -new -key jane.key -subj "/CN=jane" -out jane.csr ```  
``` cat jane.csr | base64 | tr -d "\n" ```  or ``` cat jane.csr | base64 -w 0 ```  
Now make jane-csr.yaml with pasted base64-encoded CSR:
``` yaml
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: jane
spec:
  signerName: kubernetes.io/kube-apiserver-client
  usages:
  - client-auth
  request:
    <base64 encoded csr >
```  

## Review Requests

``` kubectl get csr ```

## Approve Requests

``` kubectl certificate approve jane ```

## Share Certs to Users

View the cert with:  
``` kubectl get csr jane -o yaml ```  
Copy cert from the _certificate_ section and share with user.

# Certificates API

## Create CSR

``` openssl genrsa -out jane.key 2048 ```  
``` openssl req -new -key jane.key -subj "/CN=jane" -out jane.csr ```  
``` cat jane.csr | base64 | tr -d "\n" ```  
Now make jane-csr.yaml with pasted base64-encoded CSR:
``` yaml
apiVersion: certificates.k8s.io/v1beta1
kind: CertificateSigningRequest
metadata:
  name: jane
spec:
  groups:
  - system:authenticated
  usages:
  - digital signature
  - key encipherment
  - server auth
  request:
    <base64 encoded csr >
```  

## Review Requests

``` kubectl get csr ```

## Approve Requests

``` kubectl certificate approve jane ```

## Share Certs to Users

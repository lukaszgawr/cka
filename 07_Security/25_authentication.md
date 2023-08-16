# Authentication

## Auth mechanisms

Available mechanisms:
* Basic (static pw file & token file)
* Certificates (TLS)
* Identity services like LDAP, Kerberos, etc.


### Basic - static password file

> Deprecated in k8s v1.19

user-details.csv:
```
password123,user1,u0001,group1
password123,user2,u0002,group2
password123,user3,u0003,group1
```

In arguments of kube-apiserver add --basic-auth-file=user-details.csv

Then to use the credentials:
```
curl -v -k https://master-node-ip:6443/api/v1/pods -u "user1:password123"
```

### Basic - static token file

> Deprecated in k8s v1.19

user-token-details.csv:
```
KpasdasldjalllajasPasdAFJHFSHFGDFG,user10,u0010,group1
LASDFmaksdsDKKJSDKJaaksjdOSDOSDKSS,user11,u0011,group1
```
In arguments of kube-apiserver add --token-auth-file=user-token-details.csv

Then to use the token:
```
curl -v -k https://master-node-ip:6443/api/v1/pods --header "Authorization: Bearer KpasdasldjalllajasPasdAFJHFSHFGDFG"
```

# API Groups

API Groups:
* /metrics
* /healthz
* /version
* /api - core (legacy) group - group is found at REST path /api/v1. The core group is not specified as part of the apiVersion field, for example, apiVersion: v1.
* /apis - named groups - are at REST path /apis/$GROUP_NAME/$VERSION and use apiVersion: $GROUP_NAME/$VERSION (for example, apiVersion: batch/v1)
* /logs

To view API groups:  
``` curl http://localhost:6443 -k ```  
``` curl http://localhost:6443/apis -k ```  
(you may have to add --key, --cert, --cacert for authentication)

We'll focus on /api and /apis  

## Kubernetes API reference

[Link](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.27/)

## Core group (/api)
![core apis](../images/28_core_apis.png)

## Named group (/apis)
![core apis](../images/28_named_apis.png)

## kubectl proxy

```kubectl proxy```  - it will make a proxy to kube-api-server, by default on 8001 port.  
After that you can do from your local machine: ``` curl http://localhost:8001 -k ```


## Namespaced Resources

Pods, replicasets, jobs, deployments, services, secrets, roles, rolebindings, configmaps, PVC, etc.  
Listing namespaced resources: ```kubectl api-resources --namespaced=true```

## Cluster scoped resources

Nodes, PV, clusterroles, clusterrolebindings, certificatesigningrequests, namespaces, etc.  
Listing cluster-scoped resources: ```kubectl api-resources --namespaced=false```

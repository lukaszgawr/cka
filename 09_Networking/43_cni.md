# CNI

![CNI1](../images/43_cni1.png)

Network Namespaces, docker, rkt, mesos, k8s all do the same things like those points 1-8 so CNI defines common standard - e.g. "bridge" program that does in every implementation the same thing. Such programs are called "plugins".

![CNI2](../images/43_cni2.png)

Any runtime should be able to run any plugin.

## CNI Plugins

Available plugins:
* BRIDGE
* VLAN
* IPVLAN
* MACVLAN
* WINDOWS
* DHCP - built-in plugin
* host-local - built-in plugin
* Weave
* Flannel
* Cilium
* VMWare NSX
* Calico
* Infoblox

All those plugins implement CNI Standards.

Docker is another story. It's not CNI compliant.
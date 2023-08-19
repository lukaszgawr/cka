# Docker networking

Types:
* None
* Host
* Bridge

## None

```docker run --network none nginx```  
Docker container is not attached to any network.
Containers with none network cannot comunicate with each other and talk to the outside world.

## Host

Container is attached to the host network.

![host](../images/42_docker_host_networking.png)

Instantiating second nginx container will make it unreachable as the containers share host's network and cannot both listen on 80 port.

## Bridge
Most commonly used.  
Internal private network.

![bridge](../images/42_bridge_networking.png)
![bridge2](../images/42_bridge2.png)

Whenever container is created new network namespace is created for it.

![bridge3](../images/42_bridge3.png)

### Attaching containers to bridge

How docker attaches container (so it's namespace) to its bridge?

![bridge4](../images/42_bridge4.png)
![bridge5](../images/42_bridge5.png)
![bridge6](../images/42_bridge6.png)

### Port mapping

![bridge7](../images/42_bridge7_port_mapping.png)

Internally it's done by iptables (prerouting chain).
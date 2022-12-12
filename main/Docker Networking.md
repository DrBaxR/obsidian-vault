Tags: #devops #docker
Created: 2022-11-21 11:11
References: https://docs.docker.com/config/containers/container-networking/

# Docker Networking
This note is all about [[Docker Network]]s and how to use them. It does not cover the implementation details or how they work under the hood.

The type of network a container uses is transparent from within the container. From the container's point of view, it has a network interface with an IP address, gateway and other networking details.

## Published networks
By default, when you create or run a container, it does not publish any of its ports to the outside world. To do so, you need to use the `--publish`/`-p` flag.

## IP addresses and hostname
By default each [[Container]] is assigned and [[IP address]] for every network it connects to. The docker daemon effectly acts as a [[DHCP]] server.

When the container is started, you can only connect it to a single network, however you can connect a running container to multiple networks using `docker network connect`.

It is possible to explicitly set the container's IP using `--ip` or `--ip6` flags. In the same way, a container's [[Hostname]] defaults to the container's ID. You can override it using the `--hostname` flag when creating it or `--alias` when connecting to a network.

## Bridge networks
Bridge netowrks allow containers connected to the same bridge network communicate, while providing isolation to containers that are not connected to the same bridge network. 

These bridge networks apply to the containers that are running on the **same docker daemon host**.

### User defined bridges
Compared to the default bridge network, containers that run on a user-defined bridge network can identify each other by name or alias, instead of IP.

All containers that do not have a `--network` flag are attached to the default bridge network.

During a container's lifetime you can connect or disconnect it from user-defined networks on the fly.

User-created bridge networks are created and configured using the `docker network create` command. Differect groups of applications can be configured separately, which is an advantage.

### Manage a user-defined bridge
The main command that is used for network management is `docker network`. To create a new network, you can use `docker network create` and to remove the network once all containers are disconnected from it, use `docker network rm`.

To see all the networks on the host, use `docker network ls`.

To add a container to a network when creating it, use something like this:

```sh
docker create --name my-nginx \
  --network my-net \
  nginx:latest
```

To connect a running container to a network, you can use `docker network connect`. Something like this:

```sh
docker network disconnect my-net my-nginx
```
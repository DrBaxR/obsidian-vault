Tags: #linux 
Created: 2024-11-02 11:11
References: https://opensource.com/business/16/8/introduction-linux-network-routing https://www.cyberciti.biz/faq/what-is-a-routing-table/

# Linux Network Routing

This document describes how [[Linux]] network routing and routing tables work.

## Description
Network routing is used by computes that are connected to a networks and it consists of a bunch of routing instructions for [[TCP/IP]] packets when they leave the local host.

## Routing Table
The routing table contains information about where TCP/IP data packets need to be routed.

Here is an example of how a very simple routing table looks like:
```
[root@host1 ~]# route -n Kernel IP routing table 
Destination   Gateway         Genmask       Flags Metric Ref Use Iface 
0.0.0.0       192.168.0.254   0.0.0.0       UG    100    0   0   eno1 
192.168.0.0   0.0.0.0         255.255.255.0 U     100    0   0   eno1
```

Here are the note-worthy things about this routing table:
- The default gateway is shown as *Destination* `0.0.0.0`, the destination of the default gateway being, in this case, the gateway router.
- The *Genmask* `0.0.0.0` for the default network means that any packet that is not picked up by another entry in the table should be picked up by it.
- The *Iface* represents the [[NIC]] used to send the packet.
- The *Flags* (in this case) represent either gateway (G) or up (U).

## Routing Decisions
Here is how most hosts decide where to route the packet they want to send:
1. If destination is in local network, send it directly to it.
2. If destination is in remote network reachable by a local gateway defined in route table, send it to the explicitly defined gateway.
3. If destination is in remote network not defined in route table, send to default gateway.

## Routing Configuration
The routing table can be configured by the `ip route` command.
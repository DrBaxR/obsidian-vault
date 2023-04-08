Tags: #reference 
Created: 2023-03-26 15:03

# L3 Internetworking on Linux
In linux, all the tools used by end-nodes that want to reach other end-nodes are exactly the same as the tools used by routers to forward packets to end-nodes.

## Neighbor Table
When an IP node wants to communicate with a system in the same layer 2 domain:
1. It first looks in its **neighbor table** (or [[ARP Tables]])
2. If destination [[IP address]] is not int the table, then the node issues an [[ARP]] request, which is broadcast to everyone in the l2 domain (something like *"Please tell me the [[MAC Address]] for the node with IP address X.X.X.X"*)
3. Assuming the target device is available, the device with the IP address responds to the broadcast request

In Linux you can view the neighbor table using the `ip neighbor show` command, which will produce an output that looks something like this:

```
192.168.100.1 dev wlp1s0 lladdr 2c:9d:1e:1b:5b:63 REACHABLE
192.168.100.31 dev wlp1s0 lladdr 48:6d:bb:c4:ec:fd STALE
```

The output of the command includes a list of:
- IP addresses that have been recently resolved to MAC addresses
- Which interface is used to reach the l2 network where they can be reached
- Their associated MAC addresses
- The confidence of knowing there IP/MAC address relationships

## IP Routing
The routing table has knowledge of **specific networks that a node can reach**. Minimally, each routing table contains at least a *default route*, where the node can send any IP packet that is not in an attached l2 network.

The routing table can be viewed using the `ip route show` command, which will produce an output that looks like this:

```
default via 192.168.100.1 dev wlp1s0 proto dhcp src 192.168.100.40 metric 600
172.17.0.0/16 dev docker0 proto kernel scope link src 172.17.0.1 linkdown
192.168.100.0/24 dev wlp1s0 proto kernel scope link src 192.168.100.40 metric 600
```

Here you can see that the network `192.168.100.0/24` can be reached using the `wlp1s0` interface. Also, the routing table include sa route to the default gateway (`192.168.100.1`) that will be used to reach any node that isn't on the local network.

The routing table can be modified in different ways:
- By assigning IP addresses to node interfaces
- By manually adding or removing them using the `ip route` command
- By dynamically inserting them using routing protocols

## Resources
https://www.actualtechmedia.com/wp-content/uploads/2017/12/CUMULUS-NETWORKS-Linux101.pdf
Tags: #reference 
Created: 2023-03-25 14:03

# L2 vs. L3 Internetworking
Layer 2 and Layer 3 refer to the [[OSI Model]] layers. The first layer includes the physical media used to connect the network. Specifications there describe cable qualities and properties of electrical and optical signals used to move bits around.

## Layer 2
This layer is called the **data link** layer. The most common l2 protocol is [[Ethernet]], which encapsulates the data in *frames*. Ethernet frames include the [[MAC Address]] of the source and destination.

Layer 2 networking works in one of two ways:
1. The device (switch or whatever) knows where to send the frame it received and it sends it to the port where it knows the destination exists
2. If the destination is unknown by the device, then the frame is broadcast to all the nodes

Layer 2 networking has two major limitations:
1. It allows sending data to unknown locations, which is a bad thing because this type of data affects all the nodes in the network
2. Layer 2 networks have globally unique MAC addresses that are assigned by the manufacturers. This is a problem because you can not organize them into a hierarchy.

## Layer 3
Layer 3 solves both of the major l2 problems:
1. If you try to send a packet to an unknown destination, then the router will drop it.
2. [[IP address]]es have a hierarchical structure and can be assigned by the entities that want to use them. For example, an office can be assigned all the IP addresses of as single IP subnet (for example `10.0.0.0`).

When a node needs to send an IP packet, it consults the [[ARP Tables]] and sends the packet to the device that is most likely to get that packet where it needs to go:
1. If the destination is in the same layer 2 network, and entry in the ARP table tells the sender how to use the layer 2 internetworking.
2. If the IP device needs to communicate with other IP-based addresses that are outside the local l2 network, the ARP table may point to a router that will take the packet closer to the destination or drop the packet, if a matching route does not exist for the destination.

## Resources
https://www.actualtechmedia.com/wp-content/uploads/2017/12/CUMULUS-NETWORKS-Linux101.pdf
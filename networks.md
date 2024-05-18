# Networks

## Links

[*IP address, Network address, and Host address Explained*](https://www.computernetworkingnotes.com/networking-tutorials/ip-address-network-address-and-host-address-explained.html)
[*Calculating the Range of IP Addresses from Subnet Mask*](https://www.baeldung.com/cs/get-ip-range-from-subnet-mask)


## IPv4 and subnet masks

Computers communicating with each others form a **network**. A large network is composed of **subnets**.

An IP address is 32bits, 4 groups of 8bits/4octets each. Binary, decimal or hexadecimal form.

IP addresses can be of two kinds :
- **Network addresses** : to locate the subnet a device is in.
- **Host addresses** : to locate the device into this subnet.

How can we tell the two apart ? IPv4 uses an additional component, known as the **subnet mask**.

The subnet mask determines how many bits are used for each address in an IP address. It uses the same notation the IP uses, it is also 32bits. OR: /x with x from 1 to 32. CIDR Conversion table.

Each subnet has of course its own subnet mask.

A subnet mask is composed of **switches**, each bit from the IP address has its own switch, which is it assigned bit from the subnet mask. If an IP bit belongs to the **network** portion, the subnet mask will turn **on** the switch, if it belongs to the **host** portion, it will turn it **off**.

off = 0, on = != 0

Without the subnet mask, an IP address is considered ambiguous.

## Host addresses

Provides a unique identity to the interface in a subnet.

In comp networks, IP addresses are assigned on **interfaces**. An interface connects **one** device to the network. All interfaces have unique IP addresses. In a busy network, if IP addresses **overlap**, many devices will process packets and the network will be down.

## Network addresses

Provides a unique identity to the subnet in a network. Is common to all the devices from this subnet.

Hosts from different subnet cannot communicate directly. **Routers** are used to connect different subnets. They **store the network addresses of all available subnets** in their **routing table**.

A **gateway router** connects the subnets of a network between each others.

A computer that wants to send a data packet outside of its subnet sends it to the gateway router, that forwards it to the router connected to the destination subnet, or a router that knows how to reach the destination subnet. 

**Routers only use network addresses**.

How to read the routing table:
A Routing table maintains a map of connected networks, essential for the router to know how to get a data packet to a network.
A routing table entry is composed of:
- A network destination
- Its Netmask
- a gateway (next IP address forwarded to)
- an interface
- a metric
Each routing table has a default entry, used to get the data packet where it needs to go when the network is not on the table. The gateway in this case is often remote ex: Internet.

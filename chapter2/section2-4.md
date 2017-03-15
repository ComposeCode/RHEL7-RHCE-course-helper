# Chapter 3, Section 1: Routing

A RHEL system can be configured to route network traffic but in a very simplistic fashion.  One of three rules is applied in the routing mechanism to determine the correct route:

- If the source and destination systems are on the same network, the packet is sent directly to the destination system.
- If the source and destination systems are on two different networks, all defined (static or dynamic) routes are tried one after the other. If a proper route is determined, the packet is forwarded to it, which then forwards the packet to the correct destination.
- If the source and destination systems are on two different networks but no routes are defined between them, the packet is forwarded to the default router (or the default gateway), which attempts to search for an appropriate route to the destination. If found, the packet is delivered to the destination system.

## The Routing Table

A routing table preserves information about available routes and their status. It may be built and updated dynamically or manually by adding or removing routes. The route command can be used to view the routing table on RHEL7 systems:

```
[chartwell@workstation ~]$ route
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         gateway         0.0.0.0         UG    100    0        0 enp0s3
10.0.0.0        0.0.0.0         255.128.0.0     U     0      0        0 tun0
10.0.0.0        0.0.0.0         255.0.0.0       U     0      0        0 tun0
infoblox-trust0 0.0.0.0         255.255.255.255 UH    0      0        0 tun0
vpn1-5-66.ams2. 0.0.0.0         255.255.255.255 UH    0      0        0 tun0
infoblox-trust0 0.0.0.0         255.255.255.255 UH    0      0        0 tun0
192.168.1.0     0.0.0.0         255.255.255.0   U     100    0        0 enp0s3
vpn-ams2.redhat gateway         255.255.255.255 UGH   0      0        0 enp0s3
```

Another way to view the table is by using the ip command:

```
[chartwell@workstation ~]$ ip route
default via 192.168.1.1 dev enp0s3  proto static  metric 100
10.0.0.0/9 dev tun0  scope link
10.0.0.0/8 dev tun0  scope link
10.35.255.14 dev tun0  scope link
10.36.5.66 dev tun0  scope link
10.38.5.26 dev tun0  scope link
192.168.1.0/24 dev enp0s3  proto kernel  scope link  src 192.168.1.121  metric 100
209.132.186.252 via 192.168.1.1 dev enp0s3  src 192.168.1.121
```

This output explained:

- The first column specifies the Network Destination address. The keyword default in this column signifies the default gateway for outing out traffic to other virtual networks in the absence of a proper route.
- The dev column is the name of the physical or virtual interface used to sned out traffic.
- The proto column is used to identify the routing protocol used as defined in /etc/iproute2/rt_protos file. The proto kernel implies that the route was installed by the kernel during auto-configuration. The keyword static in this columns means a static route has been applied, most likely created manually.
- The scope column determines the scope of the destination as defined in the /etc/iproute2/rt_scopes file. Values may be global, nowhere, host or link.
- The src column shows the resource address associated with the interface for sending data out to the destination.
- The metric column displays the cost of using a route, which is usually the numer of hops to the destion system. Systems on the local network are one hop, and each subsequent router thereafter is an additional hop.

## Configuring Routes

```
  Need example of configuring routes manually.
```

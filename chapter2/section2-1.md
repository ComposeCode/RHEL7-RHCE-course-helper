# Chapter 2, Section 1: Configuring Bonding, Teaming, IPv6, and Routing

This chapter covers advanced network configuration: bonding, teaming, IPv6 and routing (static routes etc).  

## Link aggregation

Link aggregation is a technique by which two or more network interfaces are logically configured to provide higher performance using their combined bandwidth and fault tolerance should all but one of them fail. Two common ways to provide link aggregation is through bonding and teaming. Both of these options are natively supported in RHEL7.

IPv4 has long be deprecated and IPv6 should start to become more prevalent. Therefore, it is now an integral part of the RHCE exam. Routing is the process of selecting a path for traffic in a network, or between or across multiple networks. RHEL systems can be configured to provide routing with limited capability.

There are three major advantages of link aggregation:

- Better throughput: A single interface will have a speed limit, so combing interfaces allows for more throughput.
- Load Balancing: Connections can be distributed across multiple interfaces instead of using a single one.
- Fault Tolerance: if an interface goes down, the others can pick up the slack.

There are two major ways to perform link aggregation, known as bonding and teaming. Bonding has been around for many years but teaming is relatively new.  The tool bond2team tool can be used migrate from bonded interfaces to teamed interfaces.

Bonding and teaming can be configured by modifying text files for both IPv4 and IPv6 networks. The NetworkManager CLI or TUI and the Gnome Network Connections GUI. Additional tasks such as activating or deactivating logical channels can also be performed with these and other OS tools as well.

## Interface bonding

Bonding interfaces provides the ability to bond two or more network interfaces together into a single, logical bonded channel that acts as the master for all slave interfaces that are added to it. An IP address is applied to the bond rather than to individual slaves within the bond. Users and applications will see the bond device as the connection to use rather than the individual interfaces within it.

The support for bonding is integrated entirely into the kernel as loadable kernel module. This particular kernel module is called bonding. A bonded channel is configured to use one of several modes of operation that dictate its overall behavior.

These modes are described below:

- Round-robin:  This mode distributes traffic evenly across the slave interfaces. This mode supports both load balancing and fault tolerance. It is specified as balance-rr.

- Active-backup: Only one slave is active at a time and all others are available but in passive mode. If the active interface fails, one of the passive slaves takes over and becomes active. This mode of operation does not provide load balancing, however it does provide fault tolerance. It is specified as active-backup.

- XOR: This mode uses source and destination Ethernet addresses to transfer network traffic. This mode provides both load balancing and fault tolerance. It is specified as balance-xor.

- Broadcast: This mode transmits network traffic on all slaves. This mode provides fault tolerance only. It is specified as broadcast.

## Configure Bonding Manually

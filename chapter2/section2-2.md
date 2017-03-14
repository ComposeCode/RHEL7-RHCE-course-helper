# Chapter 2, Section 1: Interface Teaming

This section details how to configure Interface Teaming. This was a new feature introduced in RHEL7 and provides an additional alternative to enhance throughput and fault tolerance at the network interface level. This technique does not conflict with bonding in anyway. Bonding has been around for a long time and is considered robust and mature. Teaming is a much newer implementation.

Teaming handles the flow of network packets faster than bonding does and unlike bonding, which is accomplished purely in the kernel space and provides no user control over its operation, teaming only requires the integration of the essential code into the kernel and the rest is implemented via the teamd daemon. This can be controlled using the teamdctl command from the terminal.

### Configuring Teaming manually

Need example of configuring teaming manually. 

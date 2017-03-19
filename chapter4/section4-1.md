# Chapter 4, Section 1: Working with Firewalld and Kerberos

This chapter will cover Firewalld and Kerberos: Zones, Services, direct language, rich language and port forwarding. We will also cover the basics of Network Address Translation (NAT) and IP masquerading. We will also look at using and managing Kerberos.

## Kerberos

Kerberos is a client/server authentication protocol that works on the basis of digital tickets to allow systems communicating over non-secure networks to prove their identity to ne another before being able to use kerberized network services. Kerberos uses a combination of Kerberos services and encrypted keys for the implementation of secure authentication mechanism on the network.

### Firewalld

Firewalld provides a new way of interacting with iptable rules. It allows the administrator to enter new security rules and activate them during runtime without disconnecting existing connections. It places network interfaces in different zones based on the level of trust for the traffic transmitted through them, thereby providing the administrator the flexibility to activate specific zones only. Network Address Translation is a feature that enables a system on the internal network to access the internet via an intermediary device.

The daemon process for the new firewall is called firewalld and it is responsible for the configuration and monitoring of system frewall rules. The old method of using the iptables command requires the reload of all defined rules, including those that are already in an active and established state, whenever there is a change. Firewalld supports the D-BUS implementation which brings the concept of network zones to manage security rules. Everything in firewalld relates to one or more zones. Iptables does not have a daemon process, as it is implemented purely in the kernel space. We can activate either of the two at a time.

The firewalld configuration is stored in the /etc/firewalld directory and can be cusotmized as desitred. Its essential code runs in the kernel space interfacing with netfilter to implement the firewall rules. The rest of the code including the daemon is implemented in userland providing full user control over its operations. The userland management tools are the command firewall-cmd and the graphical firewall configuration tool called firewall-config. We can also modify and create zone and service configuration files by hand and activate them as required.

### Network Zones

Firewalld zones classify incoming network traffic for simplified firewall management. Zones define the level of trust for network connections based on principles such as a source IP or network interface for incoming network traffic. The inbound traffic is checked against zone settings and it is handled appropriately as per configured rules in the zone. Each zone can have its own list of services and ports that are opened or closed. We can create zones with different rulesets. For instance, on a RHEL7 system with multiple network interfaces, we can group interfaces based on pre-defined trust levels and place them into one or more zones that may be activated or deactivated independently as one entity.

Nine zones are provided by default which are shown below:
```
[chartwell@workstation zones]$ ll
total 44
-rw-r--r--. 1 root root 299 Aug 16  2016 block.xml
-rw-r--r--. 1 root root 293 Aug 16  2016 dmz.xml
-rw-r--r--. 1 root root 291 Aug 16  2016 drop.xml
-rw-r--r--. 1 root root 304 Aug 16  2016 external.xml
-rw-r--r--. 1 root root 343 Aug 16  2016 FedoraServer.xml
-rw-r--r--. 1 root root 525 Aug 16  2016 FedoraWorkstation.xml
-rw-r--r--. 1 root root 369 Aug 16  2016 home.xml
-rw-r--r--. 1 root root 384 Aug 16  2016 internal.xml
-rw-r--r--. 1 root root 340 Aug 16  2016 public.xml
-rw-r--r--. 1 root root 162 Aug 16  2016 trusted.xml
-rw-r--r--. 1 root root 336 Aug 16  2016 work.xml
[chartwell@workstation zones]$ pwd
/usr/lib/firewalld/zones
[chartwell@workstation zones]
```

These zones are described below. They are sorted by trust level (untrusted to rusted). We need to select the zone that best suits our network requirements, and then we can tailor it to further meet our specific needs:

- Drop: This Zone drops all inbound connection requests without sending a message back.
- Block: Blocks all inbound connection requests with icmp-host-probhibited message for IPv4 or icmp6-adm-prohibited message for IPv6 sent.
- Public: Allows selected inbound connection requests and disallows the rest.
- External: Allows selected inbound connection requests with masquerading active.
- DMZ: Allows selected inbound connection requests. This is suited for systems with limited access to their internal network.
- Work: Allows selected inbound connection requests from other corporate systems.
- Home: Allows selected inbound connection requests from other home systems.
- Internal: Allows selected inbound connection requests on internal networks where most systems are trusted.
- Trusted: Allows all inbound connection requests; used on a highly trusted network.

The public zone is the default zone. However, this designation can be assigned to one of the other eight zones or to a new custom zone. The file for the public zone can be found in the /usr/lib/firewalld/zones directory.

```
[chartwell@workstation zones]$ cat public.xml
<?xml version="1.0" encoding="utf-8"?>
<zone>
<short>Public</short>
<description>For use in public areas. You do not trust the other computers on networks to not harm your computer. Only selected incoming connections are accepted.</description>
<service name="ssh"/>
<service name="dhcpv6-client"/>
</zone>
```

This file is quite easy to read and is pretty self explanatory, it indicates that in the default zone, the ssh and dhcp protools are enabled by default. We can use one of these files as a template to create our own customer zone and place it in the /etc/firewalld/zones directory. This custom zone can then be altered with the firewall-cmd command or the graphical firewall-config tool to suit our specific needs.

Each zone on the system may have one or more interfaces assigned to it. When a service request arrives, firewalld checks whether it is already defined in a zone by the IP it is originated from (the source network) or the network interface it is coming through. If yes, it binds the request with that zone, otherwise, it binds the request with the default zone. This allows us to define and activate several zones at a time even if there is only one network interface on the system.

### Services
Using services in zones is the preferred method for firewalld configuration and management. A service typically contains a port number, protocol and an IP address. Service configuration is stored in separate XML files located in the /usr/lib/firewalld/services and /etc/firewalld/services directories for system and user defined services respectively. The configuration files located in the user-defined service directory (/etc/firewalld/services) take precedence over the ones located else where.

It is recommended to copy one of the files from /usr/lib/firewalld/services to /etc/firewalld/services, rename it, and use the firewall-cmd or the graphical firewall-config tool to alter the contents to suit specific needs.


```
[chartwell@workstation zones]$ cat public.xml
<?xml version="1.0" encoding="utf-8"?>
<zone>
<short>Public</short>
<description>For use in public areas. You do not trust the other computers on networks to not harm your computer. Only selected incoming connections are accepted.</description>
<service name="ssh"/>
<service name="mdns"/>
<service name="dhcpv6-client"/>
</zone>
```

### Ports

Network ports in firewalld may also be defined directly without using the service configuration technique. In essence, defining network ports does not require the presence of a service or a service configuration file. The same two tools, firewall-cmd and firewall-config, used for zone and service configuration are also used for port configuration.

### Direct Interface and Rich Language

Firewalld gives us the ability to pass security rules directly to iptables using the direct interface mode; however, these rules are not persistent. We can create rules using rich language instead, using the two management tools, firewall-cmd and firewall-config.

Rich language uses several elements to set rules and name them. These elements include a source address or range within an appropriate netmask; destination address or range with an appropriate netmask; service name; port number or range; protocol; masquerade (enable or disable); forward-port (destination port number or range to divert traffic to); log and log level and an action (accept: to grand new connection requests, reject: to disallow within a reason returned or drop: discard requests without informing the sender).

### Network Address Translation (NAT) and IP Masquerading.
NAT refers to the process of altering the IP address of a source or destination network that is enclosed in a datagram packet header while it passes through a device that supports this type of modification. To summarize, NAT allows a system on an internal network (home or corporate) to access external networks (the internet) using a single, registered IP address configured on an intermediary device (a router or firewall). 

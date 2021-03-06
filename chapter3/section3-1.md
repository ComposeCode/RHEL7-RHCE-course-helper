# Chapter 3, Section 1: Network Time Protocol (NTP)

This chapter covers Network Time Protocol (NTP). We will cover the NTP protocol, time sources, NTP roles and stratum levels. We will also cover the NTP packages, tooling and the configuration files.

Network Time Protocol (known as NTP) is used to maintain the clock on the system and keeps it synchronized with a more accurate and reliable time source. Providing an accurate source of time is important for time sensitive applications, such as monitoring software, backup tools, scheduling utilities, billing systems, file sharing protocols and authentication programs (including TSL/SSL) to perform correctly and precisely.  Other services for logging and are also aided by accurate time as they have accurate time stamps inside their log files/alerts.

### The NTP Protocol (NTP)

The NTP protocol is a networking protocol for synchronizing the system clock with timeservers that are physically closer and redundant for high accuracy and reliability. This protocol was developed over three decades ago and is still in wide use across the world. Millions of devices still use and rely on the service to obtain time from thousands of configured NTP servers spread across the globe. NTP supports both client-server as well as peer-to-peer configurations within an option to use either public-key or symmetric-key cryptography for authentication. When used correctly, time accuracies are typically within a millisecond.  

The NTP daemon is called NTP and uses the HDP protocol over the well known port 123, and it runs on all participating servers, peers and clients. This daemon typically starts at system boot and continuously operates to keep the operating system clock in sync with a more accurate source of time. In order to understand NTP, a discussion of its components and roles is imperative.

A time source is any device that acts as a provider of time to other devices. The most accurate source of time is provided by atomic clocks that are deployed around the globe. Atomic clocks use Universal Time coordinated (UTC) for time accuracy. They produce radio signals that radio clocks use for time propagation to computer servers and other devices that require accuracy in time. When choosing a time source for a network, preference should be given to the one that is physically close and takes the least amount of time to send and receive NTP packets.

The most common sources of time employed on computer networks are local system clock, an Internet-based public timeserver, a radio clock and a satellite receiver.

### Local System Clock

You can arrange for one of the RHEL systems to function as a provider of time using its own clock. This requires the maintenance of correct time on this server either manually or automatically via the cron daemon. Keep in mind, however, that this server has no way of synchronizing itself with a more reliable and precise external time source. Therefore, using a local system clock as a timeserver with reliance on its own clock is the least recommended option.

## Internet-based public timeserver
Several public timeservers (can be found through http://ntp.org) are available via the internet for the provision of time on the network. One of the systems on the local network is identified and configured to obtain time from one or more of the public timeservers. To use a time source, you may need to open a port in the firewall to allow NTP traffic to pass through. Internet-based timeservers are spread around the world and are typically operated by government agencies, research organizations and universities. This option is preferred over the use of local system clock.

A radio clock is regarded as the most accurate provider of time. A radio clock receives time updates from one or more atomic clocks. Global Positioning System (GPS), National Institute of Science and Technology (NIST) radio station WWVB broadcasts in the Americas and DCF77 radio broadcasts in Europe are some popular radio clock methods. Of these, GPS-based sources are perfect. A direct use of signals from any of these devices requires connectivity of some hardware to a computer that is intended to be used as an organizational or site-wide timeserver.

## NTP Roles
A role is a function that a system performs from an NTP standpoint; a system can be configured to assume one or more of the following roles:

- Primary NTP Server: a primary NTP Server gets time from one of the time sources mentioned above, and provides time to one or more secondary servers or clients or both. It can also be configured to broadcast time to secondary servers and clients.
- Secondary NTP Server: a secondary NTP server receives time from a primary server or directly from one of the time sources mentioned above. It can also be configured to broadcast time to secondary servers and clients.
- NTP Peer: an NTP peer provides time to an NTP serer and receives time from it. All peers work at the same stratum level, and all of them are considered equally reliable. Both primary and secondary servers can be peers of each other.
- NTP Client: an NTP client receives time from either a primary or secondary server. A client can be configured in one of the following ways:
  * As a polling client that contacts a defined NTP server directly for time synchronization
  * As a broadcast client that listens to time broadcast by an NTP server. A broadcast client binds itself with the NTP server that responds to its requests and synchronizes its clock with it. The NTP server must be configured in the broadcast mode in order for a broadcast client to be able to bind to it. A broadcast NTP configuration cannot span the local subnet.
  * A multicast client operates in a similar fashion as a broadcast client; however, it is able to span the local subnet. The NTP server must be configured in the multicast mode in order for a multicast client to work with it.
  * A manycast client automatically discovers manycast NTP servers and uses the ones with the best performance. The NTP server must be configured in the many cast mode in order for a many cast client to work with it.

## Stratum levels

Need explanation of stratum levels.

```
  Need diagram explaining Stratum levels
```

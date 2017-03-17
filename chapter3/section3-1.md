# Chapter 2, Section 1: Configuring Bonding, Teaming, IPv6, and Routing

This chapter covers Network Time Protocol (NTP). We will cover the NTP protocol, time sources, NTP roles and stratum levels. We will also cover the NTP packages, tooling and the configuration files.

Network Time Protocol (known as NTP) is used to maintain the clock on the system and keeps it synchronized with a more accurate and reliable time source. Providing an accurate source of time is important for time sensitive applications, such as monitoring software, backup tools, scheduling utilities, billing systems, file sharing protocols and authentication programs (including TSL/SSL) to perform correctly and precisely.  Other services for logging and are also aided by accurate time as they have accurate time stamps inside their log files/alerts.

### The NTP Protocol (NTP)

The NTP protocol is a networking protocol for synchronizing the system clock with timeservers that are physically closer and redundant for high accuracy and reliability. This protocol was developed over three decades ago and is still in wide use across the world. Millions of devices still use and rely on the service to obtain time from thousands of configured NTP servers spread across the globe. NTP supports both client-server as well as peer-to-peer configurations within an option to use either public-key or symmetric-key cryptography for authentication. When used correctly, time accuracies are typically within a millisecond.  

The NTP daemon is called NTP and uses the HDP protocol over the well known port 123, and it runs on all participating servers, peers and clients. This daemon typically starts at system boot and continuously operates to keep the operating system clock in sync with a more accurate source of time. In order to understand NTP, a discussion of its components and roles is imperative.

A time source is any device that acts as a provider of time to other devices. The most accurate source of time is provided by atomic clocks that are deployed around the globe. Atomic clocks use Universal Time coordinated (UTC) for time accuracy. They produce radio signals that radio clocks use for time propagation to computer servers and other devices that require accuracy in time. When choosing a time source for a network, preference should be given to the one that is physically close and takes the least amount of time to send and receive NTP packets.

The most common sources of time employed on computer networks are local system clock, an Internet-based public timeserver, a radio clock and a satellite receiver.

You can arrange for one of the RHEL systems to function as a provider of time using its own clock. This requires the maintenance of correct time on this server either manually or automatically via the cron daemon. Keep in mind, however, that this server

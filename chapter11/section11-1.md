# Chapter 11, Section 1: Configuring DNS

This chapter will cover configuring DNS (BIND) on RHEL7 systems. We will look at BIND, the SELinux requirements, the configuration and zone files, caching, and testing/troubleshooting.

### What is DNS?

DNS is a tree-like distributed structure that is employed on the Internet and corporate networks as the defacto standard for resolving hostnames to their numerical IP addresses. The protocol is both hardware and OS independent. Name lookups can be made both using IP addresses and domain names (reverse and forward lookups) if the entries are present on the domain name servers. It works based on a client/server architecture where one central server provides name resolution to many remote clients.

### What is BIND?

Bind, Berkeley Internet Name Domain is an open source implementation of DNS on Unix/Linux operating systems. RHEL7 includes BIND as the standard DNS package.

### BIND Software packages

- The bind software packages are: bind, bind-libs and bind-utils.

- Bind provides the main binaries for BIND. The libs package contains library files for bind and bind-utils. The bind utils package contains resolver tools, such as dig, host and nslookup.

### DNS Name Space and Domains

- The DNS Name space is a hierarchical organization of all the domains on the Internet. The root of the name space is represented by a dot. The hierarchy right below the root represents top-level domains (TLDs) that are either generic, such as .com, .net, .edu, .org and .gov, and referred to as gTLDs, or specific to two-letter country-code, such as .ca and uk and referred to as ccTLDs. A DNS domain is a collection of one or more systems.

- Sub-domains fall under domains and are separated by a dot. For example, .com domain consists of a second-level domains, such as .redhat and .ibm, with further division into multiple, smaller, third-level domains, such as bugzilla.redhat.com under .redhat.com

```
  Need diagram of DNS hierarchy.
```

- Need to explain the DNS hierarchy (leaves, etc)

### DNS Root Servers

The root servers sit at the top of the DNS hierarchy in the root zone and are managed and maintained by ICANN. There are currently 13 opterional root servers, with a number of mirrors for each one are placed across the globe to offload the main servers. Information about the root servers is supplied as part of the bind software and it is stored in the named.ca file in the /var/named directory.

```
  need a list of the ROOT dns servers
```

- The root DNS servers handle queries for TLDs only and provide the client with the IP address of the name server that is responsible for a requested TLD.

### DNS Roles

- A role is a function that a system performs from a NDS standpoint, a system typically is configured to operate as one of three types of DNS server or as a client. A DNS server, also refered to as a nameserver, stores the DNS records for a domain and responds to client queries for name resolution.

- Primary DNS Server: a primary (aka master) DNS server has the authority over its domain (or sub-domain) and maintains that domain's original (or master) data. Each domain must have one primary server with one or more optional DNS servers, referred to as secondary and caching server. Zone data files are maintained on the primary server and they can be propagated to secondary servers.

- Secondary DNS server: a secondary (aka slave) DNS server has the authority for its domain and stores that domain's zone data files; however, these files are copied over from the primary server. When updates are made to the zone files on the primary server, the second server gets a copy of the updated files automatically.

- Caching DNS Server: a caching DNS server has no authority for any domains. It gets data from a primary or secondary server and caches it locally in the memory. Like a secondary server, a caching server is used for redundancy and load sharing, but its more common use is due to its ability to provide faster responses to queries as it stores data in the memory rather than on the disk.

- Forwarding DNS Server: a forwarding DNS server has no authority for any domains. It simply forwards an incoming query to a specified DNS server.

- DNS Client: a DNS Client is used for initiating and sequencing hostname queries by referencing name server information defined in resolver configuration files.

### Types of Nameserver Configuration

- There are two fundamental types of DNS server configurations referred to as authoritative and recursive. An authoritative DNS server is usually a primary or secondary server that provides authoritative responses to name resolution queries from its own zone data files.

- In contrast, a recursive DNS server, which is usually a caching server, is able to query the next available DNS server in order to get a response, if it does not have one. This type of server configuration does not store authoritative records; rather the server polls other authoritative servers for that information. Upon receiving a reply, the caching server caches it locally from a preset time period.  

### DNZ Zones and Zone Files  

- Every DNS server maintains complete information about the portion of the NDS name space it is responsible for. This information includes a complete set of authoritative data files.

- The portion of the name space for which a NDS server has a complete set of authoritative data files is known as the server's zone. The set of authoritative data files is known as zone files or zone databases.

- The zone files contain directives and resource records. Directives control the behaviour of a DNS server and instruct how to undertake tasks or apply special settings to the zone; resource records describe zone limits and assign each individual host an identity.

- In order to provide the name resolution service to a zone, resource records must be defined, directives are optional. Directives in a zone file are prepended by the dollar ($) sign. Some of the common directives are $INCLUDE, $ORIGIN, and $TTL that let you include additional zone files, append the domain name to simple hostnames and define the default validity (time to live) setting, in seconds, for the zone, respectively.

The different types of records are described below:

- A or AAAA Address Record. Specifies an IPv4 or IPv6 address to be mapped to a hostname.
- CNAME. Canonical Name record. Maps an alias to a real name.
- MX. Mail Exchanger record. Points to a weighted list of mail servers configured to receive mail for a domain.
- NS, nameserver record. Specifies the name of an authoritative nameserver.
- PTR, pointer record. Points to a different location in the namespace. it is usually used for reverse lookups.
- SOA, start of authority record. Defines key authoritative data for a namespace that includes primary DNS server, email address of the administrator and the following:
  - Serial: indicates the number of times this zone file has been updated.
  - Refresh: identifies the amount of time secondary servers wait before requesting zone updates.
  - Retry: determines the amount of time secondary servers wait before they reissue a request for zone updates.
  - Expiry: sets the amount of time for secondary servers wait before they reissue a request for zone updates.
  - Minimum: denotes the amount of time name servers cache the zone's data.
  - Values may be set in seconds (Default), minutes (M), hours (H), days (D) or weeks (W). Comments are followed by the ; character. A domain name at the very beginning identifies the owning domain for this zone. If there is a @, it will point to the value of the $ORIGIN variable. The IM before the SOA identifies it is an Internet record.

A sample zone file is presented below:

```
  # Need output from a example zone file
  $TTL 86400                                           [24 hours; time to hold data in cache.]
  $ORIGIN example.com                                  [name of the domain]
  @ IN SOA server1.example.com. root.example.com. (    []
      0 ; serial                                       []
      1D ; refresh                                     []
      1H ; retry                                       []
      1W ; expire                                      []
      3H) ; minimum                                    []
        IN  NS server1.example.com.
server1 IN A 192.168.0.110
        IN MX 8 server1.example.com.

server1 IN A 192.168.0.110
        IN MX 9 server2.example.com.
server2 IN A 192.168.0.120
```

All zone database files are maintained by the primary DNS server.

### The named.conf file

- The main configuration file for Bind is /etc/named.conf. It provides the DNS server with the names and locations of zone databases for al the domains the server is responsible for.

- This file typically contains options, zone, and include statements. Each statement begins with { and ends in };. The default version of this file is preconfigured for use as a caching only name server.

Below is a example named.conf file:

```
  # need named.conf file from server
```

- In this file, comments begin with the // characters or can be enclosed with /* and */ tags.

- Options can be defined at the global level within the options statement or at an individual level within a zone statement, as required.
- The listen-on directive defines port 53, which the DNS server uses to listen to queries on the localhost interface and the interface configured on the 192.168.0.111 address.
- The systems (or networks or domains), defined within curly brackets and separated by the semicolon character, are called match-list.
- The directory directive identifies the working directory location for the named service.
- The dump-file and statistics-file directives specify the location for the DNS dump and statistics files.
- The memstatistics-file specifies the location for the memory usage statistics file.
- The pid-file directive provides the file name and location to store the named daemon's PID.
- The allow-query directive defines a match-list which enables the specified systems to query the DNS server. You can restrict queries to one or more networks by specifying IP addresses of the networks.
- The recursive directive instructs the DNS server to act as a recursive server.
- The next three directives - dnssec-enable, dnssec-validation, and dnssec-lookaside are related to DNS security.
- The logging function instructs the DNS server to log debug messages of dynamic severity to the /var/named/data/named.run file.
- 

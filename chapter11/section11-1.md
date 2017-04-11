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
  // need named.conf file from server
  // sample is in /usr/share/doc/bind*/sample
  options {
      listen-on       port 53 {127.0.0.1; 192.168.0.111;};
      listen-on-v6    port 53 {::1; };
      directory           "/var/named";
      dump-file           "/var/named/data/cache_dump.db";
      statistics-file     "/var/named/data/named_stats.txt"
      memstatistics-file  "/var/named/data/named_mem_stats.txt";
      pid-file            "/run/named/named.pid";
      allow-query         { localhost; };
      ....
      recursion           yes;
      dnssec-enable       yes;
      dnssec-validation   yes;
      dnssec-lookaside    auto;
  };
  logging {
    channel default_debug {
        file "data/named.run";
        severity dynamic;
    };
  };
  zone "." IN {
      type hint;
      file "named.ca"
  };
  include "/etc/named.rfc1912.zones";
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
- The named.conf file must have the named as the owning group and read permission at the group level.

### Analysis of the Default Zone configuration file

- The /etc/named.rfc1912.zones is the default DNS zone configuration file that points to the zone databases located in the /var/named directory. This file is defined with the include directive in the named.conf file and is processed each time the DNS server daemon is started or restarted.

- The default version of this file provides the DNS server with zone information for localhost only. For each zone statement, it specifies a zone name, its type (master for primary, slave for secondary, or forward for forwarding DNS server), its file name that stores zone data and the hostnames that are allowed to submit dynamic updates to the zone with the allow-update directive. Each statement begins with a { and ends with };. There are many other directives that can be defined in this file to customize the behaviour.

- The default version of this file is ready for use as a localhost caching nameserver. The following shoes a few statements in this file:

```
  // need output from file
  zone "localhost" IN {
      type master;
      file "named.localhost";
      allow-update { none; }
  };
  zone "1.0.0.127.in-addr.arpa" IN {
    type master;
    file "named.loopback";
    allow-upate { none; };
  };
  zone "0.in-addr.arpa" IN {
      type master;
      file "named.empty";
      allow-update { none; };
  };
```

- Each statement in this file points to a relevant zone file in the /var/named directory:

```
  // need listing from server
  ll /var/named
```

The default contents of the named.localhost zone file are shown below:

```
  $TTL 1D
  @ IN SOA @ rname.invalid. (
        0   ; serial
        1D  ; refresh
        1H  ; retry
        1W  ; expire
        3H ) ; minimum
    NS    @
    A     127.0.0.1
    AAAA  ::1
```

### DNS Message Logging

- DNS messages and alerts are logged to /var/log/messages, as are all messages related to stopping and starting the named service, zone loading and unloading, general information, warning messages, etc.

### SELinux Requirements for BIND operation

- By defalt, the named daemon runs confined in its own domain, and it is labelled appropriately with the domain type named_t. This can be confirmed with the ps command:

```
  # need output from server
  ps -eZ | grep named
```

The SELinux file type associated with the /etc/named.conf and /etc/named.rfc1912.zones files is named_conf_t and that for the zone directory /var/named is named_zone_t.

```
  // Need output from server
  ll -Zd  /etc/named.conf /etc/named.rfc1912.zones /var/named
```

The SELinux type associated with the DNS port is dns_port_t, and it is place by default. Here is the semanage command that outputs this:

```
  # needoutput from server
  semanage port -l | grep dns_
```

From a SELinux boolean standpoint, there are two booleans that are associated with BIND, and we can see this with getsebool:

```
  # need output from server
  getsebool -a | grep ^named
```

These booleans determine whether the BIND service can bind a TCP socket to HTTP ports and to write master zone files, respectively. Both booleans are turned off by default.

### Understanding and Configuring the DNS Client

- For DNS Client configuration, there are two configuration files of interest: resolv.conf and nsswitch.conf, located in the /etc directory and installed at the time of OS installation.

- The resolv.conf file is the NDS resolver configuration file where we define information to support hostname lookups. This file may be updated manually using a text editor. It is referenced by resolver utilities, such as dig, host and nslookup.

There are three directives set in this file:

- domain: Specifies the default domain name to be searched for queries. In the absence of this directive, the domain associated with the hostname is assumed, and if the hostname is without a domain name, then the root domain is considered. This directive useful in a multi-domain environment.

- nameserver: Specifies up to three DNS server IP addresses to be queried one at a time in the order in which they are listed for name resolution. If none specified, the DNS server on the localhost is assumed.

- search: Specifies up to six domain names, of which, the first must be the local domain. The resolver appends these names one at a time in the order in which they are listed to the hostname being looked up. In the absence of this directive, the local domain is assumed.

A sample entry is shown below:

```
  domain somedomain.com
  search somedomain.com
  nameserver 191.11.11.22
```

- On a system with this file absent, the resolver utilities only query the name server configured on the localhost, determine domain name from the hostname of the system, and construct the search patch from the domain name.

### The name service switch configuration file.

- The nsswitch.conf file is referenced by many system and network tools to determine the source, such as NIS, NIS+, LDAP, DNS or local files, for obtaining information about users, user aging, groups, mail aliases, hostnames, networks, protocols and so on.

- In the presence of multiple sources, the file also identifies the order in which to consult them and what to do next based on the status result we receive from the preceding source. There are four keywords, success, notfound, unavail, and try again, that affect this behaviour:

- success: Information found in the source and returrned to the requester (default action, return, do not try the next source)
- notfound: Information not found in the source (continue, try the next source).
- unavail: Source down, not responding, or service is dsiabled or not configured (continue, try the next source).
- tryagain: Source of service is busy temporarily, try again later. Continue to try the next source.

Here is an example line entry in the nsswitch.conf file, it shows two sources for name resolution: DNS (the /etc/resolv.conf file and /etc/hosts):

```
  hosts:  dns files
```

Based on the default behaviour, the search will terminate if the requested information is found in DNS. However, we can alter this behavior and instruct the lookup program to return if the requested information is not found in DNS. The modified entry will look like:

```
  hosts: dns[NOTFOUND=return] files
```

This altered entry will ignore hosts file.

### DNS Lookup Utilities

- These tools can be used to lookup hostanmes: dig, host, nslookup.


## DIG

- The dig utility is a DNS lookup utility. It queries the nameserver specified at the command line or consults the resolv.conf file to determine the nameservers to be used for lookups if no nameserver is supplied with the command.

- In case the nameservers ar enable to fulfil the query, the dig command contacts one of the root DNS servers listed in the /etc/named.ca file for directions.

- This tool may be used for DNS troubleshooting because of its flexibility and verbosity.

- To perform a lookup to get the IP address of redhat.com using the nameservers listed in the resolv.conf file:

```
  # dig redhat.com, need output from server
  ...
```

To perform a reverse lookup on the IP address, use the -x option:

```
  # dig -x 8.8.8.8 need output from server
  ...
```

## The host utility

host is a simple DNS lookup utility that works on the same principles as the dig command. This produces less output:

```
  # host redhat.com, need output from server  
  ...
```

To perform a reverse lookup, just use the IP address:

```
  host 8.8.8.8, need output from server.
  ...
```

# Chapter 11, Section 1: Configuring DNS

This chapter will cover configuring DNS (BIND) on RHEL7 systems. We will look at BIND, the SELinux requirements, the configuration and zone files, caching, and testing/troubleshooting.

### What is DNS?

DNS is a tree-like distributed structure that is employed on the Internet and corporate networks as the defacto standard for resolving hostnames to their numerical IP addresses. The protocol is both hardware and OS independent. Name lookups can be made both using IP addresses and domain names (reverse and forward lookups) if the entries are present on the domain name servers. It works based on a client/server architecture where one central server provides name resolution to many remote clients.

### What is BIND?

Bind, Berkeley Internet Name Domain is an open source implementation of DNS on Unix/Linux operating systems. RHEL7 includes BIND as the standard DNS package.

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

# Chapter 11, Section 1: Configuring DNS

This chapter will cover configuring DNS (BIND) on RHEL7 systems. We will look at BIND, the SELinux requirements, the configuration and zone files, caching, and testing/troubleshooting.

### What is DNS?

DNS is a tree-like distributed structure that is employed on the Internet and corporate networks as the defacto standard for resolving hostnames to their numerical IP addresses. The protocol is both hardware and OS independent. Name lookups can be made both using IP addresses and domain names (reverse and forward lookups) if the entries are present on the domain name servers. It works based on a client/server architecture where one central server provides name resolution to many remote clients.

### What is BIND?

Bind, Berkeley Internet Name Domain is an open source implementation of DNS on Unix. 

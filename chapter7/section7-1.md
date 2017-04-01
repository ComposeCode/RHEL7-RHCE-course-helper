# Chapter 7, Section 1: NFS

In this chapter we will cover NFS, its main concepts and benefits. We will look at the different versions of versions, the packages needed to configure NFS and the different daemons and commands used to manipulate NFS.

### The NFS Protocol (Network File System)

The NFS Protocol (Network File System Protocol) allows the sharing of files between systems over the network. The protocol allows for a directory or entire file system on one machine to be mounted and used remotely on another system as if it exists locally on that system. Users will not see a different between the network file system and another local file system.

NFS shares may be used for collaboration among group members on a remote system and can be secured using the kerberos authentication system to prevent unauthorized access and ensure data integrity.

Read and write activities can occur from both the client and the sever while the shares accessed and files are modified. These I/O activities can be monitored and used to troubleshoot performance issues.

The architecture of the protocol is based on a server/client architecture whereby users on one system access files, directories and file systems (known as shares), residing on a remote system as if they were mounted locally on their system. the remote system that makes it shares available for network access is referred to as an NFS server and the process of making the shares accessible is referred to as exporting.

The shares the NFS server exports may be accessed by one or more systems. 

# Chapter 7, Section 1: NFS

In this chapter we will cover NFS, its main concepts and benefits. We will look at the different versions of versions, the packages needed to configure NFS and the different daemons and commands used to manipulate NFS.

### The NFS Protocol (Network File System)

The NFS Protocol (Network File System Protocol) allows the sharing of files between systems over the network. The protocol allows for a directory or entire file system on one machine to be mounted and used remotely on another system as if it exists locally on that system. Users will not see a different between the network file system and another local file system.

NFS shares may be used for collaboration among group members on a remote system and can be secured using the kerberos authentication system to prevent unauthorized access and ensure data integrity.

Read and write activities can occur from both the client and the sever while the shares accessed and files are modified. These I/O activities can be monitored and used to troubleshoot performance issues.

The architecture of the protocol is based on a server/client architecture whereby users on one system access files, directories and file systems (known as shares), residing on a remote system as if they were mounted locally on their system. the remote system that makes it shares available for network access is referred to as an NFS server and the process of making the shares accessible is referred to as exporting.

The shares the NFS server exports may be accessed by one or more systems. These systems are called NFS clients and the process of making the shares accessible on clients is referred to as mounting.

```
  Need diagram to explain NFS client/server relationship
```
A system can provide both the server and client functionality concurrently. When a directory or file system share is exported, the entire directory structure beneath it becomes available for mounting on the client.
A sub-directory or the parent directory of a share cannot be re-exported if it exists in the same file system. Similarly, a mounted share cannot be exported further. A single exported file share is mounted on a directory mount point.

NFS uses the Remote Procedure Call (RPC) and eXternal Data Representation (XDR) mechanisms that allow a server and client to communicate with eahc other using a common language that they both understand. This allows the NFS serverand client to run on two different operating systems with different hardware platforms.

### Benefits of Using NFS

The use of NFS provides several benefits, such as: supports Linux/Unix and Windows, Multiple NFS clients can access a single share simultaneously, Enables the sharing of common appliction binaries and other read-only information, resulting in reduced administration overhead and storage cost, gives users access to uniform data, allows the consolidation of scattered user home directories on the NFS server and then exporting them to the client. This way users will have only one home directory to maintain.

### NFS Versions

- RHEL7 supports NFS versions 3, 4.0 and 4.1, where NFSv4 is the default.
- NFSv3 supports both TCP and UDP transport protocols, asynchronous writes, 64 bit file sizes and that gives the clients the ability to access files larger than 2GB.
- NFSv4 and NFSv4.1 are Internet Engineering Task force (IETF) standard protocols that provide all the features of NFSv3 Protocol. They also provide the ability to transit firewalls, work on the internet, enhance security, encrypted transfers, support for ACLs, greater scalability, better cross-platform interoptability, and better handling of system crashes. They use the TCP protocol by default but can work with UDP for backward compatibility. They use usernames and groupnames rather than UIDs and GIDs for files located on network shares. 

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

- NFSv4.1 is the latest NFS protocol version, and one of its attractive features is the support of pNFS (parallel NFS). This new feature greatly improves the I/O performance by allowing NFS clients parallel and direct access to the share that sits on a remote physical storage system and limiting the NFS server role to regulate metadata and manage access.

### NFS Security

- NFSv4 guarantees secure operation of NFS on WANs. When an NFS client attempts to access a remote share, an exchange of information takes place with the server to identify the client and the user on the server, authenticate them to the server and authorize their access to the share.

- In-transit data between the two entities is encrypted to prevent eavesdropping and unauthorized access. NFS may be configured to use an existing Kerberos server for authentication, integrity and data encryption. The NFS protocol uses TCP port 2049 for all communications between the server and client; hence this port must be opened in the firewall for the NFS traffic to pass through.


### NFS Daemons

 NFS is a client/server protocol that employs several daemon programs to work collaboratively in order to export and mount shares, and manage I/O between them.

 Below is a list of different daemons used by nfs:

 - nfsd: The NFS Server process that responds to client requests on TCP port 2049 for file access and operations. It also provides the file locking and recovery mechanism. If the client sees an issue with the state of a file on the share, it notifies this server process for an action.

 - rpcbind: Runs on both the server and client. It converts RPC program numbers into universal addresses to facilitate communication for other RPC-based services.

 - rpc.rquotad: Runs on both the server and client. It displays user quota information for a remotely mounted share on the server, and it allows the setup of user quotas on a mounted share on the client.

 - rpc.idmapd: Runs on both the server and the client to control the mappings of UIDs and GIDs with their corresponding usernames and groupnames based on the configuration defined in the /etc/idmapd.conf file.

### NFS Commands

There are a lot of available NFS commands available to manage and configure NFS shares and to monitor their IO.

- exportfs: Server command that exports shares listed in the /etc/exports file and the files in the /etc/exports.d directory with the .exports extension. I tis also used to display the exported shares as listed in the /var/lib/nfs/etab and /proc/fs/nfs/exports files. Some of the key switches available with this command are -r to re-export entries, -a to perform an action on all configured shares, and -u to unexport the specified share and -v to enable verbosity. This command displays the list of shares when executed without any options.

- mount: clint command that mounts a share specified at the command line or listed in the /etc/fstab file and adds an entry to the /etc/mtab file. Without any options, this command can also be used to display mounted shares as listed in the /etc/mtab fle.

- nfsiostat: Client command that provides NFS I/O statistics on mount shares by consulting the /proc/self/mountstats file.

- nfsstat: Displays NFS and RPC statistics by consulting the /proc/net/rpc/nfsd (server) and /proc/net/rpc/nfs client files, respecitvely.

- mountstats: Client command that displays per-mount statistics by consulting the /proc/self/mountstats file.

### NFS Configuration and Functional Files

NFS reads configuration data from various files at start up and during its operation:

- /etc/exports: Server file that contains share definitions for export.
- /var/lib/nfs/etab: Server file that records entries for exported shares whether or not they are remotely mounted. This file is updated each time a share is exported or unexported.
- /etc/fstab: Client file system table that contains a list of shares to be mounted at system reboots or manually with the mount command. This file also maintains a list of local file systems.
- /etc/mtab: Client file that keeps track of mounted shares, as well as the local file systems. the mount and umount commands update file.
- /etc/sysconfig/nfs: A server- and client-side NFS startup configuration file.

### The /etc/exports File and NFS Server Options

The /etc/exports file defines the configuration for NFS shares. It contains one-line entry per share to be exported. For each share, a pathname, client information and options are included.

These options govern the share access on the clients. Options must be enclosed within parentheses and there must not be any space following the hostname. If an option is specified, it will override its default setting; the other defaults will remain effective.

- * 

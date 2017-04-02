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

- * (Represents all possible matches for hostnames, IP addresses, domain names, or network addresses.)
- all_squad (no_all_squash) [no_all_squad] (treats all users, including the root user, on the client as anonymous users)
- anongid=GID [65534] Assigs this GIF explictly to anonymous groups on the client
- anonuid=UID [65534] Assigns this UID explicity to anonymous users on the client.
- async (sync) [sync] Replies to the client requests before changes made by previous requests are written to disk.
- fsid Identifies the type of share eing exported. Options are device number, root or UUID. This option applies to file system shares only.
- mp Exports only if the specified share is a file system.
- root_squash (no_root_squash) Prevents the root user on the client from gaining super access on mounted shares by mapping root to an unprivileged user account called nfsnobody with UID 65534.
- rw (ro)[ro] - Allows access only on clients using ports lower than 1024.
- sec [sec=sys] - Limits the share export to clients using one of these security methods: sys, krb5, krb5i, or krb5p. The sys option uses local UIDs and GIDs and the rest use Kerberos for user authentication, krb5 plus integrity check and krb5i plus data encryption, respectively.
- secure / insecure [secure] allows access only on clients using ports lower than 1024.
- subtree_check (no_subtree_check) [no_subtree_check] - Enables permission checks on higher-level directories of a share.
- wdelay (no_wdelay) [wdelay] - Delays data writes to a share if it expects the arrival of another write request to the same share soon thereby reducing the number of times the actual writes to share must be made.

Here is an example of an export:

/export1 client1 client2 client3.example.com(rw,insecure)
/export2 client4.example.com(rW) 192.168.1.20(no_root_suash) 192.168.0.0/24

### SELinux requirements for NFS Operation

SELinux protects systems by setting appropriate controls using contexts and booleans. By default the SELinux policy allows NFS to export shares on the network without making any changes to either the file contexts or booleans. All NFS daemons are confined by default and are labeled with appropriate domain types. For instance, the nfsd process is labeled with the kernel_t type, rpc bind is labeled with the rpcbind_t type, rpc.mountd is labeled with the nfsd_t type etc.

To get this information, run the command:

```
  #need output from machine.
  ps -eZ | egrep 'nfs|rpc'
```

Similarly, NFS configuration and functional files already have proper SELinux contexts in place and therefore, they need no modifications. For instance, the context on the /etc/exports file is:

```
 #need output from machine
 ll -Z /etc/exports
```

However, any directory or file system that you want to export on the network for sharing purposes will need to have either public_content_ro_t or public_content_rw_t SELinux type applied. This is only required if more than one file-sharing service, such as any combination of NFS and CIFS, NFS and FTP, or CIFS and FTP are used. For use of NFS alone, there is no need to change the context of the directory or file system being shared.

The SELinux policy includes numerous booleans that may be of interest from an NFS operation standpoint. These booleans need a careful review to see which ones might require a toggle for NFS to operate well. Most of these booleans relate to services, such as HTTP, KVM and FTP that want to use mounted NFS shares to store their files. There is one boolean called samba_share_nfs which is enabled in case the same directory or file system is shared via both NFS and CIFS.

```
  # need output from a machine
  getsebool -a | egrep '^nfs|^use_nfs'
```

The output lists four booleans:
  - nfs_export_all_ro (Allows/disallows share exports in read-only mode)
  - nfs_export_all_rw (Allows/disallows share exports in read/write mode)
  - nfsd_anon_write (Allows/disallows the nfsd daemon to write anonymously to public directories on clients)
  - use_nfs_home_dirs (Allows/disallows NFS clients to mount user home directories)

### NFS Client Options

Once a shared is exported, the client can then mount the share using the mount command. This command supports several options, which are described below:
- ac (noac) [ac] Specifies whether to cache file attributes for better performance.
- async (sync) [async] Causes the I/O to happen asynchronously.
- defaults Select the following default options automatically: rw, suid, dev, exec, auto, nouser, async
- exec / no exec [exec] Allows the execution of binaries.
- fg /bg [fg] Use fg (foreground) for shares that must be available to the client to boot successfully or operate properly. If a foreground mount fails, it is retried for retry minutes in the foreground until it either succeeds or times out. With background (bg), the mount attempts are tried repeatedly for retry minutes in the background without hampering the system boot process or hanging the client.
- hard / soft The hard/soft option the client tries repeatedly to mount a share until either it succeeds or times out. With the soft option, if a client attempts to mount a share for trans times unsuccessfully, an error message is displayed.
- _netdev: mounts a share only after the networking has been started.
- nfsvers - Specifies the NFS version to be used.
- remount - attempts to remount an already mounted share with perhaps, different options.
- retrans=n [3] The client retransmits read or write request for n times after the first transmission times out. If the request does not succeed after the n retransmissions have completed, a soft mount displays an error message and a hard mount continues to retry.
- retry= [2 minutes for fg or 10,000 minutes for bg] - Tries to mount a share for the specified amount of times before giving up.
- rsize=n [negotiated] - Specifies the size or each read request.
- rw / ro [rw]  - rw allows file modifications and ro prevents file modifications.
- sec=mode [sys] -  Specifies the type of security to be used. The default uses local UIDS and GIDs, additional choices are krb5, krb5i, krb5p, and they use Kerberos for user authentication, krb5 plus integrity check and krb5i plus data encryption, respectively.
- suid/nosuid [suid] - Allows users to run setuid and setgid programs.
- timeo=n [600] Sets a wait timeout, in tenths of a second, for NFS read and write requests to be responded before it retries again for retrans times. When the number of retrans attempts have been made, a soft mound displays an error message while a hard mount continues to retry.
- wsize=n [negotiated] - Specifies the size of each write request.

### Monitoring NFS Activities

- Monitoring NFS activities typically involves capturing and displaying read and write statistics on both the NFS server and NFS Client. To do this, tools such as nfsstat, nfsiostat and mountstats are available and may be used for this purpose. The details that these tools provide require an in-depth understanding of various fields and parameters that are depicted in the output. The following presents only a high-level introduction of these tools.

The nfsstat command can be run on both the NFS server and client to produce NFS and RPC I/O statistics. It can be used to display server (-s), client (-c), NFS (-n) and RPC (-r) statistics. With both the -m option, it shows all activities on mounted shares, without any options, it exhibits, both NFS and RPC statistics.

```
  # Need output from nfsstat on server
  nfsstat
```

```
  # Need output from nfsstat on client
  nfsstat
```

The nfsiostat command is an NFS client-side utility that produced read and write statistics for each mounted share by consulting the /proc/self/mountstats file. You can specify a time interval and a count of iterations for the execution of this command.

```
  # Need output of nfsiostat on the client
  nfsiostat
```

The mountstats command also consults the /proc/self/mountstats file and displays the NFS read and write statistics for the specified mount share. You can specify the --nfs or --rpc option with the command to restrict it to display NFS or RPC statistics

```
 # Need to run mountstats command on the nfs server
  mountstats /somemount
```

# Chapter 8, Section 1: Samba

This chapter will cover Samba: We will look at the Samba daemons, commands and configuration files. We will look at the configuration files and the different software packages involved. We will also look at the SELinux requirements for Samba.

### The Samba Protocol.

- Samba is a networking protocol that allows Linux and Unix Systems to share file and print resources with Windows and other Linux/Unix Systems. It is the standard Windows interoptability0 suite of programs for Linux and Unix.

- Samba offers numerous benefits, including seamless interaction with Microsoft Windows systems. RHEL7 includes support for Samba v4.1, which uses the SMB3 protocol that allows encrypted transport connections to Windows and other Linux-based Samba servers. The samba service is configured with the help of a single configuration file and a few commands.

- Server Message Block (SMB), now widely known as the Common Internet File System (CIFS) is a network protocol developed by Microsoft, IBM and Intel in the late 1980s to enable Windows-based PCs to share file and print resources with one another. This protocol has been used in Windows Operating systems as the primary native protocol for file and printer sharing.

- Samba was developed in the Linux world to share file and print resources with Microsoft Windows systems using the SMB format. This allowed Linux systems t participate in Windows workgroups and domans, as well as share Windows resources with other Linux systems and unix.

- In Samba terminology, the system that shares its files and print resources is referred to as a Samba server, and the system that accesses those shared resources is referred to as a Samba client. The network file shares may be used for collaboration among group members with accounts of different systems running a mix of Linux and Windows operating systems. Moreover, user home directories that exist on Windows may be shared with Linux systems and vice versa. This eliminates the need of having a separate home directory on each system for each user to log on. A single system can be configured to provide both Samba server and client functionality concurrently.

## Benefits of using Samba

- Samba shares can be accessed on Windows as well as Unix/Linus Systems.
- Windows shares can be accessed and mounted on Linux.
- Linux and Windows domain user credentials can be used interchangeably on either platform for authentication and authorization.
- Samba server:
  - Act as a print server for Windows systems.
  - Be configured as a Primary domain Controller (PDC) and as a Backup Domain Controller (BDC) for a Samba based PDC.
  - Be set up as an Active Directory member server on a Windows network.
  - Provide Windows Internet Name Service (WINS) name resolution.

### Samba Daemon

Samba and CIFS are client/server protocols that employ the smbd daemon on the server to share and manage directories and file systems. This daemon process uses TCP port 445 for operation and it is also responsible for share locking and user authentication.

### Samba Commands

There are numerous commands available to establish and manage samba functionality on the server and client:

- mount: Mounts a Samba share specified at the command line or listed in the /etc/fstab file. it adds an entry for the mounted share to the client's /et/cmtab file and can be used to display mounted shares listed in the file.
- mount.cifs: Mounts a Samba share on the client.
- pdbedit: Maintains a local user database in the /var/lib/samba/private/smb/passwd file on the server.
- smbclient: Connects to a samba share to perform FTP-like operations.
- smbpasswd: Changes Samba user passwords.
- testparam: Tests the syntax of the smb.conf file.
- umount: Functions opposite to that of the mount command.

### Samba Configuration and Functional Files.

Samba uses several configuraiton files at startup and during it separation, these files include those that store configuration data and logs:

- /etc/samba/smb.conf: Samba server configuration file.
- /etc/samba/smbusers: Maintains Samba and Linux user mappings.
- /etc/sysconfig/samba: Contains directives used at Samba startup. Stores Samba startup configuration.
- /var/lib/samba/private/smbpasswd: Maintains Samba user passwords. This file is usd for authentication purposes.
- /var/log/samba: Directory location for Samba logs.

- The smb.conf is the primary configuration file for setting up a samba server. You specify share definitions and set parameter vlues to modify their behavior. This file has two major sections: Global Settings and Share definitions.

- Global settings: defines the directives that affect the verall samba server behavior and includes options for networking, logging, standalone server, domain members, domain controller, browser control, name resolution, printing, and file systems.

- Share definitions sets share-specific directives for home and custom shares. Most settings in the global section are applied to all other sections in the file provided the other sections do not have them defined explicitly.

```
  # Need listing of directives form a server and a listing of each one.
```

### Samba Software Packages

The samba packages are: samba, samba-common, samba-libs, cifs-utils, samba-client.

The server side requires: samba, samba-common, samba-libs (cifs-utils is optional).

The client side requires: cifs-utils and samba-client.

### Configuring Samba Server and Client

SELinux contexts: the Samba daemon is confined by default and is labelled appropriately with smbd_t domain type. This information can be verified through:

```
  # need output from machine
  ps -eZ | grep smbd
```

Similarly, Samba configuration and functional files already have proper SELinux contexts in place; therefore, need no modifications.

- However, any directory or file system that you want to share on the network with Samba alone needs to have samba_share_t type applied to it. In case of multiple file-sharing, such as combination of CIFS, and NFS, CIFS and FTP or NFS and FTP, sharing the same directory or file system, you wil need to use either the public_content_ro_t or public_content_rw_t type instead.

- The SELinux policy includes numerous booleans that may be of interest from a Samba operation standpoint. These booleans need a careful review to see which ones might require a toggle for Samba to operate well. Most booleans related to a specific service. A special boolean, called samba_sharE_nfs is enabled in csae the same directory or file system is shared with both NFS and CIFS.

To list the Samba related booleans, run the following command to list the samba booleans:

```
  # need output from server
  getsebool -a | egrep 'samba|smb|cifs'
```

Here are some key booleans:

- ftpd_use_cifs: Allows/disallows a mounted samba share to be used as a public file transfer location by the FTP service.
- httpd_use_cifs: Allows/disallows a mounted Samba share to be used by the apache service.
- samba_create_home_dirs: Allows/disallows Samba to create home directories
- samba_enable_home_dirs: Allows/disabllows Samba to share user home directories
- samba_export_all_ro: allows/disallows Samba to share in read-only mode.
- samba_export_all_rw: Allows/disallows samba to share in read/write mode.
- smbd_anon_Write: Allows/disallows samba to write to public directories with public_content_rw_t type.
- use_samba_home_dirs: Allows/disallows Samba clients to mount user home directories.


### Exercises. 

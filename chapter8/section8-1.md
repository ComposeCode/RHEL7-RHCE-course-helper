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



### Samba Commands

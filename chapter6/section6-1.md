# Chapter 6, Section 1: Sharing Block Storage with iSCSI

### What is iSCSI ?

iSCSI is a storage networking protocol used to a share a computer's local storage with remote clients using the SCSI commandset over an existing IP network infrastructure. The clients see the shared storage as a locally attached hard disk and can use any available disk and file system management tools to partition, format and mount it. This way, unused storage on one system can be utilized by other systems without the need of physically relocating it or reorganizing cables.

## Understanding the iSCSI Protocol

The Internet Small Computer System Interface (iSCSI) is a storage networking transport protocol that carries SCSI commands over IP Networks, including the Internet. This protocol enables data transfer between iSCSI clients and iSCSI storage servers, instituting a Storage Area Network (SAN) of disparate storage. As this protocol is designed to work on IP networks, the distance between clients and storage servers is irrelevant as long as the link between them is fast and stable. Once a proper configuration is in place and a connection is established, any storage shared by a storage server appears as a locally attached hard disk to the client.

An iSCSI SAN is a low-cost alternative to the expensive Fibre Channel-based SAN (FC SAN). It does not require special or dedicated cabling, switches and host controller adapters as does the FC SAN; rather is uses existing network infrastructure for storage sharing. Dedicated infrastructure can be deployed for improved performance and is recommended to evade bandwidth issues.

The iSCSI protocol uses port 3260 with TCP by default. Unlike NFS and CIFS protocols that are using for file sharing, iSCSI presents the network storage to clients as a local raw block disk drive. The clients can carve the disk up using any disk partition software, such as parted or LVM. The shared network storage does not have to be an entire physical or virtual disk, rather it could be a simple file, an LVM logical volume, a disk partition, a RAID partition or a RAMDISK. In all cases it appears to clients as just another regular local disk.

```
  Need diagram of iSCSI Target and Initator Arrangement.
```

* Note that iSCSI Target is the storage server and the other machine is the iSCSI Client known as the initiator.

## Terminology

The iSCSI technology has several terms that need to be grasped in order to fully understand how it works and is configured. The key iSCSI Terms:

- ACL: An ACL (access control list) controls an iSCSI client access to target LUNs.

- Addressing: iSCSI assigns a unique address to each target server. It supports multiple addressing formats; however, the IQN (iSCSI qualified name) is most common. A sample IQN is shown below:
  - iqn.2015-01.com.example:testiscsilun0

- Alias: an Alias is an optional string of up to 255 characters that may be defined to a give description to an iSCSI LUN. "Database Backups, LUN 001" may be a sample description alias. An Alias can help to quickly identify a LUN.

- Authentication: Authentication allows initiators and targets to prove their identity at the time of discovery and normal access. The iSCSI protocol supports CHAP-based authentication (challenge-handshake authentication protocol) methods that use usernames and passwords but hide the network transmission of passwords. These methods are referred to as CHAP initiator authentication and mutual CHAP authentication. The former requires the initiators to prove their identity to the target by entering valid credentials (one-way authentication), and the latter requires both the initiator and the target to supply their credentials to each other to confirm their identities (two way authentication). The third option, the demo mode, is the default option and it is used to disable authentication feature and open full access for all initiators to all exported target LUNs.

- Backstore: a backstore is a local storage resource thaty serves as the backend for the LUN presented to the initator. A backstore may be entire physical or virtual disk (block), a standard partition (block), a RAID partition (block), an LVM logical volume (block), a plain file (fileio), or a ramdisk image (Ramdisk). The first four represents disk-based block devics, the fileio identifies the backstore as a plain file that is treated as a disk image, and the ramdisk image represents the kernel memory that is treated as a block device. There is another backstroe type called pscsi, however, it is recommended to use the block backstore type instead of pscsi.

- Initiator: An initiator is a client system that accesses LUNS presented by a target server. Initiators are either software or hardware-driven. A software initator is a kernel module that uses iSCSI protocol to emulate a discovered LUN as a block SCSI disk. A hardware initiator, on the other hand, uses a dedicated piece of hardware called an HBA (host bus adapter) to perform the same function. AN HBA offloads system processors by processing SCSI commands on board processors, resulting in improved system performance.

- iSNS: An iSNS (internet storage name service) is a protocol that is used by an initiator to discover shared LUNs.

- LUN: A LUN (Logical Unit Number) represents a single addressable logical SCSI disk that is exported on the target server. From an initiator perspective, a LUN is just like any other hard disk attached to it. Disk Management software, such as parted and LVM, treat both LUN and hard disk identically.

- Node: A node is a single discoverable object on the iSCSI SAN. It may represent a target server or an initiator. A node is identified by its IP address or a unique iSCSI address.

- Portal: A portal is a combination of an IP address and TCP port that a target server listens on and initiators connect to. iSCSI uses TCP port 3260 by default.

- Target: A target is a server that emulates a backstore as a LUN for use by an initiator over an iSCSI SAN.

- TPG: A TPG (target Portal Group) represents one or more network portals assigned to a target LUN for running iSCSI sessions for that LUN.

## iSCSI Software Packages

- A single software package, targetcli needs to be installed on the target server in order to provide the iSCSI target functionality. This package has a number of dependencies that are also installed with it.

- Targetcli implements the open source Linux IO (LIO) iSCSI target subsystem in the kernel to support the configuration and sharing of storage resources and their presentation as block storage to clients over IP networks. This utility is used to manage the LIO subsystem and it runs as a shell interface.

- On the client side, the iSCSI initiator functionality becomes available when the iscsi-initaitor-utils package is installed. This package brings the iscsiadm management command, the /etc/iscsi/iscsid.conf configuration file, and other relevant commands and files.

## Managing iSCSI Target Server and Initiator

- To configure the target server: a backstore must be setup and an iSCSI target must be configured on the backstore. A network portal must be assigned, a lun must be created and exported, ACL must be configured for the LUN and the configuration must be saved.

- To configure the initiator: you must first discover a target server for LUNs, logging onto discovered target LUNs, using disk management tools to partition, format and mount the LUNS.

## Understanding the targetcli Command for Target Administration

The targetcli command is an administration shell that allows you to display, create, modify and delete target LUNs. This tool is a complete iSCSI target configuration tool in RHEL7 that acts as an interface between your system and the LIO subsystem in the kernel.  It gives you the ability to present local storage resources backed by a file, whole disk, logical volume, RAID partition, standard partition or ramdisk to iSCSI clients as block storage.

The targetcli tool provides a hierarchical view (similar to the Linux directory tree) of all target LUNs configured on the target server.

Several kernel modules load in the memory to support the setup and operaiton of iSCSI LUNs on the target server. You can view the modules that are currently loaded by running the lsmod command:

```
lsmod | grep target
```

There are additional modules that are loaded when specific target types are configured. These modules include target_core_ibclock for a disk type target, target_core_file for a plain file type target and so on.  

When you invoke targetcli, a cli is started:

```
  need example of targetcli
```

```
  need example of running targetcli help command
```

Here is a list of subcommands provided by help:
- ls (Shows the downward view of the tree form the current location)
- pwd (Displays the current location in the tree)
- cd (navigates in the tree)
- exit (Quits the targetcli shell interface)
- saveconfig (Saves the modifications)
- get /set (Gets or sets configuration attributes)
- sessions (Displays details for open sessions.)

While in the targetcli shell, there are different paths that point to a single target, and each one of them includes a different set of subcommands. The available commands shell prompt change as you navigate the path. You can use the pwd subcommand to view your current location in the tree. The cd subcommand helps you navigate in the tree, and without any options, it displays the full object tree view from where you can highlight and select a desired path and get there directly.

Run the ls subcommand to list the entire object hierarchy from the root of the tree:

```
  need output of ls subcommand in targetcli
```

The tree currently shows an empty view. If you want to move to the block target path under backstores, run the following:
```
      cd /backstores/block
```

And run the following to go to the iscsi directory:
```
    cd /iscsi
```

While in any of the object directories, running the ls command show sinformation specific to that object only. For instance, run ls while in the /iscsi directory:
```
  ls
```
The targetcli command may alternatively be run directly from the command prompt.

## Understanding the iscsiadm Command for Initiator Administration

The primary tool to discover iSCSI targets, to log in to them, and to manage the iSCSI discovery database is the iSCSiadm command. This command interacts with the iscsid daemon and reads the /etc/iscsi/iscsid.conf file for configuration directives at the time of discovering and logging in to new targets.

The iscsiadm command has four modes of operation:

- Discovery: Queries the specified portal for available targets based on the configuration defined in the /etc/iscsi/iscsi.conf file. The records found are stored in discovery database files located in the /var/lib/iscsi directory.
- Node: Establishes a session with the target and creates a corresponding device files for each discovered LUN in the target.
- Session: Displays current session information
- Iface: Defines network portals

There are several options available with the iscsiadm command, some of which are described in Table 19-3:
- D (--discover), this option discovers targets using Discovery Records. If no matching record is found, a new record is created based on settings defined in the /etc/iscsi/iscsi.conf file.
- l (--login) Logs in to the specified target
- L (--loginall) Logs in to all discovered targets.
- m (--mode) Specifies one of the supported modes of operaiton: discovery, node, fw, iface and session.
- p (--portal) Specifies a target server portal
- o (--op) Specifies one of the supported modes of operation: discovery, node, fw, iface and session.
- T (--targetname) Specifies a target name.
- t (--type) Specifies a type of discovery. Sendtargets (st) is usually used. iSNS is another available type.
- u (--logout) Logs out from a target
- U (--logoutall) Logs out form all targets.

## The /etc/iscsi/iscsid.conf File

The /etc/iscsi/iscsid.conf file is the iSSI initiator configuration file that defines several options for the iscsid daemon that dictate how to handle an iSCSI initiator via the iscsiadm command.  

During an iSCSI target discovery ,the iscsiadm command references this file and creates discovery and node reccords which are stored in send_targets (or other supported discovery type) and nodes sub directories under the /var/lib/iscsi directory. The records which are saved in send_targets are used when you attempt to perform discovery on the same target server again, and the records saved in nodes are used when you attempt ot log in to the discovered targets.

The following shows the default uncommented entries form the iscsid.conf file. The grep command is used to remove commented and empty lines form the output:

```
  # need output from command
  grep -v '^#|^$' /etc/iscsi/iscsid.conf
```

## The /etc/iscsi/initiatorname.iscsi file

The /etc/iscsi/initiatorname.iscsi file stores the discovered node names along with optional aliases using the InitiatorName and InitiatorAlias directives, respectively. This file is read by the iscsid daemon on startup,, and it is used by the iscsiadm comand to determine node names and aliases. A sample entry from this file is shown below:

```
  InitiatorName=iqn.2014-09.net.example.server5:mdblun01
  InitiatorAlias="Lun for database logs"
```

This file is updated manually with discovered node names that exist in the /var/lib/iscsi/nodes directory

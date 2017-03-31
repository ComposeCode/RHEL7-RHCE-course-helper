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

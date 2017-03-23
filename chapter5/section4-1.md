# Chapter 4, Section 1: Tuning Kernel Parameters, Reporting System Usage, and Loggin Remotely

This chapter will cover tuning kernel parameters, we will look at run-time and boot-time parameters, we will look at modifying run-time and boot-time parameters, we will look at reporting on system resource usage with sysstat and dsata toolsets.

We will also look at remote system logging and setting up the loghost server and client.

### Understanding Kernel Parameters

Run time kernel parameters control the behaviour while the system is still operational. the default parameter values are typically acceptable for normal system operation; however, one or more of them must be modified in order to meet the requirements for smooth installation and proper operation of certain applications, such as database or ERP software.

RHEL7 has over a thousand runtime kernel parameters with more added as new modules and applications are installed on the system. These parameters are set automatically during kernel loading. The current list of active runtime parameters may be viewe with the sysctl command as follows:

```
[chartwell@workstation shadytrade-importer]$ sysctl -a
abi.vsyscall32 = 1
crypto.fips_enabled = 0
debug.exception-trace = 1
debug.kprobes-optimization = 1
dev.cdrom.autoclose = 1
dev.cdrom.autoeject = 0
dev.cdrom.check_media = 0
dev.cdrom.debug = 0
dev.cdrom.info = CD-ROM information, Id: cdrom.c 3.20 2003/12/17
dev.cdrom.info =
dev.cdrom.info = drive name:		sr0
dev.cdrom.info = drive speed:		32
dev.cdrom.info = drive # of slots:	1
dev.cdrom.info = Can close tray:		1
dev.cdrom.info = Can open tray:		1
dev.cdrom.info = Can lock tray:		1
dev.cdrom.info = Can change speed:
 ... output omitted.
```

Runtime values fore these parameters are stored in various files located under sub-directories in the /proc/sys directory and can be altered on the fly by changing associated files. However, these changes are not permanent and only remain changed until the system is rebooted or the values are adjusted again. You can also make these changes using the sysctl or echo commands. In order to make changes which will survive a reboot, you need to modify the values in the /etc/sysctl.conf file or in a file under the /etc/sysctl.d directory.

By default, the sysctl.conf only contains a few commands. There is another file, /usr/lib/sysctl.d/00-system.cnf, that stores system default settings which can be used in case anything goes wrong.

### Boot-Time Parameters

Boot time parameters also known as command-line options, afect the boot behavior of the kernel. Their purpose is to pass any hardware-specific information that the kernel would otherwise not be able to determine automatically, or to override random values that the kernel would detect by itself. Boot-time parameters are supplied to the kernel via the GRUB2 interface.

The entire boot string and command line options can be viewed after the system has been booted up and it is in operational state. The information is gathered and stored in the /proc/cmdline file and can be viewed with the cat command as follows:

```
  # output command line options
  cat /proc/cmdline

  cat /proc/cmdline
  BOOT_IMAGE=/vmlinuz-4.7.2-201.fc24.x86_64 root=/dev/mapper/fedora-root ro rd.lvm.lv=fedora/root rd.lvm.lv=fedora/swap rhgb quiet LANG=en_GB.UTF-8
```

The output shows the kernel name, version, UUID of the root file system and several other boot-time parameters that were passed to the kernel by default at the last system boot. This information is stored in the /boot/grub2/grub.cfg file on x86 systems.

We can modify the boot time parameters in two ways, either we modify the grub.cfg file and add the required parameters to the kernel boot string or we modify them at boot time by interrupting GRUB. The first option is permanent, the second is not.

### Generating System Usage Reports

There are several tools available for reporting system usage. RHEL comprises of sysstat and dstat software packages that include tools to monitor the performance and usage of these resources and generate reports. Other tools such as df (for viewing disk), vmstat (for viewing virtual memory stats), top for viewing realtime CPU, memory, swap and processes and so on.

## The sysstat toolset

The sysstat toolset includes several additional monitoring and performance reporting commands such as cifsiostat, iostat, mpstat, nfsiostat, pidstat, sadf and sar. A description of each tool is given below:

- cifsiostat: reports read and write operations on CIFS file systems
- iostat: Reports CPU, device and partition statistics.
- mpstat: Reports activities for each available CPU.
- nfsiostat: Reports read and write operations on NFS file systems.
- pidstat: Reports statistics for running
- sa1: Captures and stores binary data in sadd (system activity daily data) files located in the /var/log/sa directory.
- sa2: Captures and stores daily reports in sardd (system activity reporter daily data located in the /var/log/sa directory)
- sadc:
- sadf:
- sar:
- dstat

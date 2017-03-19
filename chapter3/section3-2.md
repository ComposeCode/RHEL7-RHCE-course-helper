# Chapter 3, Section 2: Configuring NTP

There is only one package we need for NTP, called ntp and it includes all the necessary support to configure the system as an NTP server, peer or client. This package is included with RHEL7. Another package called ntpdate may also be installed to get access to a command that is used to update the system with an NTP server without the involvement of the ntpd daemon.

```
  # install the packages
  yum install -y ntp ntpdate
```

These packages bring several administration commands, listed below:

- ntpdate: Updates the system date and time immediately. This command is deprecated and will bee removed from a future RHEL release. Use ntp -q instead.
- ntpq: Queries the NTP daemon.
- ntpstat: Shows time synchronization status
- ntpd: the NTP daemon program that must run on a system to use it as a server, peer or cient.  


## The NTP Configuration File

The key configuration file is /etc/ntp.conf. This file can be modified by hand, and the directives in it can be set based on the role the system is to play. Some key directives from this file are:

```
driftfile /var/lib/ntp/drift
logfile /var/log/ntp.log
restrict default nomodify notrap nopeer noquery
server 0.rhel.pool.ntp.org iburst
server 1.rhel.pool.ntp.org iburst
server 2.rhel.pool.ntp.org iburst
server 3.rhel.pool.ntp.org iburst
peer
broadcast 192.168.1.255 autokey
broadcastclient
broadcast 224.0.1.1 autokey
multicastclient 224.0.1.1
manycastserver 239.255.254.254
manycastclient 239.255.254.254 autokey
crypto
includefile /etc/ntp/crypto/pw
keys  /etc/ntp/keys
```

Here is a description of these directives:

- driftfile: Specifies the location of the driftfile (default is /var/libntp/drift). This file is used by the ntpd daemon to keep track of local system clock accuracy.

- logfile: Specifies the location of the log file.

- restrict: Sets access control restrictions on inbound NTP queries. Several defaults are defined with this directive including nomodify, notrap, nopeer and noquery. The no modify option disallows any modification attempts by other NTP servers ; the notrap option disables control messages from being captured; the nopeer option prevents remote servers from establishing peer relationship; and the noquery option disallows remote ntpq queries but answers time queries.  The second restrict directive allows time requests from systems on the 192.168.1.0/24 network; however, it allows modification attempts and does not capture control messages.

- server: Specifies the hostname or IP address of the imte server. By default the file contains four public timeserver entries. The iburst option helps improve the time taken for initial synchronization. The server directive with IP 128.128.1.0 specifies to use the local system clock as the provider of time.

- peer: specifies the hostname or IP address of the peer.

- broadcast: specifies the hostname or IP address of the broadcasting timeserver. The autokey option describes the type of authentication to use. This option is preferred in an environment with a large number of NTP clients.

- broadcastclient: the presence of this directive sets the system as a broadcastclient.

- multicastclient: Enables reception of multicast server messages to the specified multicast group address.

- manycastserver: specifies the hostname or IP address of the manycast timeserver.

- manycastclient: sets the system as a manycast client. The autokey option describes the type of authentication to use.

- crypto: Enables public-key or symmetric-key authentication.

- includefile: This file stores the password to be used to decrypt encrypted files that contain private keys.

- Keys: this file stores authentication keys.

## The system-config-date tool
The NTP client service can be setup using the graphical system-config-date tool. This tool is not installed by default. To install the tool run the following command:

```
  yum -y install system-config-date
```

## Update the System Clock Manually
You can run the ntpdate command anytime to instantly bring the system clock close to the time on an ntp server. The NTP service must not be running in order for this command to work. Run ntpdate manually and specify either the hostname or the ip address of the remote timeserver to immediately sync your system time.  

```
  need example of ntpdate
```
### Querying NTP Servers

The ntpq command can be used to send out requests to NTP servers, receive their responses and print the output. This command can also be run in interactive mode.

```
  ntpq -p

  need output
```

This command produces 10 columns:

- remote: This column shows the IP addresses or hostnames of NTP servers and peers. Each IP/hostname may be preceded by one of the following characters: * indicates the current source of synchronization, # indicates the server selected for synchronziation, but distance exeeeds the maximum. o Displays the server selected for synchronization. + Indicates the system considered for synchronization. x Designated false ticker by the intersection algorithm. . Indicates the systems picked up from the end of the candidate list. - Indicates the system not considered for synchronization. Blank Indicates the server rejected because of high stratum level or failed sanity checks.
- refid: Shows a reference ID for each timeserver.
- st: Displays a stratum level. The Stratum level 16 indicates an invalid level.
- t: Shows availale types: l = local (such as a GPS clock), u = unicast, m = multicast, b = broadcast, and - = netaddr (usually 0).
- when: Displays time, in seconds when a response was last received from the server.
- poll: Shows a polling interval. Default is 64 seconds.
- reach: expresses the number of successful attempts to reach the server. The value 001 indicates that the most recent probe was answered, 357 indicates that one probe was unanswered and the value 377 indicates that all recent probes were answered.
- delay: Indicates a length of time in miliseconds, it took for the reply packet to return in response to a query sent to the server.
- offset: Shows a time difference, in milliseconds, between server and client clocks.
- jitter: Displays a variation of offset measurement between samples. This is an error-bound estimate.

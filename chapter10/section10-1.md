# Chapter 4, Section 1: Sending and Receiving Email (PostFix)

This chapter will cover

Mail User Agent (MUA): A mail user agent is a program used to compose messages and submit them to an outgoing MTA. On the receiving side, an MUA pulls the message into the inbox of the user and allows them to read. An MUA uses either POP or IMAP protocols for mail retrieval. Some common MUAS are mail, mailx, mutt, evolution, pine, elm, thunderbird and Outlook.

Mail Submission Agent (MSA): A mail submission agent is responsible for accepting new mail messages from an MUA. The MSA function may be integrated within an MUA or MTA, or it may be a separate program. It uses TCP port 587; however, it normally works on the SMTP port.

Mail Transport Agent (MTA): A mail transport agent is responsible for transporting a message from a

Mail Delivery Agent (MDA):
Post Office Protocol (POP):
Internet Message Access Protocol (IMAP):
Smart Host (Relay):
Mail Queue:
Mailbox:

```
  need example diagram of these and how they work together
```


### Postfix

Postfix is a sendmail compatible but easier to configure and administrator. It is more modular, secure and offers better performance and is the default MTA in HREL7.  Postfix uses the SMTP protocol over port 25 for mail transfers. By default, Postifx, out of the box, is ready to accept mail from local system users.

## Postfix daemons

The primary service daemon for postfix is the master daemon located in the /usr/libexec/postfix directory. This daemon process starts several agent processes, such as nqmgr, pickup, smtpd, on demand to carry out tasks on its behalf.

The network queue manager agent, nqmgr, is responsible for mail transmission, relay and local delivery, and the pickup agent transfers mail messages from the /var/spool/postfix directory to the /var/spool/mail directory in collaboration with the smtpd agent.

The agent processes terminate after servicing clients or being inactive for a pre-determined period of time. The master and all agent processes work collaboratively to manage entire Postfix email systems.

### Postfix commands:

- alternatives: displays and sets the default MTA.
- mail/mailx: Sends and receives email.
- postalias/newaliases: processes the aliases database, which is /etc/aliases  by default.
- postconf: displays and modifies the postfix configuration stored in main.cf file
- postfix: controls the operation of the postfix service, including starting, stopping, health checking and reloading the configuration
- postmap:  processes and converts some configuration files into postfix-compatible databases
- postquery/mailq: Lists and controls Postifx Queue

### Postfix Configuration Files:

Postfix configuration files are located in /etc/postfix directory, with the exception of the aliases file, which is stored in /etc.

```
  Need an output of the configuration files.
```

## The access file

The access file may be used to establish access control based on email addresses, hosts domains or network addresses for Postfix use. The controls are placed in the form "pattern action". Each pattern has an associated action, such as OK or REJECT. The following shows some examples.

To allow access to the specified IP address:

  192.168.122.20 OK

To allow access to systems in the example.org domain:

  example.com OK

To reject access to systems on the 192.168.3 network:

  192.168.3 REJECT

The contents of this file are converted into an indexed database called access.db for Postifx use. After making changes to the access file, run the postmap command on the file to create or update the database:

```
show example of converting file into postfix database.
```

You can also use the firewall for host, domain and network access restrictions/control.  

## The canonical file

The optional file is used to establish mapping for local and non-local mail addresses. The mapping can be done on incoming mail from other users, domains and networks and it is configured in the form "pattern result".

To forward mail sent to a local user user1 to user1@yahoo.com:

user1 user1@yahoo.com

## The generic File
## The master.cf File
## The relocated File
## The transport File
## The virtual File
## The /etc/aliases File

### Postfix Logging

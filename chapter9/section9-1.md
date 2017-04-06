# Chapter 9, Section 1: Apache

This chapter covers hosting websites with Apache (httpd). We will cover the packages required, we will examine the configuration files required, we will look at CGI scripts, SSL/TLS for Apache.

### What is Apache?

Apache is a web server, used for hosting websites. It uses HTTP/HTTPS to transfer and exchange information with remote clients. RHEL7 includes the support for the version 2.4.6 of Apache.

### Apache Features:

- support for hosting multiple websites with different names on a single system.
- Access Control at the host, user and group levels.
- Dynamic loading of modules when needed.
- Integration of common configuration settings as the default in the source code for easier management.
- Support for various authentication methods, including password authentication and digital certificate authentication.
- support for compression methods for faster transfers and lower bandwidth utilization.
- Support for intrusion detection and prevention.
- Works over SSL (Secure Sockets Layer) and TLS (Transport Layer Security) protocols to provide secure web service.
- Support for separate configuration files and separate services stored in separate locations.
- Support for proxy load balancing.

### Apache Daemon

- The HTTP Server is a client/server application that relies on the httpd daemon running on the server to allow the transfer of web content. This daemon process uses TCP port 80 for non-secure and TCP port 443 for secure operations, by default.

- The software can be configured to listen on any other port number as long as it is available and appropriate permissions are enabled in firewalld and SELinux.

### Apache Commands

- There are a few commands available to control the apache daemon and perform certain configuration tasks on the web server:

- apachectl: Starts, stops, restarts and checks the status of, the httpd process (the systemctl command may be used instead).
- htpasswd: Creates and updates files to store usernames and passwords for basic authentication of Apache users. Specify the -c option to create a file and -m to use MD5 encryption for passwords.
- httpd: server program for the Apache web service. With the -t option, it checks for any syntax errors in the apache web configuration files.

### Apache Configuration files:

By default, all apache web server configuration supporting files are stored under the /etc/httpd directory. The primary configuration file, httpd.conf is under the conf sub-directory. Additional configuration files are under conf.d, and the configuration files that load modules are placed under the conf.modules.d sub-directory. An ll on the /etc/httpd directory produces the following output:

```
  # need output from server
  ll /ec/httpd
```

the output also indicates the presence of three directories: /var/log/httpd, /usr/lib64/httpd/modules, and /run/httpd. These directories store Apache log files, additional modules, and runtime information for apache.

- At start, module files are processed first to load necessary modules, followed by the httpd.conf file and then any additional configuration files from the /etc/httpd/conf.d directory.

## Analysis of the httpd.conf file

The httpd.conf file contains numerous directives that can be set as per requirements. Here are the general directives that affect the overall operation of the Apache Web Server:

- Group: Default is apache, Specifies the owning group for the httpd daemon.
- Include: Default is the conf.modules.d/*.conf with respect to ServerRoot. Specifies the locaiton of module configuration files to be loaded at Apache startup.
- Listen: Default is 80. Specifies a port number to listen for client requests. Specify an IP address and a port if you wish to limit the web server access to a specific address.
- ServerRoot: Default is /etc/httpd. Directory location to store configuration, error and log files.
- User: Default is apache. Specifies the owner for the httpd daemon.

There are several directives defined under "Main server configuration" in the file. These directives set up the default web server, which responds to client requests that are not handled by virtual hosts. The values of these directives are also valid for any configured virtual hosts, unless they are overridden:

- AddHandler: Maps a file extension to the specified handler.
- AccessedFileName: default is .htaccess, specifies the file to be used for access control information. See AllowOverride and Require directives.
- Alias: Defines a directory ocation to store files outside of DocumentRoot
- AllowOverride: Default is none. Defines types of directives that can be defined in AccessFileName files. These directives are used to control user or group accesss to private directories, as well as to control host access. some other common options are: All (allows the use of all AccessfileName-supported directives), AuthConfig (allows the use of authorization directives, such as AuthName, AuthType, AuthUserFile, AuthGroupFile, and require in AccessFileName).
- CustomLog: Defaut is combined and stored in logs/httpd/acess_log with respect to ServerRoot. Specifies the custom log file and identifies its format.
- DirectoryIndex: Default is inde.html. Specifies the web page to be served when a client requests an index of a directory.
- DocumentRoot: Default is /var/www/html. Specifies the directory location for website files.
- ErrorLog: Default is logs/error_log with respect to ServerRoot. Specifies the location to log error messages.
- IncludeOptional: Default is conf.d/*.conf with respect to ServerRoot. Specifies the location of additional configuration files to be processed at Apache startup.
- LogFormat: sets the format for logging messages.
- LogLevel: Default is warn. Specifies the log error level (debug, notice, error, crit, info, etc)
- Options: Default is FollowSymLinks. Sets features associated with web directories. Some common features are: ExecCGI (allows the execution of CGI scripts), FollowSymLinks (allows directories external to DocumentRoot to have symlinks), Indexes (Displays a list of files on the web server if no index.html is present), Includes (allows server-side includes), multiviews (allows substitution of file extensions), all (allows all options besides multiviews), None (disables all extra features).

## Main Server Directives

A There are four main contains in the httpd.conf file:
- <Directory></Directory>
- <Files></Files>
- <IfModule></IfModule>
- <VirtualHost></VirtualHost>

## Apache Log Files

- Apache Log files are located in /var/log/httpd directory, which is symbolically linked from /etc/httpd/logs.

```
  # Need server output for log files.
```

## Apache Software Packages

- The apache software packages are: httpd, httpd-tools.

## Access Control

## Controlling Access for Users and Groups:

- Limiting access to private directories is done through the .htaccess file or directly in the directory container in the httpd.conf file.

Users can be assigned passwords that may be different to their RHEL passwords, the key directives to control access at user and group levels are described below:
- AuthType: Sets basic authentication.
- AuthhName: adds general comments.
- AuthBasicProvider Default is file. Specifies the type of authentication to be used.
- AuthUserFile: specifies the file that contains authorized user passwords.
- AuthGroupFile: specifies the file that contains authorized gorup passwords.
- Require: see below.

The apache web server can be configured to limit access from specific hosts, network or domains. The require directive is used to specify these options:
- Require user: <username or UID> access is granted to the specified user only (used to grant access to private directories).
- Require not user <username or UID> access is denied to the specified user.
- Require Group <group name or GID> access is granted to the specified group members only (used to grant access to group-managed contents)
- Require not group <group name or GID> access is denied to members of the specified group.
- Require valid-user Access is granted to all valid users.
- Require ip 192.168.0. 15.2 Access is granted from 192.168.0 and 15.2 networks only.
- Require not ip 192.168.0 15.2 Access is denied from 192.168.0 and 15.2 networks.
- Require host server2 Access is granted from server2.
- Require not host server2 Access is denied from server2.
- Require host example.com Access is granted from example.com domain.
- Require not host .example.com Access is denied from example.com domain.
- Require all granted Access is granted from everywhere.
- Require all denied Access is denied from everywhere.

An example below:

This example allows user1, user2 and dba gorup members to access the contents of /var/www/example with no password authentication required:

<Directory /var/www/example>
  AllowOverride None
  Require user user1 user2
  Require group dba
</Directory>

Example 2:

To allow user1, user2 and dba group members to access the contents of /var/www/example from domain example.net, network 192.168.0 and host server2.eample.com and disallow access from domain example.org. No password authentication is required.

<Directory /var/www/example>
  AllowOverride None
  Require user user1 user2
  Require group dba
  Require host example.net server2.example.com
  Require ip 192.168.0
  Require not host example.org
</Directory>

Example 3:

To allow user1, user2 and dba group members to access the contents of /var/www/example from domain example.net, network 192.168.0 and host server2.example.com and disallow access from domain example.org. Both users and group members must enter their passwords to access the following directory contents:

<Directory /var/www/example>
  AllowOverride AuthConfig
  AccessFileName conf/.htaccess
</Directory>

The .htaccess file:

  AuthType  Basic
  AuthName  "This site is password protected."
  AuthBasicProvider file
  AuthUserFile  /etc/httpd/conf/.userdb
  AuthGroupFile /etc/httpd/conf/.groupdb
  Require user user1 user2
  Require group dba
  Require host example.net server2.example.com
  Require ip 192.168.0
  Require not host example.org

## Configuring Apache Servers

SELinux requirements for Apache Operation:

- httpd_anon_write: Allows/disallows Apache to write to directories labeled with the public_content_rw_t type, such as public directories.
- httpd_sys_script_anon_write: Allows/disallows Apache scripts to write to directories labeled with the public_content_rw_t type, such as public directories
- httpd_enable_cgi: Enables/disables execution of CGI scripts labeled with the httpd_sys_script_exec_t type.
- httpd_enable_ftp_server: Allows/disallows Apache to act as a FTP server and listen on port 21.
- httpd_enable_homedirs: Enables/disables Apache's access to user home directories.
- httpd_use_cifs: Allows/disallows Apache to use moutned Samba shares with cifs_t type.
- httpd_use_nfs: Allows/disallows Apache to use mounted NFS shares with nfs_t type.

The proper SELinux contexts must be set on apache files and directories, this is mandatory. There are three key directories: /etc/httpd, /var/www and /var/log/httpd.

```
  # need to run commands on server
  ll -Zd /etc/httpd
  ll -Zd /var/www
  ll -Zd /var/log/httpd
```

### Understanding and Configuring Apache Virtual Hosts

- Apache allows us to run multiple virtual hosts on a single system for shared hosting of several distinct websites. This technique offers a low-cost hosting solution for customers. Each hosted website can either share a common IP address or be configured with a unique IP. Both mechanisms direct the inbound web traffic to the appropriate virtual host.

## Virtual Host Configuration File

- The primary configuration file for defining virtual hosts is httpd.conf, however it is better to have separate files in /etc/httpd/conf.d directory instead.

- Common directive used within a virtual host container are ServerAdmin, ServerName, DocumentRoot, ErrorLog, CustomerLog.

Here is an example container:

```
  <VirtualHost *:80>
    DocumentRoot /var/www/html/somehost.com
    ServerAdmin admin@somehost.com
    ServerName somehost.com
    ErrorLog logs/somehost.com-error_log
    CustomLog logs/somehost.com-access_log common
  </VirtualHost>
```

These files look very similar to the previous examples above. Virtual host configuration files can be checked for syntax errors with the httpd command as follows:

```
  # Need output from server
  httpd -D DUMP_VHOSTS
```

### Understanding and Configuring Apache web Servers over SSL/TLS

- Apache web Servers can be configured to use SSL/TLS. These use digital identify certificates in order to prove its authenticity to clients when they attempt to form a connection and establish a two-way encrypted communication channel for exchange of data. A trusted digital identify certificate is signed and issued by a certificate authority (CA). To obtain one, the applicant generates a private/public encryption key pair and a certificate signing request (CSR) for the desired sever. The CSR contains applicant identity, such as company details and contact information, as well as the public key and the hostname of the system. The CSR is encoded before it is transmitted to the CA. The CA reviews the CSR and issues and issues a signed certificate after validating the data provided in the CSR.

- The alternative is to use self-singed certificates, which are created on the local system.

### HTTPS/SSL Software Packages

- There are two software packages required to use HTTPs with Apache, these are: mod_ssl and openssl.

- The installation of mod_ssl installs the ssl.conf file in /etc/httpd/conf.d, which is the config file for setting up a secure Apache server. The openssl package loads the openssl command and a diectory tre with some templates under /etc/pki.

```
  # need output from server
  yum install mod_ssl openssl openssl-libs -y
```

### The OpenSSL Toolkit

- The openssl toolkit offers a variety of subcommands to create and manage encryption keys, CSRs and digital certificates. Test HTTPS server and client connections etc. The openssl package offers an interactive mode with a prompt allowing us to subcommands directly from the prompt. There are 110 subcommands which are divided into three sets: standard, cipher (encoding and ecnryption) and message digest (detection of and protection against data corruption).

```
  # openssl list-standard-commands
  # openssl list-cipher-commands
  # openssl list-message-digest-commands
```

### The OpenSSL Configuration File

By default, the SSL configuration file, ssl.conf is stored in the /etc/httpd/conf.d directory. This file is processed after the httpd.conf file completes its processing at Apache Service start up or reload.

This file sets directives necessary to run secure web servers. It is divided into two sections: SSL Global Context and SSL Virtual Host Context. One directive which sets the listen port to 443 is at the top of the file:

```
  # Need SSL VirtualHost listening from server
```

The start of the directive indicates an IP address (IP address or *). The next five directives: DocumentRoot, ServerName, ErrorLog, TransferLog, LevelLog and CustomLog directive before the end of the file, have the same meaning as those used in httpd.conf. The next three directives, SSLProtocol, SSLCertificateFile and SSLCertificateKeyFile, specify the SSL version to use, the location of the SSL certificate and the location of the SSL Key.

The Files and Directory sub-containers specify the file types containing dynamic contents and their location.

### OpenSSL Log Files

The OpenSSL Log files are located in the /var/log/httpd directory, which is sumbolically linked from /etc/httpd/logs directory. The ssl_access_log, ssL_ERROR_LOG and ssl_request_log.

```
  # needoutput from server
  ll /var/log/httpd
```

- It is recomended to use separate log files for different websites.

### Overview of CGI and CGI Scripts

- Apache allows us to run dynamic websites. For the RHCE, we will enable and use CGI Scripts (common gateway interface).

- CGI Scripts may be written in Perl, Ruby, Python, C, shell or other programming languages.

```
  # need example of simple CGI scripts. 
```

### Exercises.

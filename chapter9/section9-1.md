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
  AuthGroupFile
  Require
  Require
  Require
  Require
  Require

### Exercises.

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

## Apache Log Files
##

### Exercises.

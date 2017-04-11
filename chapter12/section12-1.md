# Chapter 11, Section 1: MariaDB

This chapter will cover relational databases, specifically, MariaDB packages, the daemon, commands and configuration. We will also look at the SELinux requirements for MariaDB and how to interact with MariaDB using the shell interface.

### Understanding Databases, DBMS and MariaDB

- MariaDB is an open source relational database. A database is a structured repository of data that is accessed and administered by management software, such as MariaDB. Databases created with MariaDB use the relational model to identify the database structure and data storage, organization and handling. A relational database uses a table-based format.

- A database is a structure collection of data, which is comprised of facts and figures of something and can be processed to generate meaningful results.

- Need more background info on databases.

### What is a relational database?

... need more info

### What is MariaDB?

- Maria DB Packages and daemons:

```
  yum install -y mariadb mariadb-server Mariadb-libs
```

- mariadb: Provides the mariadb client programs and a configuration file
- mariadb-server: contains mariadb server, tools and configuration, log files.
- mariadb-libs: comprises of essential library files for MariaDB client programs

- The mariadb daemon is known as the mysqld daemon and listens on port 3306 by default. It supports both TCP and UDP protocols for operation.

- MariaDB offers several commands for administation and query: mysql (command line shell interface for mysql), mysql_secure_installation (improves the security of the mariadb installation), mysqldump (backs up or restores one or more tables or databases).

## MariaDB Configuration files

The primary configuration file for MariaDB is /etc/my.conf, which sets the global defaults for mysql shell program, mysqld_safe startup script and the mysqld daemon process. The uncommented line entries from this file are presented below:

```
  # grep -v ^# /etc/my.cnf
  # need output from server
```

- The my.cnf file contains two configuration groups by default. These groups are mysqld and mysqld_safe with some settings under each one of them. Their purpose is to separate the configuration eneded by the mysqld daemon, mysqld_safe startup progrram and mysql client program at startup.

- The directives in this file set several defaults, including the locations to store database files, log files and the PID file.

- The includedir directive at the bottom of the file instructs the startup program to look for additional configuration files in the /et/cmy.cnf.d directory and process tem if they exist. By default, there are three configuration files in this directory:

```
  # need output from server:
  ll /et/cmy.cnf.d
```

- These files set configurations for general clients, specific MariaDB client tools and MariaDB server program, mysqld, respectively. The default files have several configuration groups defined, but they all are empty. 

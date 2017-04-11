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

- 
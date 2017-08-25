# MySQL Server Role

Documentation for the [mysql-server role](/roles/mysql-server).

This role will install and configure the `mysql-server` and 
`python3-mysqldb` packages.

## Variables

`server_version`: The version of `mysql-server` to install.
`python_mysql_version`: The version of `python3-mysqldb` to install.
`root_password`: Password to set for the mysql root user.
`user`: Name for the mysql user to create.
`password`: Password to set for the created mysql user.
`database`: Name for the database to create.

Example:

```
---
mysql:
  server_version: 5.7.*
  python_mysql_version: 1.3.*
  root_password: password
  user: dbuser
  password: password
  database: symfony
```

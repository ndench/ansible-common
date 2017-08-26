# PHP Role

Documentation for the [php role](/roles/php).

This role will install `php7.1` and a bunch of `php7.1-*` modules,
as well as the `acl` package. Optionally create one or more 
directories for uploaded files.

## Variables:

`php_version`: The version of `php` to install (will only work with 7.1.*)

`acl_version`: The version of `acl` to install.

`upload_dirs`: A list of directories to create with `www-data` write
               permissions.

Example:

```
---
php:
  php_vesion: 7.1.*
  acl_vesnion: 2.2.*
  upload_dirs:
    - /var/uploads
```

# Common ansible roles

Ansible roles that can be used by many projects.


## Configuring ansible

In the root of your project, create an `ansible.cfg` file:

```
[defaults]
roles_path = <the path to this repo>/roles
```

I recommend that you clone this repo into the same place as
your other projects, because then you can use a relative 
`roles_path`. Otherwise everyone working on your project will need
to have a different `ansible.cfg`.

```
[defaults]
roles_path = ../ansible-common/roles
```


## Setting variables

These roles depend on quite a few variables being set. To set them
you need to create a `group_vars` directory that contains a `.yml`
file for each group listed in your inventory file.

For example, say you have an inventory file that looks like this:

```
[prod]
prod1.examle.com
prod2.examle.com

[test]
test.example.com
```

You would then create group_vars files for each group:

```
group_vars/
  prod.yml
  test.yml
```

Then you can specify the required variables for each group of hosts.
To find the required variables check out the [docs folder](docs)


## Using the roles

You can then use the roles just like you would if they were stored
with your `playbook.yml`:

```
---
- hosts: all
  roles:
    - common
    - php
...
```


## Best Practices

This is the way I set up my projects that use ansible.

### Folder structure

I create a folder structure like this:
```
ansible.cfg
Vagrantfile
provisioning/
  group_vars/
    all.yml
    dev.yml
    prod.yml
    # other group vars file
  playbook.yml
  production # production inventory file
```

### Ansible.cfg

Then my `ansible.cfg`:
 
```
[defaults]
hostfile = provisioning/production
hash_behaviour = merge
roles_path = ../ansible-common/roles
```

The `hostfile` section points to my production inventory file, so
that I don't need to specify it on the command line when running 
`ansible-playbook`.

The `hash_behaviour` setting allows me to specify common varibles
in the `all.yml` file, then environment specific variables in 
`prod.yml` and `dev.yml` and any other group vars files (see 
[group vars](#group-vars) further down for an example).

### Vagrantfile

My Vagrantfile contains a section like this:

```
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "provisioning/playbook.yml"
    ansible.groups = {
        "dev" => ["default"]
    }
    ansible.verbose = "v"
  end
```

Which tells vagrant where to find the `playbook.yml` file, which
group it should be in (see [playbook.yml](#playbookyml) further down),
and to run ansible with `-v` because it gives better output.

### Group vars

Let's say we're setting up nginx, I'll have the common nginx
variables set in `all.yml`:

```
---
nginx:
  webroot: /srv/www
  server_name: localhost
  version: 1.10.*
  sendfile_dirs:
    - location: /uploads/
      alias: /var/uploads/
```

Then dev specific variables in `dev.yml`:

```
---
nginx:
  enable_ssl: false
```

and prod specific variables in `prod.yml`:

```
nginx:
  enable_ssl: true
```

Because of the `hash_behaviour` setting in `ansible.cfg`, the nginx
varibles will be merged with `all.yml` for each host. The default
behaviour is for variables in `all.yml` to be ignored in favour
of the one is the host specific yml file.

### playbook.yml

My `playbook.yml` file:

```
---
- hosts: dev
  roles:
    - common
    - php
    - nginx
    - mailhog
    - xdebug
    - faxes3
    - mysql-server

- hosts: prod
  roles:
    - common
    - php
    - redis
    - deploy-user
    - mysql-client
    - certbot
```
### Inventory file

My `production` inventory file looks like:

```
[prod]
web ansible_host=1.2.3.4 ansbile_user=ubuntu ansible_ssh_private_key_file=$HOME/.ssh/prod.pem
```

I like to put the user and private key file in here because
they often differ from machine to machine, as long as that user
has `sudo` access, everything should work.

I also give the host an alias (in this case `web`) which makes
it easier to include in other groups. A more complex example would be:

```
[prod-web]
web ansible_host=1.2.3.4 ansbile_user=ubuntu ansible_ssh_private_key_file=$HOME/.ssh/web.pem

[prod-db]
db ansible_host=1.2.3.5 ansbile_user=ubuntu ansible_ssh_private_key_file=$HOME/.ssh/db.pem

[prod-all]
web
db
```

This allows me to specify variables in `prod-all.yml` that will
be shared across both `web` and `db`, and I can put shared
roles in the `hosts: prod-all` section of my `playbook.yml'.`

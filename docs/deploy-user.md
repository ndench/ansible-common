# Deploy User Role

Documentation for the [deploy-user role](/roles/deploy-user).

This role will create a user called `deploy` which has write access
to the `/srv/www`. The deploy user can be used with CI/CD tools for
auto deployment.

## Variables

`deploy_key`: A ssh private key file, which should have access to 
              you GitHub repo, so that it can do deploys.

`authorized_keys`: A list of ssh public keys to put against the 
                   `deploy` user to allow others to ssh in.

Example:

```
---
deploy:
  deploy_key: |
    -----BEGIN RSA PRIVATE KEY-----
    ...
    -----END RSA PRIVATE KEY-----
  authorized_keys: |
    ssh-rsa ...
    ssh-rsa ...
```

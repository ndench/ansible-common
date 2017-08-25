# Certbot Role

Documentation for the [certbot role](/roles/certbot).

This role will install certbot and acquire ssl certificatets from
[Let's Encrypt](https://letsencrypt.org/).

## Varibles

`email`: an email address that will be registered against the ssl certs.
`domains`: a comma separated list of domains to get ssl certs for.

Example:

```
---
certbot:
  email: me@example.com
  domains: example.com,www.example.com,blog.example.com
```

## Dependencies

Certbot depends on the [nginx role](/docs/nginx.md), which it will
automatically pull in because of it's [metadata](meta/main.yml).

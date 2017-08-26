# FakeS3 Role

Documentation for the [fakes3 role](/roles/fakes3).

This role will install, setup a service and create a bucket for 
[FakeS3](https://github.com/jubos/fake-s3), to allow development
againt the AWS S3 API.

## Variables

`bucket`: Name of the bucket to create.

Example:

```
---
fakes3:
  bucket: dev_local
```

## Dependencies

FakeS3 depends on the [ruby role](/docs/ruby.md) role, which it will
automatically pull in because of it's [metadata](roles/fakes3/meta/main.yml).

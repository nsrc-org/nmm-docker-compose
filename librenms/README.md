# Librenms

This is the librenms+MariaDB stack, with some tweaks to `librenms.env` and a
`docker-compose.override.yml` file added.

To start, `cd` into this directory and then run

```
docker compose up -d
```

There is no default admin account; you will be prompted to create one the
first time you connect to the web interface.

## Source and references

Imported from [librenms/docker](https://github.com/librenms/docker) repo,
directory `examples/compose`.  Beware that there is a hidden file (`.env`)
in this directory.

See also [librenms docker
installation instructions](https://docs.librenms.org/Installation/Docker/).

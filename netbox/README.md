# Netbox

This is the community docker deployment of Netbox.  The default credentials
are:

* Username: **admin**
* Password: **admin**
* API Token: **0123456789abcdef0123456789abcdef01234567**

(which are created automatically unless you set `SKIP_SUPERUSER=true` in `env/netbox.env`)

## Plugins

It adds netbox-plugin-prometheus-sd and netbox-topology-views as described in
[Using Netblock Plugins](https://github.com/netbox-community/netbox-docker/wiki/Using-Netbox-Plugins).

You will need to start using:

```
docker compose build --no-cache
docker compose up -d
```

## Source and references

Imported from
[netbox-community/netbox-docker](https://github.com/netbox-community/netbox-docker)
repo, with scripts and reports from
[netbox-community/reports](https://github.com/netbox-community/reports).

See also the [Getting Started
guide](https://github.com/netbox-community/netbox-docker/wiki/Getting-Started).

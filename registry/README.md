# Pull-through docker registry cache

This provides a cache for docker hub, so that if multiple clients fetch the
same image, it is only downloaded once.

The registry is configured as "insecure" (HTTP) on port 5000 with no
authentication.  *Do not expose this directly to the Internet* (i.e.  make
sure this port is firewalled off)

To start, `cd` into this directory and run

```
docker compose up -d
```

## Configuring docker engine clients

Include this in `/etc/docker/daemon.json`:

```
{
  "insecure-registries": ["registry.ws.nsrc.org:5000"],
  "registry-mirrors": ["http://registry.ws.nsrc.org:5000"]
}
```

Replace "registry.ws.nsrc.org" with the hostname or IP address of the server
where the registry is running, and `systemctl restart docker`.  The client
should then attempt to fetch via the cache, in preference to talking
directly to docker hub.

## Viewing cache contents

After pulling an image, you can check whether it is now stored in the
registry like this:

```
$ curl registry.ws.nsrc.org:5000/v2/_catalog
{"repositories":["library/hello-world"]}
```

## Source and References

The docker compose file is derived from
[this template](https://docs.docker.com/registry/deploying/#deploy-your-registry-using-a-compose-file)
and the [mirror recipe](https://docs.docker.com/registry/recipes/mirror/).

The registry image is [here](https://hub.docker.com/_/registry) and
detailed configuration information is
[here](https://docs.docker.com/registry/configuration/).

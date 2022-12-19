# whoami test

This is a test container for traefik; it echoes back information about the
connecting client.

It is configured as a wildcard match, so it will work when connecting with
any hostname other than those matching another container.

To deploy:

```
docker compose up -d
```

To test:

```
curl -H 'Host:whoami.localhost' localhost
```

## Source and references

Based on the [docker compose basic
example](https://doc.traefik.io/traefik/user-guides/docker-compose/basic-example/).

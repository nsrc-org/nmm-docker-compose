# Traefik ingress proxy

Traefik is a reverse proxy which forwards web requests to the appropriate
containers, based on HTTP Host: header matching.

It monitors the docker socket for containers starting and stopping, and
updates its rules dynamically based on labels supplied by those containers.

## Source and references

Based on the [docker compose basic
example](https://doc.traefik.io/traefik/user-guides/docker-compose/basic-example/).

See also:
* [Docker provider](https://doc.traefik.io/traefik/providers/docker/)
* [Routers](https://doc.traefik.io/traefik/routing/routers/)

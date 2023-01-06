# Docker compose files for Network Monitoring and Management

This is a collection of docker compose files, mostly imported from existing
projects, and tweaked for use with the NSRC Network Monitoring and
Management labs.

Licenses are included within their respective directories.

## IPv6

Some of these examples include `enable_ipv6: true` in
`docker-compose.override.yml` to enable IPv6 container networking.

If you don't need this, then remove it.  If you want it to work, then
`/etc/docker/daemon.json` must have `"ipv6": true` and also define a
`fixed-cidr-v6`.

`docker compose` will attempt to create a new network for each project.
However, due to [this bug](https://github.com/moby/moby/issues/41438),
docker is unable to allocate more than one /64 network from a larger
assignment.  Therefore (as of Dec 2022) you need to list a bunch of
separate available networks, like this:

```
{
  "ipv6": true,
  "fixed-cidr": "172.17.0.0/24",
  "fixed-cidr-v6": "fd0c:d0ca:0:ffff::/64",
  "default-address-pools":[
          {"base": "172.17.0.0/16", "size": 24},
          {"base": "fd0c:d0ca:0:1::/64", "size": 64},
          {"base": "fd0c:d0ca:0:2::/64", "size": 64},
          {"base": "fd0c:d0ca:0:3::/64", "size": 64},
          {"base": "fd0c:d0ca:0:4::/64", "size": 64},
          {"base": "fd0c:d0ca:0:5::/64", "size": 64},
          {"base": "fd0c:d0ca:0:6::/64", "size": 64},
          {"base": "fd0c:d0ca:0:7::/64", "size": 64},
          {"base": "fd0c:d0ca:0:8::/64", "size": 64},
          {"base": "fd0c:d0ca:0:9::/64", "size": 64},
          {"base": "fd0c:d0ca:0:a::/64", "size": 64},
          {"base": "fd0c:d0ca:0:b::/64", "size": 64},
          {"base": "fd0c:d0ca:0:c::/64", "size": 64},
          {"base": "fd0c:d0ca:0:d::/64", "size": 64},
          {"base": "fd0c:d0ca:0:e::/64", "size": 64},
          {"base": "fd0c:d0ca:0:f::/64", "size": 64}
  ]
}
```

Unlike IPv4, docker does not configure NAT of IPv6 traffic to the host's own
IPv6 address.  To enable this, create
`/etc/networkd-dispatcher/routable.d/ip6nat` with contents similar to this:

```
#!/bin/sh
if [ "$IFACE" = "eth0" ]; then
  ip6tables -t nat -I POSTROUTING -s fd0c:d0ca::/32 -o "$IFACE" -j MASQUERADE
fi
```

and make it executable with `chmod +x`.  (Replace "eth0" with your external
interface name)

## DNS

Docker uses Google DNS by default.  If you have a nearer DNS cache, then
point to it in `/etc/docker/daemon.json`:

```
{
  "dns": ["100.64.0.1"],
  "dns-search": ["ws.nsrc.org"]
}
```

## Registry mirror

If you have many clients pulling the same images, then it's worth running a
local registry cache.

The clients need to be configured to use it in `/etc/docker/daemon.json`:
e.g.  if it's reachable using plain (unencrypted) HTTP on port 5000 on
`registry.ws.nsrc.org` then you'd set:

```
{
  "insecure-registries": ["registry.ws.nsrc.org:5000"],
  "registry-mirrors": ["http://registry.ws.nsrc.org:5000"]
}
```

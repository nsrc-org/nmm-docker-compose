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
  "fixed-cidr-v6": "2001:db8:d0ca:ffff::/64",
  "default-address-pools":[
          {"base": "172.17.0.0/16", "size": 24},
          {"base": "2001:db8:d0ca:1::/64", "size": 64},
          {"base": "2001:db8:d0ca:2::/64", "size": 64},
          {"base": "2001:db8:d0ca:3::/64", "size": 64},
          {"base": "2001:db8:d0ca:4::/64", "size": 64},
          {"base": "2001:db8:d0ca:5::/64", "size": 64},
          {"base": "2001:db8:d0ca:6::/64", "size": 64},
          {"base": "2001:db8:d0ca:7::/64", "size": 64},
          {"base": "2001:db8:d0ca:8::/64", "size": 64},
          {"base": "2001:db8:d0ca:9::/64", "size": 64},
          {"base": "2001:db8:d0ca:a::/64", "size": 64},
          {"base": "2001:db8:d0ca:b::/64", "size": 64},
          {"base": "2001:db8:d0ca:c::/64", "size": 64},
          {"base": "2001:db8:d0ca:d::/64", "size": 64},
          {"base": "2001:db8:d0ca:e::/64", "size": 64},
          {"base": "2001:db8:d0ca:f::/64", "size": 64}
  ]
}
```

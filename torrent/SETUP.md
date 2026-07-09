# Torrent

## Gluetun

See <https://github.com/qdm12/gluetun-wiki/blob/main/setup/providers/protonvpn.md> on how to obtain Proton credentials.

## Env variables

Generate Keys with:

```shell
openssl rand -hex 32
```

## Qbittorrent

Settings > Web UI

- Set Password
- Enable `Bypass authentication for clients on localhost`
- Enable `Bypass authentication for clients in whitelisted IP subnets`
  - Enter `172.31.255.0/24,fdd0:0:0:1000::/64`
- Enable `Enable reverse proxy support`
  - Enter `172.31.255.0/24,fdd0:0:0:1000::/64`

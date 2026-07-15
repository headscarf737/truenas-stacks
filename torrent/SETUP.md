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
- Generate API Key
- Enable `Bypass authentication for clients on localhost`
- Enable `Bypass authentication for clients in whitelisted IP subnets`
  - Enter `172.31.255.0/24,fdd0:0:0:1000::/64`
- Enable `Enable reverse proxy support`
  - Enter `172.31.255.0/24,fdd0:0:0:1000::/64`

See <https://trash-guides.info/Downloaders/qBittorrent/Basic-Setup/>:

- Basic Setup
- Categories

## Qui

Add Instance

URL: `http://torrent:8080`

Enable Local Filesystem Access

Use Auth none

## Autobrr

- Add Indexer
- Add Download Client
  - Host: `http://torrent:8080`
  - Enable Rules
    - Max active downloads: 2

## Uppollo (WIP)

```shell
sudo docker run --rm -it --user 568:568 -v ${MEDIA_FOLDER}:${MEDIA_FOLDER} -v /home/truenas_admin/upPollo:/home/uppollo/.config/upPollo --network traefik --entrypoint /usr/local/bin/upPollo uppollo/uppollo:latest upload --modq
```

- Set tmdb_api_key
- Set trackers
- Set qbittorrent
- Set screenshots
- Set crowdnfo

```yaml
# TODO: check if needed
path_mapping: # if you run qbittorrent on a remote server, you will need to map the paths
  enabled: true
  local: "/mnt/tank/media/torrent" # path to download folder on your local machine
  remote: "/data/torrent"
```

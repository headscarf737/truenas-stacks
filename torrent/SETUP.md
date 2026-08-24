# Torrent

## Gluetun

See <https://github.com/qdm12/gluetun-wiki/blob/main/setup/providers/protonvpn.md> on how to obtain Proton credentials.

## Env variables

Generate Keys with:

```shell
openssl rand -hex 32
```

## Qbittorrent

Download Proton VPN config and copy to `${APPS_FOLDER}/torrent/qbittorrent/wireguard/wg0.conf`. Remove IPv6 from Address

Settings > Web UI

- Set Password
- Generate API Key
- Enable `Bypass authentication for clients in whitelisted IP subnets`
  - Enter `172.31.255.254/32,fdd0:0:0:1000::fe/128`
- Enable `Enable reverse proxy support`
  - Enter `172.31.255.254/32,fdd0:0:0:1000::fe/128`

See <https://trash-guides.info/Downloaders/qBittorrent/Basic-Setup/>:

- Basic Setup
- Categories

## Qui

Add Instance

URL: `http://qbittorrent:8080`

Enable Local Filesystem Access

Enter API Key

## Autobrr

- Add Indexer
- Add Download Client
  - Host: `http://qbittorrent:8080`
  - Enter API Key
  - Enable Rules
    - Max active downloads: 2

## upPollo

```shell
sudo docker run --rm -it --user 568:568 -v ${MEDIA_FOLDER}:${MEDIA_FOLDER} -v /home/truenas_admin/upPollo:/home/uppollo/.config/upPollo --network container:torrent-gluetun --entrypoint /usr/local/bin/upPollo uppollo/uppollo:nightly upload --modq --qbit-auto-tmm
```

<!--
sudo docker run --rm -it --user 568:568 -v /mnt/tank/media:/mnt/tank/media -v /home/truenas_admin/upPollo:/home/uppollo/.config/upPollo --network container:torrent-gluetun --entrypoint /usr/local/bin/upPollo uppollo/uppollo:nightly upload --qbit-auto-tmm
-->

- Set TMDB, TVDB API Keys
- Set Path Mapping `${MEDIA_FOLDER}/torrent:/data/torrent`
- Set trackers
- Set qbittorrent
- Set reuse_hash to `true`
- Set screenshots
- Enable crowdnfo, configure api key

## parsec

```shell
sudo docker run --rm -it --user 568:568 -v ${MEDIA_FOLDER}:${MEDIA_FOLDER} -v /home/truenas_admin/parsec:/home/parsec/.config/parsec --network container:torrent-gluetun --entrypoint /usr/local/bin/parsec uppollo/parsec:nightly check
```

<!--
sudo docker run --rm -it --user 568:568 -v /mnt/tank/media:/mnt/tank/media -v /home/truenas_admin/parsec:/home/parsec/.config/parsec --network container:torrent-gluetun --entrypoint /usr/local/bin/parsec uppollo/parsec:nightly check
-->

## auditorr

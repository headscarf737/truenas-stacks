# truenas-stacks

## Description

This is the repo for my NAS server based on TrueNAS Community Edition with various docker compose stacks for media, home automation, networking, and other services.

## Notice

All the Services mentioned in this repository are only for educational purposes. Do not use any of the services to pirate copyrighted content.

All Services have legitimate and legal use cases.

Ensure you comply with all relevant laws and regulations in your jurisdiction when using these services. Only download content that is not copyrighted and for which you have the necessary rights and permissions.

The author is not responsible for any misuse of the provided services.

With above in mind, you can deploy a subset of the stacks like:

```shell
docker compose up -d <space separated service names>
```

## Content

This repo includes the following stacks:

- [auth](./auth): Authentication stack with Authelia and LLDAP

  - Authelia: Used for 2FA and SSO
  - LLDAP: Lightweight LDAP server for user management

- [immich](./immich): Immich self-hosted photo and video backup and management solution

- [media](./media): Arr stack based on Usenet

  - Gluetun: VPN client for secure connections
  - Jellyfin: Media server for streaming
  - Prowlarr: Indexer manager
  - Radarr: Movie collection manager
  - Sonarr: TV show collection manager
  - Whisparr: Adult collection manager
  - Bazarr: Subtitle management
  - Sabnzbd: Usenet downloader
  - Seerr: Media request management
  - Stash: Adult media manager

- [monitoring](./monitoring): Monitoring stack for server health and performance

  - Homepage: Dashboard for quick access to services
  - Scrutiny: SMART monitoring for drives

- [network](./network): Network services stack

  - Traefik: Traefik reverse proxy with Cloudflare DNS ACME
  - CrowdSec: Security tool to protect against attacks
  - Technitium DNS Server: Split DNS server

- [nextcloud](./nextcloud): Nextcloud self-hosted cloud storage and collaboration platform

  - Nextcloud: Core application for file storage and sharing
  - MariaDB: Database backend for Nextcloud
  - Redis: Caching server to improve performance
  - Collabora: Online office suite integration

## Planned Content

- OpenCloud - Nextcloud alternative
- Tdarr - Pre-transcode media
- UmlautAdaptarr - Fix issues with german umlauts

## Updates

Stacks are updated regularly using Renovate

## Server

TODO: add geizhals whishlist link

You can find the parts list here: <https://geizhals.de/>

## Name

The name used for the server is `sodium`

## Setup

See [SETUP.md](SETUP.md) for setup instructions.

See the `SETUP.md` files in the subdirectories for service-specific setup instructions.

## Lint

To lint the docker compose files, you can use [docker-compose-linter](https://github.com/zavoloklom/docker-compose-linter)

```shell
pnpm dlx dclint **/.
```

## Resources

- <https://github.com/tomMoulard/make-my-server>
- <https://github.com/geekau/mediastack>
- <https://wiki.servarr.com>
- <https://trash-guides.info>
- <https://github.com/PCJones/usenet-guide>

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

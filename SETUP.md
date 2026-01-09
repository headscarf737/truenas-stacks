# Install Guide

## Before You Start

If your drive is `512e`, you should switch to `4kn` for better performance.

## Notice

This setup assumes a very specific TrueNAS setup

You should have two pools: one SSD pool for the applications and one HDD pool for media/content.

You need to have a publicly accessible domain name with DNS records pointing to your home IP address. Your home IP address can't be behind CG-NAT.

## Interface

In your Network settings, make sure that your network interface has three IP addresses assigned.

The first one will be used for internal communication inside your network. (192.168.178.221)

The second one will be used for external communication from outside your network. This one should be port forwarded from your router/firewall to your TrueNAS server. (192.168.178.222)

The third one will be used for TrueNAS web interface and services not using the reverse proxy. (192.168.178.220)

Set your TrueNAS web interface to only listen on the third IP address.

## Apps Datasets

These datasets should be created on your SSD pool

`APPS_FOLDER` should be the path to the apps dataset (e.g. `/mnt/apps/docker`).

Enable Auto TRIM on the SSD pool

Set Record size to 16K on the Docker dataset

### ACL

Set owner and group to `apps`. Don't forget to check the box to apply

Permissions should only contain `builtin_administrators` with Full Control, and `apps` with Modify

When using git, make sure to set add the following config to avoid permission issues:

```shell
git config --global --add safe.directory /mnt/apps/docker/stacks
```

### Docker

- `stacks`

### Auth

- `auth/authelia`
- `auth/lldap`

### Immich

- `immich/db`

### Media

- `media/bazarr`
- `media/usenet` (Record size 1M)
- `media/configarr`
- `media/gluetun`
- `media/jellyfin`
- `media/prowlarr`
- `media/radarr`
- `media/sabnzbd`
- `media/seerr`
- `media/sonarr`
- `media/stash`
- `media/whisparr`

### Monitoring

- `monitoring/scrutiny/config`
- `monitoring/scrutiny/data`
- `monitoring/fluentbit`

### Network

- `network/traefik/data`
- `network/traefik/logs`
- `network/crowdsec/data`
- `network/crowdsec/config`
- `network/dns`

### Nextcloud

- `nextcloud/db`
- `nextcloud/app`
- `nextcloud/redis`
- `nextcloud/logs`

## Media / Content Datasets

These datasets should be created on your HDD pool

`MEDIA_FOLDER` should be the path to the media dataset (e.g. `/mnt/tank/media`).

Set Record size to 1M on the media dataset

- `media`
- `media/movies` - Movies (Radarr)
- `media/tv` - TV Shows (Sonarr)
- `media/adult` - Adult Content (Whisparr)

`CONTENT_FOLDER` should be the path to the content dataset (e.g. `/mnt/tank/content`).

Keep default Record size 128K on the content dataset

- `content`
- `content/files` - Files (Nextcloud)
- `content/images` - Photos and Videos (Immich)

## Git

Pull the repository to the SSD pool apps stacks dataset

## Environment Variables

Copy the example environment file:

```shell
cp .env.example .env
```

change permissions to protect your secrets:

```shell
chmod 600 .env
```

## Docker Networks

```shell
sudo docker network create --subnet 172.31.255.0/24 --subnet fdd0:0:0:1000::/64 traefik
sudo docker network create --internal --subnet 172.31.254.0/24 --subnet fdd0:0:0:fff::/64 auth_internal
```

## Email

In Settings > General enter your email information and send a test email to verify that everything is working.

## Snapshots

Create snapshot tasks for your `docker` dataset (e.g. daily, but exclude `media/usenet`) and your `content` dataset (e.g. hourly).

## Smb User Shares

If you want to make some disks available via SMB, create a dataset `Users` (with SMB Preset) and one dataset per user inside it (with SMB Preset but without Share).

Permissions for the `Users` dataset. Owner should be `root:builtin_users`. Group `builtin_administrators` with Modify, and `group@` with Read

Permissions for each user dataset. Owner should be the respective user and group. `owner@` should have Modify

The Share ACL for the `Users` share should contain group `builtin_users` with Full Control

Set a Quota for each user dataset if needed.

## Smart Monitoring

Use <https://github.com/JoeSchmuck/Multi-Report/blob/main/Multi_Report_Quick_Start_Guide.pdf> to set up email reports for your SMART data.

## Run Docker Command in all Stacks

```shell
./docker-all <docker-commands>
# e.g. ./docker-all compose up -d
```

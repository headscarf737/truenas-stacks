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

## Docker Datasets

These datasets should be created on your SSD pool (`apps`).

`APPS_FOLDER` should be the path to the docker dataset (e.g. `/mnt/apps/docker`).

Enable Auto TRIM on the SSD pool.

Set Record size to 16K on the docker dataset.

Enable Encryption if you want to use it.

### ACL

Set owner and group to `apps`. Don't forget to check the box to apply.

Permissions should only contain `builtin_administrators` with Full Control, and `apps` with Modify.

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
- `immich/model-cache`
- `immich/model-dotcache`
- `immich/model-config`

### Media

- `media/bazarr`
- `media/usenet` (Record size 1M)
- `media/configarr`
- `media/gluetun`
- `media/jellyfin`
- `media/prowlarr`
- `media/radarr`
- `media/radarr-uhd`
- `media/sabnzbd`
- `media/seerr`
- `media/sonarr`
- `media/sonarr-uhd`
- `media/stash`
- `media/thelounge`
- `media/whisparr`
- `media/whisparr2`

### Monitoring

- `monitoring/crowdsec`
- `monitoring/fluentbit`
- `monitoring/grafana`
- `monitoring/scrutiny/config`
- `monitoring/scrutiny/data`
- `monitoring/victorialogs`

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

These datasets should be created on your HDD pool (`tank`).

`MEDIA_FOLDER` should be the path to the media dataset (e.g. `/mnt/tank/media`).

Set Record size to 1M on the media dataset.

- `media`

There are no child datasets to allow hardlinks.

`CONTENT_FOLDER` should be the path to the content dataset (e.g. `/mnt/tank/content`).

Keep default Record size 128K on the content dataset.

Enable Encryption if you want to use it.

- `content`
- `content/files` - Files (Nextcloud)
- `content/images` - Photos and Videos (Immich)

## Encryption

TODO

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

In System > General Settings enter your email information and send a test email to verify that everything is working.

## Alerts

In System > Alert Settings set Email Level to Notice.

Under the Category System change Update Available to NOTICE.

## Snapshots

Create `recursive` snapshot tasks:

- apps/docker
  - Exclude:
    - `apps/docker/media/usenet`
    - `apps/docker/immich/model-cache`
    - `apps/docker/immich/model-dotcache`
    - `apps/docker/immich/model-config`
  - Lifetime: 2 Weeks
  - Schedule: Daily
- tank/content
  - No excludes
  - Lifetime: 2 Weeks
  - Schedule: Hourly

Create snapshots for any other datasets as needed.

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

## Maintenance

Run docker cleanup commands regularly to keep your system tidy:

```shell
sudo docker system prune
```

See disk usage with

```shell
sudo docker system df
```

## Copy Data

To copy data between datasets, use `cp --reflink=auto` for faster transfer.

As TrueNAS ships with an older version of `cp`, use this docker image to copy data:

```shell
sudo docker run --rm -it -v VOLUME_MAPPING debian
```

### Merge Datasets

Rename child dataset

```shell
sudo zfs rename old old-temp
```

Merge child dataset into parent dataset

```shell
cp -a --reflink=auto old-temp old
```

Now delete the dataset `old-temp`

If you have snapshots enabled on the (parent) dataset you should delete the snapshots first, otherwise you will use twice the space for the lifetime of the snapshots.

# Nextcloud

## Setup File

```shell
touch ${APPS_FOLDER}/nextcloud/redis/redis-session.ini
```

Start Container to create files

Add `'logfile' => '/var/log/nextcloud/nextcloud.log',` to the config.php file located at `${APPS_FOLDER}/nextcloud/app/config/config.php`

Remove `nextcloud.log` from `${CONTENT_FOLDER}/files`

## Install Office

Go to Apps and install `Nextcloud Office`

Go into Settings > Administration > Nextcloud Office and set the Document Server address to `http://collabora:9980`

Enable OOXML as default

Set the internal WOPI callback URL:

```shell
sudo docker compose exec nextcloud ./occ config:app:set richdocuments wopi_callback_url --value='http://nextcloud'
```

Add `172.31.253.2/32,fdd0:0:0:ffe::2/128` to the WOPI allow list

TODO fonts

## Basic Configs

```shell
./occ maintenance:repair --include-expensive
./occ config:system:set maintenance_window_start --type=integer --value=1
./occ db:add-missing-indices
```

### Email

Setup Email Server

Choose the System Email Account

### Ldap

Install LDAP User and Group Backend App

Host: `lldap:3890`

LDAP User-DN: `uid=service,ou=people,dc=sodium,dc=example,dc=com`

Base-DN: `ou=people,dc=sodium,dc=example,dc=com`

User Filter: `(&(objectclass=person)(memberOf=cn=files,ou=groups,dc=sodium,dc=example,dc=com))`

Expert > Internal Username set to `user_id`

### Cron

Set Background jobs to `Cron` in the Basic Settings

Add a TrueNAS Cron Job with the following command

```shell
docker exec -u 568:568 nextcloud php ./cron.php
```

Run as user `root`

Custom Schedule: `*/5 * * * *` (It should display `Every 5 minutes`)

## Install Whiteboard

Generate JWT Secret

```shell
openssl rand -hex 16
```

Go to Apps and install `Whiteboard`

Go into Settings > Administration > Whiteboard and set the Server address to `https://whiteboard.${DOMAIN}`

Paste the generated JWT Secret into the Shared Secret field

Adjust max image size

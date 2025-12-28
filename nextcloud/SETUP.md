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

Go into Settings > Administration > Nextcloud Office and set the Document Server address to `https://collabora.${DOMAIN}`

Enable OOXML as default

Add `172.31.255.0/24` to wopi allow list

TODO fonts

## Basic Configs

TODO CRON

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

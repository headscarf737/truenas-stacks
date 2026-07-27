# Monitoring

## Crowdsec UI

Generate API key

```shell
openssl rand -hex 32
```

Create machine (in the `network` stack)

```shell
sudo docker compose exec crowdsec cscli machines add crowdsec-ui --password <generated_password> -f /dev/null
```

## Setup Homepage

Get keys from the various places

Get crowdsec key from `crowdsec/config/local_api_credentials.yaml`

Create immich key with `server.statistics` permissions

Create nextcloud key using instructions provided under Settings > System

Create TrueNAS key with Readonly Admin access

### Technitium DNS

Create new user with long random password

Create Token for that user

# Monitoring

## Beszel

Set `BESZEL_KEY` in `.env` to the hub's public key. Add the local system with `/beszel_socket/beszel.sock` as Host / IP.

## Crowdsec UI

Generate machine password

```shell
openssl rand -hex 32
```

Create machine (in the `network` stack)

```shell
sudo docker compose exec crowdsec cscli machines add crowdsec-ui --password <generated_password> -f /dev/null
```

Set `CROWDSEC_API_KEY` in `monitoring/.env` to the generated machine password.

## Setup Homepage

Get keys from the various places

Get CrowdSec login and password from `${APPS_FOLDER}/network/crowdsec/config/local_api_credentials.yaml`

Create immich key with `server.statistics` permissions

Create nextcloud key using instructions provided under Settings > System

Create TrueNAS key with Readonly Admin access

### Technitium DNS

Create new user with long random password

Create Token for that user

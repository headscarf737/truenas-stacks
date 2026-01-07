# Network

## Setup Environment

`NET_INTERFACE_INTERNAL` should be set to your first IP address of the network interface

`NET_INTERFACE_EXTERNAL` should be set to your second IP address of the network interface

## Cloudflare DNS API Token

Create a Cloudflare DNS API token with the following permissions:

- Zone - DNS - Edit
- Zone - Zone - Read

Use this for `CF_DNS_API_TOKEN`

## Setup DNS

For the self-signed certificate for the DNS server to work you have to restart it

Log in the admin Panel at <https://$NET_INTERFACE_DNS4:53443> with user `admin`

Create Primary Zone `${DOMAIN}`

Add A record `${DOMAIN}` pointing to `$NET_INTERFACE_INTERNAL`

Add CNAME record `*.${DOMAIN}` pointing to `${DOMAIN}`

Add `${DOMAIN}` to your routers rebind protection whitelist.

Do the same for your Public DNS provider (e.g. Cloudflare) so that `${DOMAIN}` and `*.${DOMAIN}` point to your public IP address.

### Static IPv6

Choose an ULA prefix for your network, e.g. `fd42:168:178:220::/64`

In your router add a static IPv6 route for `fd42:168:178:220::/64` via your link local address.

Add `NET_INTERFACE_DNS6` as an alias to your network interface

## Setup Crowdsec

Generate a secure API key for `CROWDSEC_API_KEY`

```shell
openssl rand -hex 16
```

Visit <https://app.crowdsec.net/security-engines?distribution=linux> and look for a command like this

```shell
sudo cscli console enroll <ENROLL_KEY>
```

Use the `ENROLL_KEY` for `CROWDSEC_ENROLL_KEY`

Start the stack and accept the enroll request

Run this command to add extra context information to Crowdsec Alerts

```shell
sudo docker compose exec -it crowdsec /bin/sh
# and then inside the container
cat > /etc/crowdsec/contexts/extra_info.yaml << EOF
context:
  host:
  - evt.Meta.target_fqdn
EOF
```

Restart the stack again to apply the new configuration.

Subscribe to some Blocklists on <https://app.crowdsec.net/blocklists> (e.g. Firehol cruzit.com list, Firehol cybercrime tracker list, Firehol greensnow.co list)

# Media

## Gluetun

See <https://github.com/qdm12/gluetun-wiki/blob/main/setup/providers/mullvad.md> on how to obtain Mullvad credentials.

## Env variables

Generate Keys with:

```shell
openssl rand -hex 16
```

## sabnzbd

Edit `data/sabnzbd/sabnzbd.ini` and add `sabnzbd.$DOMAIN` to the host_whitelist

Temporary Download Folder: `/data/usenet/incomplete`
Completed Download Folder: `/data/usenet/complete`

Categories:
movies -> movies
tv -> tv
adult -> adult

See <https://trash-guides.info/Downloaders/SABnzbd/Basic-Setup>:

- Tuning
- System Folders
- Servers

Get API Key for Sonarr/Radarr/Whisparr integration.

## Whisparr2

Edit `${APPS_FOLDER}/media/whisparr2/config.xml`

```xml
<Port>6968</Port>
<ApiKey>$WHISPARR_API_KEY$</ApiKey>
<AuthenticationMethod>External</AuthenticationMethod>
<AnalyticsEnabled>False</AnalyticsEnabled>
```

## Prowlarr

Add Sonarr, Radarr, Whisparr, Whisparr2 instances with API keys.

Use `media` as hostname instead of `localhost`

Add you preferred indexers.

## Sonarr / Radarr / Whisparr

Add Download Client. Add SABnzbd instance. Use `media` as hostname instead of `localhost`.

Use category `adult` for Whisparr
Use category `adult2` for Whisparr2

Media Management: Propers and Repacks: Do Not Prefer

Media Management: Root Folders:

- /media/tv (Sonarr)
- /media/movies (Radarr)
- /media/adult (Whisparr)
- /media/adult2 (Whisparr2)

## Stash

Create account <https://guidelines.stashdb.org/docs/faq_getting-started/stashdb/accessing-stashdb/>. Add API to Metadata providers.

Whisparr: Add Connection Stash, Port: 9998, match Settings like manual settings in Stash. Enable Identify Task

## Bazarr

Follow <https://wiki.bazarr.media/Getting-Started/Setup-Guide/>

## Configarr

```shell
sudo docker compose --profile=manual up -d
sudo docker compose logs -f configarr
```

## Jellyfin

Go to the Transcoding settings and set the Engine to VAAPI

### Ldap

Install the LDAP plugin from the Jellyfin Plugin Catalog.

LDAP Server: `lldap`

LDAP Port: `3890`

LDAP Bind User: `uid=service,ou=people,dc=sodium,dc=example,dc=com`

LDAP Base DN: `ou=people,dc=sodium,dc=example,dc=com`

LDAP Search Filter: `(memberof=cn=media,ou=groups,dc=sodium,dc=example,dc=com)`

LDAP Search Attributes: `uid, mail`

LDAP Uid Attribute: `uid`

LDAP Username Attribute: `uid`

Enable profile image synchronization: true

LDAP Admin Filter: `(memberof=cn=admin,ou=groups,dc=sodium,dc=example,dc=com)`

### Network

Network > Known Proxies : `172.31.255.0/24,fdd0:0:0:1000::/64`

### Public Live TV Channels

```m3u
#EXTM3U
#EXTINF:-1 tvg-id="DE: Das Erste" tvg-name="Das Erste" group-title="ARD",Das Erste
https://daserste-live.ard-mcdn.de/daserste/live/hls/de/master.m3u8
```

See <https://github.com/jnk22/kodinerds-iptv/blob/master/iptv/clean/clean.m3u> and <https://xmltv.info/> or <https://www.open-epg.com/app/index.php>

## Seerr

Setting up seerr is straight forward. Just follow the onboarding steps.

Add Sonarr and Radarr instances like in Prowlarr section.

For Sonarr make sure to enable Season Folders

Setting up Email notifications is recommended.

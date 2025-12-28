# Auth

## Generate Secrets

Before starting the LDAP service, you need to generate some secrets for secure operation. You can use the following commands to generate the required secrets:

```shell
openssl rand -base64 30
```

## Environment Variables

Copy the example environment file:

```shell
cp .env.example .env
cp lldap.env.example lldap.env
cp authelia.env.example authelia.env
```

change permissions to protect your secrets:

```shell
chmod 600 .env
chmod 600 lldap.env
chmod 600 authelia.env
```

## lldap

Create Account `service` for LDAP binding. Give it the group `lldap_strict_readonly`

Create Account `service_auth` for Authelia binding. Give it the group `lldap_password_manager`

Default username is `admin`

### User-defined Attributes

| Name         | Type    |
| ------------ | ------- |
| immich-quota | Integer |

### Groups

Add Group `admin`, `images`, `files`, `media`

## Authelia

Set secret permissions

```shell
chmod 0700 ${APPS_FOLDER}/auth/authelia/secrets
```

Generate secrets

```shell
docker run --rm -u 568:568 -v ${APPS_FOLDER}/auth/authelia/secrets:/secrets authelia/authelia sh -c "cd /secrets && authelia crypto rand --length 64 session_secret.txt storage_encryption_key.txt jwt_secret.txt"
```

Generate Keys

```shell
docker run --rm -u 568:568 -v ${APPS_FOLDER}/auth/authelia/keys:/keys authelia/authelia authelia crypto pair rsa generate --directory /keys
```

Generate Hmac Secret

```shell
docker run --rm authelia/authelia authelia crypto rand --length 72 --charset alphanumeric
```

Generate Client ID

```shell
docker run --rm authelia/authelia authelia crypto rand --length 72 --charset rfc3986
```

Generate Client Secret

```shell
docker run --rm authelia/authelia authelia crypto hash generate pbkdf2 --variant sha512 --random --random.length 72 --random.charset rfc3986
```

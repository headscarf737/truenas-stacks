# Immich

## Settings

Disable Video Transcoding

## OAuth2

| Setting                            | Value                                    |
| ---------------------------------- | ---------------------------------------- |
| Issuer URL                         | <https://auth.$DOMAIN>                   |
| Client ID                          | From Auth                                |
| Client Secret                      | From Auth                                |
| Token Endpoint Auth Method         | client_secret_post                       |
| Scope                              | openid email profile groups immich_scope |
| ID Token Signed Response Algorithm | RS256                                    |
| Userinfo Signed Response Algorithm | RS256                                    |
| Storage Label Claim                | preferred_username                       |
| Role Claim                         | groups                                   |
| Storage Quota Claim                | immich-quota                             |
| Default Storage Quota (GiB)        | empty                                    |
| Button Text                        | Sign in with Authelia                    |
| Auto Register                      | Enabled                                  |
| Auto Launch                        | Enabled                                  |
| Mobile Redirect                    | URI Override Disable                     |

After verifying OAuth login works, you can disable password login

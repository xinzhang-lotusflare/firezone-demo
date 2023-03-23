## Prepare /etc/hosts
Add those entries in `/etc/hosts`

```
127.0.0.1 admin.firezone.lf
127.0.0.1 openid
127.0.0.1 keycloak
```

## Start services
Execute cmds to initialize firezone:

```
DATABASE_PASSWORD=123 VERSION=0.7.25 sudo docker compose run --rm firezone bin/migrate
DATABASE_PASSWORD=123 VERSION=0.7.25 sudo docker compose run --rm firezone bin/create-or-reset-admin
```

Start Keycloak first for init configuration: add groups, users, and oidc clients.

```
docker compose up keycloak -d
```

Then start firezone
```
docker compose up firezone
```

It can be accessed in browser: http://admin.firezone.lf:13000

## Status

WIP.

### Failed for oidc clients

It would fail if login via oidc clients, either `dex` or `keycloak` directly. Error log:

```
firezone-demo-firezone-1  | 06:22:16.864 erl_level=error application=fz_http domain=elixir file=lib/fz_http_web/controllers/auth_controller.ex function=oidc_callback/2 line=91 mfa=FzHttpWeb.AuthController.oidc_callback/2 module=FzHttpWeb.AuthController pid=<0.523.0> request_id=F074IjTAiP37nqcAAADB [error] An OpenIDConnect error occurred. Details: "Cannot verify state"
```

### Failed for init email user

It says `Forbidden` after login :-(


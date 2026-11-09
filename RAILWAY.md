# Deploying LibreCrawl on Railway

Railway ignores docker-compose.yml, so config comes from service variables.

## Variables (service -> Variables)
| Name | Value |
|---|---|
| `SECRET_KEY` | `python3 -c "import secrets; print(secrets.token_hex(32))"` |
| `LOCAL_MODE` | `false` (never `true`: it makes every visitor admin) |
| `DANGEROUSLY_SKIP_AUTH` | `false` |
| `REGISTRATION_DISABLED` | `false` until the team has signed up, then `true` |
| `PORT` | `5000` (the app binds 0.0.0.0:5000, hardcoded) |
| `RAILWAY_RUN_UID` | `0` (volumes mount as root; the image runs as uid 1000) |

## Volume
Add a Volume mounted at `/app/data` (holds `users.db`: accounts and saved crawls).

## Networking
Settings -> Networking -> Generate Domain, target port 5000.

## First run
1. Open the URL and register the admin account first (first signup = admin).
2. Teammates register; approve them via the Railway shell:
   `python manage.py users` / `approve <name>` / `set-tier <name> user`
3. Optionally set `REGISTRATION_DISABLED=true` afterwards.

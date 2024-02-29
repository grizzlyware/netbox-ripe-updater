# Netbox RIPE Updater

A Docker Compose wrapper for the RIPE updater and supporting utilities.

## Requirements

* Docker
* Netbox
* RIPE DB access

## Setup

```bash
git clone git@github.com:grizzlyware/netbox-ripe-updater.git
cd netbox-ripe-updater
cp .env.example .env
cp .env.updater.example .env.updater
docker compose up # -d flag to daemonize
```

## ripe-updater

The ripe-updater can be found at: [./ripe-updater](./ripe-updater)

Most of the README is still applicable, with some defaults being changed to match the Docker Compose setup.

### Changes from the original version

* The `/backup` endpoints are now authorised by HTTP basic auth, set via `UI_USER` & `UI_PASSWORD` in the `.env.updater` file
* Backups are enabled by default to a local Docker instance of Minio. Credentials are hard coded, but the Minio container is not bound to the host, so it's not accessible from the outside world

## Tips

If this application is being hosted on the same machine as Netbox, you should reverse proxy the `/backup` endpoints to the Netbox instance to benefit from TLS.

If the application is hosted on a different machine, you should consider overriding the docker-compose.yml file to include a HTTPS server such as Caddy to expose the updater via TLS. When doing this, the host port binding of 9000 (`UPDATER_HTTP_PORT`) should be removed, and the HTTP proxy exposed on the host instead.


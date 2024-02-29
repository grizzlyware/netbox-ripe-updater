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
cp .env.minio.example .env.minio
docker compose up # -d flag to daemonize
```

## ripe-updater

The ripe-updater can be found at: [./ripe-updater](./ripe-updater)


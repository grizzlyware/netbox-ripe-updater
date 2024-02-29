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


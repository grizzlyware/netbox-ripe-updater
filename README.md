![image](https://github.com/grizzlyware/netbox-ripe-updater/assets/1097093/f8b7442d-2223-4486-b33b-f4fb77314727)

# Netbox RIPE Updater

A Docker Compose wrapper for the [RIPE updater](https://github.com/interdotlink/ripe-updater) and supporting utilities, including local backups and custom `NETNAME`.

> [!NOTE]  
> Credit to the original authors of `ripe-updater`, see: https://github.com/interdotlink/ripe-updater

## Requirements

* Docker
* Netbox
* RIPE DB access

## Setup

```bash
git clone https://github.com/grizzlyware/netbox-ripe-updater.git
cd netbox-ripe-updater
cp .env.example .env
cp .env.updater.example .env.updater
```

### Configuration

* Configure the `.env` and `.env.updater` files to match your environment.
* Configure/copy templates in the `templates` directory to match your environment (to the custom directory usually).

> [!WARNING]  
> Ensure you update your `.env` and `.env.updater` to use non-default `MINIO_ROOT_USER`, `MINIO_ROOT_PASSWORD` `S3_ACCESS_KEY` & `S3_SECRET_ACCESS_KEY` variables

### Running

After configuration and templates, start the containers:

```bash
docker compose up # -d flag to daemonize
```

### Updating

To update the containers, pull the latest changes and restart the containers:

```bash
git pull
docker compose down
docker compose build
docker compose up -d
```

You may sometimes need to rebuild the containers using `docker compose build`, for example if the Dockerfile or Python code has changed.

## ripe-updater

The ripe-updater can be found at: [./ripe-updater](./ripe-updater)

Most of the README is still applicable, with some defaults being changed to match the Docker Compose setup.

### Changes from the original version

* The `/backup` endpoints are now authorised by HTTP basic auth, set via `UI_USER` & `UI_PASSWORD` in the `.env.updater` file
* Backups are enabled by default to a local Docker instance of Minio. Credentials are hard coded, but the Minio container is not bound to the host, so it's not accessible from the outside world
* Minor tweaks and fixes
* The `_TEST_` environment variables are not used, testing can be achieved by changing `RIPE_TEMPLATE_DIR`
* The ISO Alpha 2 country code is fetched from the regions slug in Netbox, ie `gb` for the UK.
* Added `INCLUDE_DESCR` & `INCLUDE_ORG` to the configuration, to allow for the exclusion of `descr` and `org` fields in the INETNUM objects (defaulted to include them).

## Tips

If this application is being hosted on the same machine as Netbox, you should reverse proxy the application to the Netbox instance to benefit from TLS:

```nginx
location /ripe-updater/ {
    proxy_pass http://127.0.0.1:9000/;
} 
```

If the application is hosted on a different machine, you should consider overriding the docker-compose.yml file to include a HTTPS server such as Caddy to expose the updater via automatic TLS. When doing this, the host port binding of 9000 (`UPDATER_HTTP_PORT`) should be removed, and the HTTP proxy exposed on the host instead.

## Authors

* [Josh Bonfield (Grizzlyware Ltd)](https://www.grizzlyware.com)

## Sponsors

* [Netwise Hosting Ltd - UK Colocation](https://www.netwise.co.uk)

![image](https://github.com/grizzlyware/netbox-ripe-updater/assets/1097093/4531d922-4a78-4abd-a173-4cf14a969b3e)



# Install Nextcloud on Ubuntu 22.04 using Docker AIO Configuration

## Original Documentation Links Links

- [Docker Install Guide](https://docs.docker.com/engine/install/ubuntu/)

- [NextCloud AIO Install Guide](https://github.com/nextcloud/all-in-one)


## Setup NextCloud

Use Docker run to start and run NextCloud AIO

```bash
# For Linux:
sudo docker run \
--init \
--sig-proxy=false \
--name nextcloud-aio-mastercontainer \
--restart always \
--publish 8080:8080 \
--env APACHE_PORT=11000 \
--env APACHE_IP_BINDING=0.0.0.0 \
--env APACHE_ADDITIONAL_NETWORK="" \
--env SKIP_DOMAIN_VALIDATION=true \
--volume nextcloud_aio_mastercontainer:/mnt/docker-aio-config \
--volume /var/run/docker.sock:/var/run/docker.sock:ro \
ghcr.io/nextcloud-releases/all-in-one:latest
```

Find the password for the NextCloud AIO admin user

```bash
sudo cat /var/lib/docker/volumes/nextcloud_aio_mastercontainer/_data/data/configuration.json | grep password
```
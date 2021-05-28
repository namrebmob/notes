# [Nextcloud](https://nextcloud.com)

[Apps](https://apps.nextcloud.com)
[User Manual](https://docs.nextcloud.com/server/21/user_manual/)
[Administration Manual](https://docs.nextcloud.com/server/21/admin_manual/)
[Developer Manual](https://docs.nextcloud.com/server/21/developer_manual/)
[Docker Official Images](https://hub.docker.com/_/nextcloud)
[Docker Images Github](https://github.com/nextcloud/docker)

## Configuration

### Via envars

```.env
# logging
NC_log_type=syslog
NC_logfile=''
NC_logleve=3

NC_auto_logout=true

NC_filelocking.enabled=false

NC_simpleSignUpLink.shown=false
```

## Run in docker container

```sh
docker run -d \
    -p 8080:80 \
    -v nextcloud:/var/www/html \
    -v apps:/var/www/html/custom_apps \
    -v config:/var/www/html/config \
    -v data:/var/www/html/data \
    --name <name> \
    --rm \
    nextcloud
```

## Run in gcloud vm free tier, archlinux image

```sh
gcloud compute instances create $INSTANCE_NAME \
    --deletion-protection \
    --description="Nextcloud" \
    --hostname=$INSTANCE_HOSTNAME \
    --image-project=arch-linux-gce \
    --image-family=arch \
    --machine-type=f1-micro \
    --zone=us-central1-a
```
## Cron jobs

```sh
docker-compose exec app crontab -u www-data -e
```

## Preview generation

1. Install [Preview Generator app](https://apps.nextcloud.com/apps/previewgenerator)
2. generate all previews via:

```sh
./occ preview:generate-all -vvv
```

3. add php /var/www/nextcloud/occ preview:pre-generate to a system cron job:

```sh
docker-compose exec app crontab -u www-data -e
0 * * * * php /var/www/nextcloud/occ preview:pre-generate
```

## OCC cli

### run with custom memory limit

```sh
docker-compose exec --user www-data app php -d memory_limit=1024M occ
```

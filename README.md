# PHP S2I Images CentOS  / Alpine

![Docker Stars](https://img.shields.io/docker/stars/mvilche/php-s2i.svg)
![Docker Pulls](https://img.shields.io/docker/pulls/mvilche/php-s2i.svg)
![Docker Automated](https://img.shields.io/docker/cloud/automated/mvilche/php-s2i)
![Docker Build](https://img.shields.io/docker/cloud/build/mvilche/php-s2i)


# Features

- Non-root
- Okd Ready!
- Kubernetes Ready!
- S2i build images
- Composer and npm
- Artisan Migrations
- Composer nexus private repository
- Composer Version 1.10.16
- Php-fpm + Apache Images
- Php-fpm + Nginx Images
- Apache + Mod_php Images (Deprecated)
- Nginx Prometheus metrics
- Fpm Prometheus metrics


### Deploy Environments



| Environment | Details |
| ------ | ------ |
| TIMEZONE | Set Timezone (America/Montevideo, America/El_salvador) |
| WAITFOR_HOST | set name host |
| WAITFOR_PORT | set port for WAITFOR_HOST |
| MIGRATIONS | Enable artisan migrations. 1/0 |
| ARTISAN_COMMAND_OVERRIDE | Override artisan command execute when migration is enabled |
| PHP_MEMORY_LIMIT | Set memory limit in PHP (Example: 512M) - Default value -1(no limit) - Only in fpm images |
| FPM_MAX_CHILDREN | Set max concurrent clients fpm (Example: 250) - Default value 50 - Only in fpm images |
| RUN_USER_ID | Start cointainer with specific userid - Only in fpm images |
| FPM_ENABLE_PROMETHEUS | Enable FPM Prometheus metrics  Values: 0 (Disable) - 1 (Enable) - Only in fpm images|
| NGINX_ENABLE_PROMETHEUS | Enable NGINX Prometheus metrics  Values: 0 (Disable) - 1 (Enable) - Only in fpm images|


##### Apache images tuning
| Environment | Details |
| ------ | ------ |
| MAX_REQUEST_WORKER | Set max concurrent clients in Apache (Example: 500) - Default value 250 - Only in fpm images |
| SERVER_LIMIT | Set number server limit in Apache (Example: 20) - Default value 16 - Only in fpm images |

##### Nginx images tuning
| Environment | Details |
| ------ | ------ |
| NGINX_WORKER_CONNECTION | Set max concurrent clients in Nginx (Example: 500) - Default value 1024 - Only in fpm images |
| NGINX_WORKER_PROCESSES | Set number process in Nginx (Example: 20) - Default value 1 - Only in fpm images |


### Build Environments 

| Environment | Details |
| ------ | ------ |
| NEXUS_COMPOSER_REPO | Url private composer repository |
| COMPOSER_SELF_UPDATE | Set composer self-update --1 before run composer command. Default use composer v2 - Values: 1(Enabled), 0(Disabled) - Default 0 |
| EXTRA_COMPOSER_COMMAND | Run extra composer command after install dependencies process |
| COMPOSER_AUTOLOAD_OPTMIZATION | Run composer "composer install --optimize-autoloader --no-dev" in build process. 1(Enabled), 0(Disabled) - Default 0 |
| COMPOSER_VALIDATE_LOCK_DISABLE | Disable validation composer.lock file and auto composer update - Values: 1(Disable), 0(Disabled) - Default 0 |
| OVERRIDE_COMPOSER_COMMAND | Override default composer command execute in build process |




### Generate builder image

```console

Example build php80 fpm nginx alpine

 docker build -t s2i-php:80-fpm-nginx -f php80-fpm/Dockerfile.nginx.alpine contrib

```

### Php application image use s2i

```console

s2i build https://github.com/my_phpapp.git s2i-php:71 myphp_app:latest --incremental

```


### Run application

```console

docker run -p 8080:8080 myphp_app:latest

```

### How use s2i

```console

https://github.com/openshift/source-to-image

```

License
----

Martin vilche

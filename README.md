# PHP S2i Images Alpine / RockyLinux / AlmaLinux / CentOS

---
**NOTE**

Attention: CentOS images will be deprecated on June 30, 2024 (EOL CentOS 7)

---

https://github.com/openshift/source-to-image

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
- Composer Nexus private repository
- Composer Version 2.3.9
- Php-fpm + Apache Images
- Php-fpm + Nginx Images
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
| PHP_MEMORY_LIMIT | Set memory limit in PHP (Example: 512M) - Default value -1 (no limit) |
| FPM_MAX_CHILDREN | Set max concurrent clients fpm (Example: 250) - Default value 50 |
| RUN_USER_ID | Start cointainer with specific userid - Only in fpm images |
| FPM_ENABLE_PROMETHEUS | Enable FPM Prometheus metrics  Values: 0 (Disable) - 1 (Enable) |
| NGINX_ENABLE_PROMETHEUS | Enable NGINX Prometheus metrics  Values: 0 (Disable) - 1 (Enable) |
| DISABLE_AUTODETECT_DOCROOT_FOLDER | Disable autodetect and set Docroot folder - 1 (Disable) |


##### Apache images tuning
| Environment | Details |
| ------ | ------ |
| MAX_REQUEST_WORKER | Set max concurrent clients in Apache (Example: 500) - Default value 250 |
| SERVER_LIMIT | Set number server limit in Apache (Example: 20) - Default value 16 |

##### Nginx images tuning
| Environment | Details |
| ------ | ------ |
| NGINX_WORKER_CONNECTION | Set max concurrent clients in Nginx (Example: 500) - Default value 1024  |
| NGINX_WORKER_PROCESSES | Set number process in Nginx (Example: 20) - Default value 1  |


### Build Environments 

| Environment | Details |
| ------ | ------ |
| NEXUS_COMPOSER_REPO | Url private composer repository |
| NEXUS_COMPOSER_REPO_ENABLE_TLS | Enable certificate tls validation for NEXUS_COMPOSER_REPO |
| COMPOSER_VERSION_USE | Set composer version used in build. Example 2.2.0 |
| EXTRA_COMPOSER_COMMAND | Run extra composer command after install dependencies process |
| COMPOSER_AUTOLOAD_OPTMIZATION | Run composer "composer install --optimize-autoloader --no-dev -vvv --no-scripts" in build process. 1(Enable), 0(Disable) - Default 0 |
| COMPOSER_VALIDATE_LOCK | Enable validation composer.lock file and auto composer update - Values: 1(Enable), 0(Disable) - Default 0 |
| OVERRIDE_COMPOSER_COMMAND | Override default composer command execute in build process. Default command: "composer install -vvv --no-scripts" |


### User

| udi | gid |
| ------ | ------ |
| 2190 | 0 |


### Ports

| service | port |
| ------ | ------ |
| Apache images | 8080 |
| Nginx images | 8080 |
| Fpm prometheus metrics | 9253 |
| Nginx prometheus metrics | 9113 |


### Generate builder image

```console

Example build php80 fpm Nginx Alpine

 docker build -t s2i-php:80-fpm-nginx -f php80-fpm/Dockerfile.nginx.alpine .

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

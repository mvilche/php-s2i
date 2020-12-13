# PHP S2I Images CentOS  / Alpine

![Docker Stars](https://img.shields.io/docker/stars/mvilche/php-s2i.svg)
![Docker Pulls](https://img.shields.io/docker/pulls/mvilche/php-s2i.svg)
![Docker Automated](https://img.shields.io/docker/cloud/automated/mvilche/php-s2i)
![Docker Build](https://img.shields.io/docker/cloud/build/mvilche/php-s2i)


# Features

- Non-root
- Openshift ready!
- S2i build images
- Composer
- Artisan Migrations
- Composer nexus private repository

### Deploy Environments 


| Environment | Details |
| ------ | ------ |
| TIMEZONE | Set Timezone (America/Montevideo, America/El_salvador) |
| WAITFOR_HOST | set name host |
| WAITFOR_PORT | set port for WAITFOR_HOST |
| MIGRATIONS | Enable artisan migrations. 1/0  |
| ARTISAN_COMMAND_OVERRIDE | Override artisan command execute when migration is enabled |


### Build Environments 

! Sonarqube only include in CentOS Images

| Environment | Details |
| ------ | ------ |
| NEXUS_COMPOSER_REPO | Url private composer repository  |






### How use in Openshift

```console

oc process -f https://raw.githubusercontent.com/mvilche/php-s2i-openshift/master/php-s2i-template-openshift.yaml \ 
-p PHP_VERSION=php72 \
-p APP_NAME=miapp \ 
-p APP_REPO=https://github.com/myuser/php-composer-sample-app.git | oc create -f -

```

### Use configmap for database connection using composer env file

```yaml

apiVersion: v1
data:
  env: |-
    APP_NAME=Lumen
    APP_ENV=local
    APP_KEY=
    APP_DEBUG=true
    APP_URL=http://miapp
    APP_TIMEZONE=America/Montevideo
    LOG_CHANNEL=stack
    LOG_SLACK_WEBHOOK_URL=
    DB_CONNECTION=mysql
    DB_HOST=mysql
    DB_PORT=3306
    DB_DATABASE=database_name
    DB_USERNAME=user
    DB_PASSWORD=pass
    CACHE_DRIVER=file
    QUEUE_CONNECTION=sync
kind: ConfigMap
metadata:
  name: composer-env

```



### Generate builder image

```console

 docker build -t s2i-php:71 -f php71/Dockerfile.centos8 php71

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

### How use without s2i

```console

docker run --name my_php -p 8080:8080 -d -v ./source:/var/www/html mvilche/php-s2i:73-alpine 

```

License
----

Martin vilche

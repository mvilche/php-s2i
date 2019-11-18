# PHP S2I Images 
[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)


# Features

- Non-root
- Openshift compatible
- S2i build images
- Composer
- Artisan Migrations

### Environments


| Environment | Details |
| ------ | ------ |
| TIMEZONE | Set Timezone (America/Montevideo, America/El_salvador) |
| WAITFOR_HOST | set name host |
| WAITFOR_PORT | set port for WAITFOR_HOST |
| MIGRATIONS | Enable artisan migrations. 1/0  |



### How use in Openshift

```console

oc process -f php-s2i-template-openshift.yaml \ 
-p PHP_VERSION=php72 \
-p APP_NAME=miapp \ 
-p APP_REPO=https://github.com/myuser/php-composer-sample-app.git | oc create -f -

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

License
----

Martin vilche

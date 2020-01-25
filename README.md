# PHP S2I Images CentOS 8
[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)


# Features

- Non-root
- Openshift compatible
- S2i build images
- Composer
- Artisan Migrations
- Sonarqube Scanner

### Environments


| Environment | Details |
| ------ | ------ |
| TIMEZONE | Set Timezone (America/Montevideo, America/El_salvador) |
| WAITFOR_HOST | set name host |
| WAITFOR_PORT | set port for WAITFOR_HOST |
| MIGRATIONS | Enable artisan migrations. 1/0  |
| SONARQUBE_ENABLED | Enable sonarqube scanner. 1/0  |
| SONARQUBE_HOST | Sonarqube host. http://sonar.domain:9090  |
| SONARQUBE_PROJECTNAME | Sonarqube project name  |
| SONARQUBE_PROJECTKEY | Sonarqube project key  |



### How use in Openshift

```console

oc process -f php-s2i-template-openshift.yaml \ 
-p PHP_VERSION=php72 \
-p APP_NAME=miapp \ 
-p APP_REPO=https://github.com/myuser/php-composer-sample-app.git | oc create -f -

```



### Use configmap for database connection using composer env file

##### Create a file yaml composer-env.yaml with keyname "env"

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

##### Create configmap in Openshift

```console

 oc create -f composer-env.yaml

```

##### IMPORTANT: Mount configmap in Pod /opt/composer_env




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

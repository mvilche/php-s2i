# PHP s2i images 
[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)


# Funcionalidades:

- Non-root
- Openshift compatible
- S2i build images
- Composer
- Artisan Migrations

### Variables


| Variable | Detalle |
| ------ | ------ |
| TIMEZONE | Define la zona horaria a utilizar (America/Montevideo, America/El_salvador) |
| WAITFOR_HOST | Especifica el host que debe estar activo antes de iniciar el servicio |
| WAITFOR_PORT | Especifica el puerto del servicio WAITFOR_HOST |
| MIGRATIONS | Activa las migraciones con artisan. true/false  |


### Generate builder image

```console

 docker build -t s2i-php:71 -f php71/Dockerfile.centos8 php71

```

### Generate application image using s2i

```console

s2i builder https://github.com/my_phpapp.git s2i-php:71 myphp_app:latest --incremental

```


### How using s2i

```console

https://github.com/openshift/source-to-image

```

License
----

Martin vilche

# PHP 5.6 para desarrollo

Esta imagen, si bien puede usarse para ambientes de producción, se puede
utilizar como una abstracción para desarrollo.

Para utilizarla en desarrollo, se proveen dos shell scripts en el direcrtorio
`bin/`:

* php: wrapper del comando php-cli con los modulos provistos por la actual
  imagen
* php-server: wrapper de un apache con soporte de php y los módulos contemplados
  por esta imagen

Si analizamos estos scripts, bservaremos que necesitamos setear algunas
variables de ambiente que serían de gran utilidad. A decir:

## Configuración de wrapper php-cli

Es posible definir las siguientes variables:

* **PHP_CLI_DOCKER_IMAGE:** nombre de la imagen docker que provee el comando php. Se
  recomienda la provista por este repo `chrodriguez/php-5.6-cli:latest`
* **PHP_OPTIONS:** se inicializa por defecto en `-d
  'date.timezone=America/Argentina/Buenos_Aires' -d memory_limit=512M`, que son
argumentos al comando php que se anexan a cada corrida del comando php
* **PHP_CLI_DOCKER_RUN_OPTIONS**: son opciones usadas al correr el comando
  docker run, que permiten parametrizar determinadas cuestiones. Por defecto,
únicamente agrega en el contenedor que corre, un host con la IP del host
anfitrion, para que si desde el contenedor se desea hacer referencia a algún
servicio que corre en la máquina donde está instalado docker, se haga usando el
nombre del host _local.docker_. El valor por defecto para esta variable es:
`--add-host local.docker:172.17.0.1'`

El único valor mandatorio para el script anterior, es **PHP_CLI_DOCKER_IMAGE**

## Configuración de php-server

Es posible definir las siguientes variables:

* **PHP_SERVER_DOCKER_IMAGE:** nombre de la imagen docker que provee el servicio
  apache con soporte de php. Se recomienda la provista por este repo:
`chrodriguez/php-5.6-apache:latest`
* **PHP_SERVER_DOCKER_RUN_OPTIONS:** opciones usadas al correr el comando
  docker run, que permiten parametrizar determinadas cuestiones. Por defecto,
únicamente agrega en el contenedor que corre, un host con la IP del host
anfitrion, para que si desde el contenedor se desea hacer referencia a algún
servicio que corre en la máquina donde está instalado docker, se haga usando el
nombre del host _local.docker_. El valor por defecto para esta variable es:
`--add-host local.docker:172.17.0.1'`

El único valor mandatorio para el script anterior, es
**PHP_SERVER_DOCKER_IMAGE**. El uso del comando **requiere especificar un puerto
donde escuchará el servidor apache provisto**

Un valor aconsejable para setear **PHP_SERVER_DOCKER_RUN_OPTIONS** es una
personalización del php.ini como por ejempo, utilizar los siguientes valores en
esta variable:

```bash
PHP_SERVER_DOCKER_RUN_OPTIONS="--add-host local.docker:172.17.0.1 -e APACHE_RUN_USER=`id -n` -e APACHE_RUN_GROUP=`id -gn` -v $HOME/.php/php.ini:/usr/local/etc/php/conf.d/custom.ini:ro"
 ```

El comando anterior indica que el usuario con el que correrá apache es el mismo
usuario que corre el comando php-server, al igual que su grupo. Además, utiliza
algunos valores del php.ini que son seteados por el usuario desde un archivo en
su `$HOME/.php/php.ini`, donde por ejemplo puede cambiar el tamaño de upload de
archivos.
El comando anterior, publicará al directorio actual donde se corra el comando
como **DocumentRoot** de apache



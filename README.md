# Wikot Centos Development Environment

Este set de scripts permitirán utilizar, de manera contenida, una máquina virtual
que cuenta las mismas herramientas y configuraciones que se utilizan en los
ambientes de desarrollo de wikot. El Sistema opertivo actualmente es CentOS 7

## Servicios

1. Usuario de control con sudo
2. Bloqueo de acceso remoto a root
3. Nginx 1.8
4. MySQL Server 5.6
5. Mongo DB 3.0

## Herramientas

+ PHP 5.5 + php-fpm + composer + imagemagick
+ Ruby 2.0 + Gems + Sass
+ Node 0.10.36 + NPM + forever + yeoman

+ phpMyAdmin: [http://localhost:8980/](http://localhost:8980/)
+ Mongo-express: [http://localhost:8981/](http://localhost:8981/)

## Utilidades

+ wget
+ nano
+ tree
+ network tools
+ bzip2
+ git
+ htop

## Otros

+ El sistema tiene el firewall deshabilitado

## Requerimientos

+ [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
+ [Vagrant](https://www.vagrantup.com/downloads.html)

## Instalación

1. Instalar VirtualBox
2. Instalar Vagrant
3. Clonar el repositorio en un directorio directamente dentro del directorio que contiene la raiz de los proyectos: ```clone git@code.wktapp.com:devops/wikot-centos-development-environment.git stack```
4. Ingresar al directorio donde clonaron el repositorio: ```cd stack```
5. Ejecutar ```vagrant up```
6. Ejecutar ```./restart stack``` (Los que tengan windows tendrán que ingresar con las instrucciones de más abajo y reiniciar cada servicio con ```systemctl restart nginx && systemctl restart php-fpm && systemctl restart mysqld```. Todavía falta algún script que ayude a esto.)
7. Navegar a http://localhost:8080/ para verificar su funcionamiento

# Redireccionamiento de puertos

+ Modificar el archivo Vagrantfile añadiendo una línea del tipo ```config.vm.network "forwarded_port", guest: 8980, host: 8980```, donde guest es el puerto de la máquina virtual (ej. puerto en el que escucha node) y host es el puerto en la computadora que se está utilizando. Para facilitar al programador, utilizar el mismo puerto en ambos.
+ Recargar la máquina con ```vagrant reload```

# Datos de MySQL

Los datos de MySQL viven dentro de la máquina. Es decir, si la máquina es reinicializada (```vagrant destroy && vagrant up```) estos datos se perderán. Realizar un backup utilizando phpMyAdmin en caso de querer mantener los datos tras una reinicialización o actualización.

(Tarea por hacer, backup de datos antes de actualizar la máquina)


# ./etc y ./var

El directorio ./etc contiene una estructura simil a la que se encuentra dentro de la máquina virtual donde podrás ingresar configuraciones de nginx y php.

El directorio ./var también tiene una estructura simil a /var dentro de la máquina virtual. Los servicios se encuentran configurados para dejar sus logs en este directorio.

# Ingreso por SSH

## OS X o Linux

Para ingresar a la máquina a realizar configuraciones o levantar servicios (ej: aplicación de node), ejecutar ```vagrant ssh```

## Windows

Debes descargar la aplicación putty y utilizar la siguiente configuración para conectarte a la máquina:

```
host: localhost
user: vagrant
pass: vagrant
puerto: 2222
```

**Recuerda que al reinicializar la máquina (ej: ```vagrant destroy && vagrant up```) se perderán las configuraciones. Por esta razón las configuraciones de proyecto deben ingresarse en los directorios correspondientes dentro de ```./etc/```**



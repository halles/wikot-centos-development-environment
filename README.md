# Wikot Centos Development Environment

Este set de scripts permitirán utilizar de manera contenida una máquina virtual
que contiene las mismas herramientas y configuraciones que se utilizan en los
ambientes de desarrollo de wikot. El Sistema opertivo actualmente es CentOS 7

## Servicios Principales

+ nginx 1.6.2
+ mysql 5.6.2
+ php 5.5.22 a través de php-fpm

## Herramientas

+ phpMyAdmin
+ node, npm (gulp, bower y forever)
+ ruby, gems (sass)
+ composer

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

+ VirtualBox 4.3.26
+ Vagrant

## Instalación

1. Instalar VirtualBox
2. Instalar Vagrant
3. Clonar el repositorio en un directorio directamente dentro del directorio que contiene la raiz de los proyectos: ```clone git@code.wktapp.com:devops/wikot-centos-development-environment.git stack```
4. Ejecutar ```vagrant up```
5. Ejecutar ```./restart stack```
6. Navegar a http://localhost:8080/ para verificar su funcionamiento

# Redireccionamiento de puertos

+ Modificar el archivo Vagrantfile añadiendo una línea del tipo ```config.vm.network "forwarded_port", guest: 8980, host: 8980```, donde guest es el puerto de la máquina virtual (ej. puerto en el que escucha node) y host es el puerto en la computadora que se está utilizando. Para facilitar al programador, utilizar el mismo puerto en ambos.
+ Recargar la máquina con ```vagrant reload```

# Datos de MySQL

Los datos de MySQL viven dentro de la máquina. Es decir, si la máquina es reinicializada (```vagrant destroy && vagrant up```) estos datos se perderán. Realizar un backup utilizando phpMyAdmin en caso de querer mantener los datos tras una reinicialización o actualización.

(Tarea por hacer, backup de datos antes de actualizar la máquina)

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



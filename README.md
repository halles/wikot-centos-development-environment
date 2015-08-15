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

# Requerimientos

## Hardware

La máquina virtual requiere de soporte para virtualización y procesador de 64 bits en la máquina host. Debe verificarse si es que está activado. En general los equipos Apple tienen este soporte activado por defecto. Para otras equipos, revisar la configuración de la BIOS del equipo y en su defecto si es que este soporta algún tipo de virtualización.

## Requerimientos OSX / Linux

+ [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
+ [Vagrant](https://www.vagrantup.com/downloads.html)

Además, se requerirán una serie de plugins para la ejecución de la máquina. El Vagrantfile entregará las alertas necesarias en caso de no estar instalados. Alternativamente puedes instalarlos directamente aquí:

```
$ vagrant plugin install vagrant-bindfs
$ vagrant plugin install vagrant-vbguest
```

### Requerimientos Adicionales para Windows

Adicionalmente, para poder correr la máquina en Windows, se requiere un tercer plugin:

```
$ vagrant plugin install vagrant-winnfsd
```

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


# Directorios Compartidos con la máquina host

## /sites

La máquina por defecto montará el directorio padre al local ```../``` en ```/sites``` dentro de la máquina virtual. En caso de que hubiesemos clonado la herramienta en el directorio Sites/wcde dentro de nuestro usuario, la estructura sería la siguiente:

```
| Host            | Guest        |
===================================
/home             |              |
  ∟ /user         |              |
    ∟ /Sites      | /sites       |
      ∟ /Alpha    | ∟ /Alpha     |
      ∟ /Kotex    | ∟ /Kotex     |
	    ∟ /wcde     | ∟ /wcde      |
```

## /vagrant

El directorio /vagrant está directamente montado en donde recide la máquina virtual. Los servicios están configurados para tomar configuraciones de nginx y php desde este directorio, además de entregar logs a estas rutas.

Es escencial tomar en cuenta las rutas que se ingresen en las configuraciones respecto a los archivos que residen en /sites.

### ./etc y ./var

El directorio ./etc contiene una estructura simil a la que se encuentra dentro de la máquina virtual donde podrás ingresar configuraciones de nginx y php.

El directorio ./var también tiene una estructura simil a /var dentro de la máquina virtual. Los servicios se encuentran configurados para dejar sus logs en este directorio.

# Ingreso por SSH

## OS X o Linux

Para ingresar a la máquina a realizar configuraciones o levantar servicios (ej: aplicación de node), ejecutar ```vagrant ssh```

## Windows

La instalación de git provee bash para realizar conexiones por ssh hacia la máuina virtual usando los mismos comandos que se utilizarían en linux, incluyendo el poder reiniciar los servicios desde fuera de la máquina.

Alternativamente puedes usar putty u otra utilidad, usando los siguientes datos:


```
host: localhost
puerto: 2222
user: vagrant
pass: vagrant
```

**Recuerda que al reinicializar la máquina (ej: ```vagrant destroy && vagrant up```) se perderán las configuraciones. Por esta razón las configuraciones de proyecto deben ingresarse en los directorios correspondientes dentro de ```./etc/```**



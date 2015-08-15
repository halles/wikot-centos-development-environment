# Bugs

## v0.0.20


### nginx con archivo por defecto incorrecto

Archivo de configuración de nginx por defecto genera conflictos con nginx. Se requiere eliminar el archivo usando desde la consola del guest antes de iniciar nginx.

```
sudo rm /etc/nginx/conf.d/default.conf
```

### mysqld con nuevas restricciones

El archivo de configuración de MySQL incluye una nueva restricción desde 5.6. Esto a veces genera problemas con las fechas ingresadas en los campos tipo ```datetime```. Si se llegan a tener problemas en la inserción de datos, comprobar que su ORM es incapaz de manejar estos formatos y modificar la siguiente opción en su archivo ```/etc/my.cnf```.

```
sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES # Eliminar STRICT_TRANS_TABLES
sql_mode=NO_ENGINE_SUBSTITUTION
```
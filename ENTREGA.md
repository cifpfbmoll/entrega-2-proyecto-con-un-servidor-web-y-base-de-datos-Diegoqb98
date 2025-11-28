# Entrega Proyecto Docker - Servidor Web y Base de Datos

## Instrucciones para ejecutar el proyecto

### Servidor Web
El contenedor web está corriendo en el puerto 8000. Puedes acceder a:
- **index.html**: http://localhost:8000/index.html
- **index.php**: http://localhost:8000/index.php

### Base de Datos
El contenedor de MariaDB está corriendo en el puerto 3336 con las siguientes credenciales:
- **Host**: localhost
- **Puerto**: 3336
- **Usuario root**: root / root
- **Usuario creado**: invitado / invitado
- **Base de datos**: prueba

### Comandos útiles

#### Ver contenedores en ejecución
```bash
docker ps
```

#### Ver tamaño del contenedor web
```bash
docker ps -s --filter name=web
```

#### Conectar a la base de datos desde el host
```bash
mysql -h 127.0.0.1 -P 3336 -u invitado -p
# password: invitado
```

#### Ver las bases de datos
```sql
SHOW DATABASES;
```

#### Intentar borrar la imagen de MariaDB (fallará mientras el contenedor exista)
```bash
docker rmi mariadb
```

#### Detener contenedores
```bash
docker stop web bbdd
```

#### Eliminar contenedores
```bash
docker rm web bbdd
```

#### Reiniciar contenedores
```bash
docker start web bbdd
```

## Pantallazos requeridos

Para completar la entrega, necesitas tomar los siguientes pantallazos:

1. **Navegador mostrando index.html** - Accede a http://localhost:8000/index.html
2. **Navegador mostrando index.php** - Accede a http://localhost:8000/index.php
3. **Tamaño del contenedor web** - Ejecuta `docker ps -s --filter name=web`
4. **Conexión a la base de datos** - Conecta con un cliente MySQL/MariaDB desde tu PC y ejecuta `SHOW DATABASES;`
5. **Error al intentar borrar imagen MariaDB** - Ejecuta `docker rmi mariadb` mientras el contenedor bbdd existe

## Estado actual del proyecto

✅ Todas las imágenes descargadas:
- ubuntu:24.04
- httpd
- tomcat:9.0.39-jdk11
- jenkins/jenkins:lts
- php:7.4-apache
- mariadb

✅ Contenedor 'web' creado y funcionando en puerto 8000
✅ Archivos index.html e index.php creados en /var/www/html
✅ Contenedor 'bbdd' creado y funcionando en puerto 3336
✅ Variables de entorno configuradas correctamente en MariaDB

El proyecto está completamente configurado y listo para usar.

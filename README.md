# Proyecto con un servidor web y un servidor de base de datos

## 📋 Descripción del Proyecto

Este proyecto implementa un entorno Docker con dos servicios principales:
- **Servidor Web**: Apache con PHP 7.4
- **Base de Datos**: MariaDB

El objetivo es practicar el uso de Docker para crear, configurar y gestionar contenedores, así como la transferencia de archivos entre el host y los contenedores.

---

## 🎯 Objetivos de Aprendizaje

### Ejercicios para repasar

1. Descarga las siguientes imágenes: `ubuntu:24.04`, `httpd`, `tomcat:9.0.39-jdk11`, `jenkins/jenkins:lts`, `php:7.4-apache`.
2. Muestras las imágenes que tienes descargadas.
3. Crea un contenedor demonio con la imagen `php:7.4-apache`.
4. Comprueba el tamaño del contenedor en el disco duro.
5. Con la instrucción `docker cp` podemos copiar ficheros a o desde un contenedor. Puedes encontrar información es esta [página](https://docs.docker.com/engine/reference/commandline/cp/). 
6. Vuelve a comprobar el espacio ocupado por el contenedor.
7. Accede al fichero `info.php` desde un navegador web.

---

## 🚀 Implementación del Proyecto

### 1️⃣ Servidor Web PHP

El servidor web debe cumplir los siguientes requisitos:

- **Imagen**: `php:7.4-apache`
- **Nombre del contenedor**: `web`
- **Puerto**: 8000 (host) → 80 (contenedor)
- **Archivos requeridos**:
  - `index.html`: Página de bienvenida personalizada
  - `index.php`: Script que muestra información de PHP

#### Comando para crear el contenedor:
```bash
docker run -d --name web -p 8000:80 php:7.4-apache
```

#### Crear y copiar archivos:

**Opción 1: Usando `docker cp` (Recomendado)**
```bash
# Crear index.html localmente
echo "<h1>Hola soy Diego de IFC33X</h1>" > index.html

# Crear index.php localmente
echo "<?php echo phpinfo(); ?>" > index.php

# Copiar archivos al contenedor
docker cp index.html web:/var/www/html/
docker cp index.php web:/var/www/html/
```

**Opción 2: Usando `docker exec` con echo**
```bash
docker exec web bash -c "echo '<h1>Hola soy Diego de IFC33X</h1>' > /var/www/html/index.html"
docker exec web bash -c "echo '<?php echo phpinfo(); ?>' > /var/www/html/index.php"
```

**Opción 3: Ejecutando bash interactivo**
```bash
docker exec -it web bash
cd /var/www/html
echo "<h1>Hola soy Diego de IFC33X</h1>" > index.html
echo "<?php echo phpinfo(); ?>" > index.php
exit
```

#### Verificar el tamaño del contenedor:
```bash
docker ps -s --filter name=web
```

#### Acceder desde el navegador:
- **index.html**: http://localhost:8000/index.html
- **index.php**: http://localhost:8000/index.php

---

### 2️⃣ Servidor de Base de Datos MariaDB

El servidor de base de datos debe configurarse con:

- **Imagen**: `mariadb:latest`
- **Nombre del contenedor**: `bbdd`
- **Puerto**: 3336 (host) → 3306 (contenedor)
- **Configuración**:
  - Contraseña de root: `root`
  - Base de datos: `prueba`
  - Usuario: `invitado`
  - Contraseña: `invitado`

#### Comando para crear el contenedor:
```bash
docker run -d --name bbdd \
  -p 3336:3306 \
  -e MYSQL_ROOT_PASSWORD=root \
  -e MYSQL_DATABASE=prueba \
  -e MYSQL_USER=invitado \
  -e MYSQL_PASSWORD=invitado \
  mariadb
```

**En Windows PowerShell (una sola línea):**
```powershell
docker run -d --name bbdd -p 3336:3306 -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=prueba -e MYSQL_USER=invitado -e MYSQL_PASSWORD=invitado mariadb
```

#### Variables de entorno explicadas:
- `MYSQL_ROOT_PASSWORD`: Establece la contraseña del usuario root
- `MYSQL_DATABASE`: Crea automáticamente una base de datos al iniciar
- `MYSQL_USER`: Crea un nuevo usuario
- `MYSQL_PASSWORD`: Establece la contraseña para el usuario creado

#### Conectar desde el host:
```bash
# Usando cliente MySQL/MariaDB
mysql -h 127.0.0.1 -P 3336 -u invitado -p
# Contraseña: invitado

# Ver las bases de datos
SHOW DATABASES;
```

---

## 📦 Gestión de Imágenes y Contenedores

### Descargar todas las imágenes requeridas:
```bash
docker pull ubuntu:24.04
docker pull httpd
docker pull tomcat:9.0.39-jdk11
docker pull jenkins/jenkins:lts
docker pull php:7.4-apache
docker pull mariadb
```

### Ver imágenes descargadas:
```bash
docker images
```

### Ver contenedores en ejecución:
```bash
docker ps
```

### Ver todos los contenedores (incluyendo detenidos):
```bash
docker ps -a
```

### Detener contenedores:
```bash
docker stop web bbdd
```

### Iniciar contenedores:
```bash
docker start web bbdd
```

### Eliminar contenedores:
```bash
docker rm web bbdd
```

### Intentar borrar imagen con contenedor activo:
```bash
docker rmi mariadb
# Esto fallará con un error indicando que la imagen está en uso
```

### Ver logs de un contenedor:
```bash
docker logs web
docker logs bbdd
```

### Inspeccionar contenedor:
```bash
docker inspect web
docker inspect bbdd
```

---

## 📊 Entregables del Proyecto

Para completar la entrega, se requieren los siguientes pantallazos documentados:

### ✅ 1. Navegador mostrando `index.html`
- Acceder a: http://localhost:8000/index.html
- Capturar pantallazo mostrando el mensaje personalizado

### ✅ 2. Navegador mostrando `index.php`
- Acceder a: http://localhost:8000/index.php
- Capturar pantallazo mostrando la información de PHP (phpinfo)

### ✅ 3. Tamaño del contenedor `web`
- Ejecutar: `docker ps -s --filter name=web`
- Capturar pantallazo mostrando el tamaño después de copiar los archivos

### ✅ 4. Conexión a la base de datos desde el host
- Conectar con cliente MySQL/MariaDB instalado en el PC
- Ejecutar: `SHOW DATABASES;`
- Debe mostrar la base de datos `prueba`
- **Importante**: La conexión debe hacerse desde el host, NO con `docker exec`

### ✅ 5. Error al intentar borrar la imagen MariaDB
- Ejecutar: `docker rmi mariadb`
- Capturar el mensaje de error indicando que la imagen está en uso

---

## 🛠️ Comandos Útiles

### Verificar estado de los servicios:
```bash
# Ver contenedores activos
docker ps

# Ver uso de recursos
docker stats web bbdd

# Ver tamaño de contenedores
docker ps -s
```

### Acceso a los contenedores:
```bash
# Acceder al shell del contenedor web
docker exec -it web bash

# Acceder al shell del contenedor bbdd
docker exec -it bbdd bash

# Ejecutar comando en contenedor
docker exec web ls -la /var/www/html
```

### Limpieza del entorno:
```bash
# Detener todos los contenedores
docker stop $(docker ps -q)

# Eliminar contenedores específicos
docker rm -f web bbdd

# Eliminar todas las imágenes no utilizadas
docker image prune -a
```

---

## 📁 Estructura de Archivos del Proyecto

```
entrega-2-proyecto-con-un-servidor-web-y-base-de-datos-Diegoqb98/
│
├── README.md           # Este archivo - Documentación completa
├── ENTREGA.md         # Guía rápida de entrega
├── index.html         # Archivo HTML para el servidor web
└── index.php          # Archivo PHP para el servidor web
```

---

## 🔍 Verificación del Proyecto

### Checklist de verificación:

- [ ] Todas las imágenes descargadas (`docker images`)
- [ ] Contenedor `web` corriendo en puerto 8000
- [ ] Contenedor `bbdd` corriendo en puerto 3336
- [ ] `index.html` accesible en http://localhost:8000/index.html
- [ ] `index.php` accesible en http://localhost:8000/index.php
- [ ] Conexión exitosa a MariaDB desde el host
- [ ] Base de datos `prueba` creada
- [ ] Usuario `invitado` funcional
- [ ] Error al intentar borrar imagen MariaDB

---

## 💡 Troubleshooting

### Problema: Puerto ya en uso
```bash
# Windows
netstat -ano | findstr :8000
# Matar proceso si es necesario

# O cambiar el puerto
docker run -d --name web -p 8001:80 php:7.4-apache
```

### Problema: Contenedor no inicia
```bash
# Ver logs para identificar el error
docker logs web
docker logs bbdd
```

### Problema: No se puede conectar a la base de datos
```bash
# Esperar a que MariaDB inicie completamente (puede tardar unos segundos)
docker logs bbdd

# Verificar que el contenedor está corriendo
docker ps | grep bbdd
```

### Problema: Archivos no aparecen en el navegador
```bash
# Verificar que los archivos existen en el contenedor
docker exec web ls -la /var/www/html/

# Verificar permisos
docker exec web chmod 644 /var/www/html/index.html
docker exec web chmod 644 /var/www/html/index.php
```

---

## 📚 Referencias

- [Documentación oficial de Docker](https://docs.docker.com/)
- [Docker Hub - PHP](https://hub.docker.com/_/php)
- [Docker Hub - MariaDB](https://hub.docker.com/_/mariadb)
- [Comando docker cp](https://docs.docker.com/engine/reference/commandline/cp/)
- [Comando docker exec](https://docs.docker.com/engine/reference/commandline/exec/)

---

## 👤 Autor

**Diego - IFC33X**

Proyecto realizado para la asignatura de Sistemas - Práctica de Docker con servidores web y base de datos.

---

## 📝 Notas Adicionales

- Los contenedores se ejecutan en modo daemon (`-d`) para que funcionen en segundo plano
- El mapeo de puertos permite acceder a los servicios desde el host
- Las variables de entorno en MariaDB automatizan la configuración inicial
- El comando `docker cp` es útil para transferir archivos sin necesidad de volúmenes
- Los contenedores persisten hasta que se eliminan explícitamente con `docker rm`

---

**Fecha de realización**: Noviembre 2025  
**Versión**: 1.0


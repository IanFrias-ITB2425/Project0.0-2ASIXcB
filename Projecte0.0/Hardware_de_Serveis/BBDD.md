# Documentación Técnica: Servidor de Base de Datos (B-NCC)

## 1. Descripción del Servicio
Este documento detalla el despliegue, configuración y puesta en marcha del servidor de Base de Datos (**B-NCC**) para el proyecto de infraestructura P0.0. Este servidor aloja el motor MySQL y almacena los datos de "Equipaments d'educació de Barcelona" requeridos por la aplicación web.

### 1.1. Requisitos Cumplidos
* **Hostname:** `B-NCC`
* **Rol:** Backend de Base de Datos.
* **Usuario de Administración:** `bchecker` / `bchecker121`
* **Datos:** Importación de CSV de OpenData BCN.
* **Red:** Ubicado en la red DMZ (`LAN_SERVEIS`), accesible solo por el Servidor Web.
* **Ssh:** sudo apt install openssh-server

 ![Config Basica](/Projecte0.0/Images/Bchecker.png)
 ![Config Red](/Projecte0.0/Images/ssh.png)
 
---
### 1.2 Configuración de Red
 ![Config Red](/Projecte0.0/Images/Red.png)
---
### 1.3 Configuración de MYSQL
 Primero, actualitzamos los repositorios y instalamos el servidor MySQL
 ![Install SQL](/Projecte0.0/Images/Mysql_install.png)
 
Por defecto, MySQL solo escucha a 127.0.0.1 (localhost). Para permitir que el servidor Web (W-NCC) se connecte, hemos de modificar el archivo de configuración
 ![Conf SQL](/Projecte0.0/Images/mysqlconf.png)

 Descargamos el archivo de datos abiertos de l'Ajuntament de Barcelona a la carpeta temporal para su posterior importación
 ![Conf2 SQL](/Projecte0.0/Images/ImportarCSV.png)

Creamos el usuario bchecker para gestión local.

 ![Conf3 SQL](/Projecte0.0/Images/Usuario_bchecker.png)

Creamos la base de datos para el proyecto y definimos la estructura de la tabla centres_educatius

 ![Conf4 SQL](/Projecte0.0/Images/Creación_BBDD.png)

Habilitamos el acceso para que el servidor web (que tiene la IP 192.168.22.10) pueda conectarse

 ![Conf5 SQL](/Projecte0.0/Images/Usuario_remoto.png)

Una vez tenemos el archivo CSV en el servidor, accedemos a la consola de MySQL (con el usuario `root` o `bchecker` si tiene permisos de FILE) para volcar los datos dentro de la tabla creada.

 ![Conf6 SQL](/Projecte0.0/Images/Importar_CSV_BBDD.png)

Finalmente abrimos el puerto 3306 para la ip de nuestro servidor web y ya estaría hecha la configuración del servidor de base de datos.
 
---

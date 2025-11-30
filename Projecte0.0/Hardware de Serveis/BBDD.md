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
 ![Install SQL](/Projecte0.0/Images/Mysql_install.png)
 Primer, actualitzem els repositoris i instal·lem el servidor MySQL
 ![Conf SQL](/Projecte0.0/Images/mysqlconf.png)
 Per defecte, MySQL només escolta a 127.0.0.1 (localhost). Per permetre que el servidor Web (W-NCC) s'hi connecti, hem de modificar l'arxiu de configuració
 ![Conf2 SQL](/Projecte0.0/Images/ImportarCSV.png)
 Descarreguem el fitxer de dades obertes de l'Ajuntament de Barcelona a la carpeta temporal per a la seva posterior importació 
---

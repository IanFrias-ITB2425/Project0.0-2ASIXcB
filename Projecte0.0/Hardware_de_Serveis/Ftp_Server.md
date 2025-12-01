---
# Configuración del Servidor FTP (F-NCC)

**Host:** F-NCC (F-N02)  
**IP:** 192.168.22.12  
**Servicio:** vsftpd  
**Usuario FTP:** bchecker

## 1. Configuración de Usuario

Creamos el usuario del sistema que se utilizará para la conexión FTP.

sudo useradd -m -s /bin/bash bchecker
sudo passwd bchecker
Password establecido: bchecker121
sudo usermod -aG sudo bchecker

 ![Config Basica](/Projecte0.0/Images/Preparación_usuario.png)
 
## 2. Instalación del Servicio
Instalamos el demonio vsftpd (Very Secure FTP Daemon) y SSH para administración.
![Instalación](/Projecte0.0/Images/Instalación_Servicio.png)

![Instalación](/Projecte0.0/Images/Ssh.png)

![Instalación](/Projecte0.0/Images/Instalación_ServicioFTP.png)

## 3. Configuración de VSFTPD
Editamos el archivo de configuración principal para asegurar el servicio y permitir la escritura.
Hacemos una copia de seguridad y editamos.

![Conf](/Projecte0.0/Images/Conf_FTP.png)

![Conf](/Projecte0.0/Images/Conf1_FTP.png)

![Conf](/Projecte0.0/Images/Conf2_FTP.png)

## 4. Preparación de Directorios
Preparamos la estructura de carpetas dentro del home del usuario y creamos un archivo de prueba

![Prep](/Projecte0.0/Images/Preparación_directorios.png)

## 5. Configuración del Firewall (UFW)
Configuramos el cortafuegos ufw para permitir las conexiones SSH y el tráfico FTP (puertos 20 y 21)

![Firewall](/Projecte0.0/Images/Puertos_FTP.png)

## 6. Configuración de RED
Configuramos el archivo de netplan de la siguiente manera para poder movernos entre nuestro router y redes

![RED](/Projecte0.0/Images/RED_FTP.png)



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
![Instalación](/Projecte0.0/Images/Instalación_ServicioFTP.png)
![Instalación](/Projecte0.0/Images/Ssh.png)

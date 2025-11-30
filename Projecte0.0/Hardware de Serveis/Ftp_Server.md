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
 

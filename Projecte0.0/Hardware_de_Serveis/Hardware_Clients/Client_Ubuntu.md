# Documentación de Despliegue: Cliente Linux (Ubuntu)

## 1. Descripción y Objetivo
Este equipo simula un usuario interno de la organización. Su principal función es validar que el servicio **DHCP** del Router funciona correctamente y que el **enrutamiento** permite el acceso al Servidor Web (W-NCC) en la DMZ.

* **Ubicación:** Red Intranet.
* **Método de Red:** Obtención de IP por **DHCP** (Automático).
* **Rango IP esperado:** 192.168.20.x

---
## 2. Configuración de Red (DHCP)

Se aseguró que la interfaz de red estuviera conectada a la red virtual **net-Intranet** en Isard. La configuración del sistema operativo se ajustó para el modo automático.


### 2.2. Preparación del Sistema (Limpieza de Interfaces y Firewall)
Antes de validar la red, se realizaron acciones preventivas para evitar conflictos de enrutamiento y bloqueos de seguridad durante las pruebas.

**1. Gestión de Interfaces Redundantes:**
Se detectó que el sistema tenía activas interfaces que no estaban conectadas a la Intranet y generaban rutas erróneas.

```bash
sudo ip link set enp1s0 down
sudo ip link set enp2s0 down
sudo ip link set enp3s0 down
```
![Desactivar interfaces ubuntu](/Projecte0.0/Images/desactivar_interfaces_ubuntu.PNG)

**2. Desactivación del Firewall:**
Se desactivó para que no diese problemas en las conexiones

```bash
sudo ufw disable
```
![Desactivar ufw ubuntu](/Projecte0.0/Images/des_ufw_ubuntu.PNG)


### 2.3. Verificación de IP y Gateway

Se confirmó que el cliente recibió correctamente su configuración del Router (R-NCC).

**Comprobación de IP y Gateway:**
```bash
ip a 
ip route
```

![Config Red](/Projecte0.0/Images/ip_asignada_ubuntu.PNG)



---

## 3. Pruebas de Conectividad Funcional

Una vez verificada la configuración de red local, se realizaron pruebas para validar la comunicación a través del Router.

### 3.1. Prueba de Enrutamiento al Servidor Web (W-NCC)
Se ejecutó una prueba de conectividad Ping hacia el Servidor Web. Esta prueba confirma que el Router tiene habilitado el *IP Forwarding* y está enrutando correctamente los paquetes entre la Intranet y la DMZ.

**Comando de verificación:**
```bash
# Ping de 4 paquetes a la IP del Web Server
ping -c 4 192.168.22.10
```
![Ping Ubuntu a web server](/Projecte0.0/Images/ping_cliente_ubutu_web.png
)
### 3.2. Acceso al Servicio Web
Ahora comprovaremos si el cliente pudo acceder a la Web mediante el Router

URL de Acceso: http://192.168.22.10
![Cliente Ubuntu Webserver](/Projecte0.0/Images/cliente_ubuntu_webserver.png)





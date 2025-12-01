# Documentación de Despliegue: Cliente Windows

## 1. Descripción y Objetivo
Este equipo simula un usuario interno de la organización utilizando un entorno Microsoft. Su principal función es validar que el servicio **DHCP** del Router funciona correctamente y que el **enrutamiento** permite el acceso al Servidor Web (W-NCC) en la DMZ desde un sistema operativo distinto a Linux.

* **Ubicación:** Red Intranet.
* **Método de Red:** Obtención de IP por **DHCP**.
* **Rango IP esperado:** 192.168.20.x

---
## 2. Configuración de Red

Se aseguró que la interfaz de red estuviera conectada a la red virtual **net-Intranet** en Isard. La configuración del adaptador de red en Windows se ajustó para "Obtener una dirección IP automáticamente".

### 2.2. Preparación del Sistema (Limpieza de Interfaces y Firewall)
Antes de validar la red, se realizaron acciones preventivas para evitar conflictos de enrutamiento y bloqueos de seguridad durante las pruebas.

**1. Gestión de Interfaces Redundantes:**
Se detectó que el sistema tenía activas interfaces adicionales que no estaban conectadas a la Intranet y podían generar conflictos. Se procedió a deshabilitarlas.

```powershell
Disable-NetAdapter -Name "Ethernet 2"
Disable-NetAdapter -Name "Ethernet 3"
```
**2. Desactivación del Firewall**
Se desactivó el Firewall de Windows Defender para asegurar que no bloquease el tráfico ICMP

```powershell
NetSh Advfirewall set allprofiles state off
```

### 2.3. Verificación de IP y Gateway
Se confirmó que el cliente recibió correctamente su IP asignada

```powershell
ipconfig /all
```

## 3. Pruebas de Conectividad Funcional
Se realizaron pruebas para validar la comunicación a través del Router.

### 3.1. Prueba de Enrutamiento al Servidor Web
Se ejecutó una prueba de conectividad Ping hacia el Servidor Web
**Comando de verificación:**

```powershell
ping 192.168.22.10
```

### 3.2. Acceso al Servicio Web
Ahora comprobaremos si el cliente pudo acceder a la Web mediante el Route

URL de Acceso: http://192.168.22.10


# Documentación de Despliegue: Cliente Linux (Ubuntu)

## 1. Descripción y Objetivo
Este equipo simula un usuario interno de la organización. Su principal función es validar que el servicio **DHCP** del Router funciona correctamente y que el **enrutamiento** permite el acceso al Servidor Web (W-NCC) en la DMZ.

* **Ubicación:** Red Intranet.
* **Método de Red:** Obtención de IP por **DHCP** (Automático).
* **Rango IP esperado:** 192.168.20.x

---
## 2. Configuración de Red (DHCP)

Se aseguró que la interfaz de red estuviera conectada a la red virtual **net-Intranet** en Isard. La configuración del sistema operativo se ajustó para el modo automático.

### 2.1. Verificación de IP y Gateway

Se confirmó que el cliente recibió correctamente su configuración del Router (R-NCC).

**Comprobación de IP y Gateway:**
```bash
ip a       # Mostrará la IP asignada por el DHCP (ej. 192.168.20.100)
ip route   # Mostrará la Puerta de Enlace (192.168.20.1)

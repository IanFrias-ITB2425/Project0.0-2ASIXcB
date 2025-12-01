# Documentación Técnica: Servidor Web (W-NCC)

## 1. Descripción del Servicio
Este documento detalla el despliegue, configuración y puesta en marcha del **Servidor Web (W-NCC)** para el proyecto de infraestructura P0.0. Este servidor actúa como *Frontend* de la arquitectura, alojando el servidor Apache y la aplicación PHP encargada de procesar las peticiones de los clientes y visualizar los datos almacenados en el servidor de Base de Datos.

### 1.1. Información General del Host
* **Hostname:** W-NCC
* **Sistema Operativo:** Ubuntu Server 22.04 LTS
* **IP Asignada:** 192.168.22.10 (Mascara /24)
* **Red:** DMZ (Zona Desmilitarizada)
* **Función:** Servidor Web (Apache + PHP)

---

## 2. Gestión de Usuarios y Accesos
Para cumplir con los requisitos del proyecto y asegurar la auditabilidad del sistema, se procedió a la creación del usuario administrador estandarizado.

### 2.1. Creación del Usuario 'bchecker'
Se creó el usuario `bchecker` y se le asignaron permisos de administración (sudo) para permitir la gestión del servidor.

* **Usuario:** `bchecker`
* **Password:** `bchecker121` (o `pirineus`)
* **Permisos:** Se añadió al grupo `sudo` para tareas administrativas.

**Comandos ejecutados:**
1.  **Creación del usuario:** Se utilizó el comando `useradd` para registrar el usuario en el sistema y crear su directorio personal.
    ```bash
    sudo useradd -m -s /bin/bash bchecker
    ```
    ![Creación del usuario](../Images/creacion_usuario_bchecker_webserver.png)
    
2.  **Asignación de contraseña:** Se estableció la contraseña requerida.
    ```bash
    sudo passwd bchecker
    ```
    ![Asignación de contraseña bchecker](../Images/contrasenya_usuari.PNG)
    
3.  **Permisos de Administrador:** Se añadió el usuario al grupo *sudo* para permitirle ejecutar tareas administrativas.
    ```bash
    sudo usermod -aG sudo bchecker
    ```
    ![Permisos del usuario bchecker](../Images/comprobacion_usuario.PNG)

---

## 3. Configuración de Red e Interfaces
Debido a la arquitectura del proyecto, el servidor Web se ubica en la **DMZ**. Para garantizar la comunicación correcta y evitar conflictos con las interfaces por defecto de la virtualización, se realizó una configuración manual estricta.

### 3.1. Definición de Parámetros (Netplan)
Se editó el archivo de configuración de red para asignar la IP estática y la puerta de enlace (Gateway) hacia el Router del proyecto.

* **Archivo:** `/etc/netplan/00-installer-config.yaml`
* **Interfaz activa:** `enp3s0`

**Código de configuración aplicado:**
```yaml
# Let NetworkManager manage all devices on this system
network:
  version: 2
  ethernets:
    enp3s0:
      addresses:
        - 192.168.22.10/24
      gateway4: 192.168.22.1
      nameservers:
        addresses:
          - 8.8.8.8
```
![Neplan del Web Server](../Images/netplan_webserver.PNG)


### 3.2. Gestión y Limpieza de Interfaces
Durante el despliegue, se detectó que el sistema activaba múltiples interfaces de red (`enp1s0`, `enp2s0`) que generaban **conflictos de enrutamiento** (el tráfico intentaba salir por la interfaz equivocada).

**Solución aplicada:**
Se procedió a deshabilitar manualmente las interfaces redundantes para forzar que todo el tráfico fluyera exclusivamente por la interfaz configurada (`enp3s0`), asegurando así que el tráfico con destino a la Intranet pasara por la ruta correcta.

**Comandos de gestión de interfaces:**
```bash
sudo ip link set enp1s0 down
sudo ip link set enp2s0 down
# Una vez deshabilitadas las interfaces redundantes, se aplicó 'sudo netplan apply'
# para asegurar que la interfaz principal se activara con los parámetros definidos.
```
![Desactivar Interfaces](../Images/desactivar_interfaces.PNG)

### 3.3. Verificación Final
Tras aplicar la configuración (`sudo netplan apply`), se verificó que solo la interfaz correcta tenía IP y conexión, y que la comunicación con el Gateway (Router R-NCC) se estableció correctamente.

**Estado de la IP:**
Se comprobó que la interfaz principal (`enp3s0`) tenía asignada la dirección estática definida (`192.168.22.10/24`).

```bash
ip r
```
![Comprovacion Interficies](../Images/comprobacion_interficies_webserver.PNG)

Prueba de conexión: Se validó la conectividad al Router. Este paso es fundamental para confirmar que la comunicación entre el Servidor Web (DMZ) y el Router funciona.

```bash
ping 192.168.22.1 #Router
ping 192.168.22.11 #BBDD
```

---

## 4. Instalación de Servicios

Para convertir crear el Web Server y poder dar visivilidad a la web y tener conexión con los otros servicios se instaló los siguiente (Apache, PHP y los conectores de Base de Datos).

### 4.1. Actualización de Repositorios (Preparación)
Antes de instalar cualquier paquete, es un paso obligatorio y de buenas prácticas actualizar el índice de paquetes local del sistema para asegurar que se utilicen las versiones más recientes y estables.

```bash
sudo apt update



# Documentación Técnica: Servidor DHCP (D-NCC)

## 1. Descripción del Servicio
Este documento detalla la instalación, configuración y puesta en marcha del **Servidor DHCP (D-NCC)** para el proyecto de infraestructura P0.0.  
El servicio DHCP se encarga de distribuir direcciones IP, gateway, máscara y DNS de manera automática a los hosts de la **Intranet (192.168.20.x)**.

---

## 1.1. Información General del Host

- **Hostname:** D-NCC  
- **Sistema Operativo:** Ubuntu  
- **Servicio Implementado:** isc-dhcp-server  
- **Red Asignada:** INTRANET – 192.168.20.0/24  
- **Función:** Asignación automática de direcciones IP y parámetros de red.

---

## 2. Instalación del Servicio DHCP

### 2.1. Instalación del paquete

```bash
sudo apt install isc-dhcp-server -y
```

### Explicación
Se instala el paquete oficial **isc-dhcp-server**, encargado de gestionar el servicio DHCP en Ubuntu.

---

## 3. Configuración del Servicio DHCP

### 3.1. Archivo `/etc/default/isc-dhcp-server`

```txt
INTERFACESv4="enp4s0"
```

### Explicación
Aquí se define **en qué interfaz** escuchará el servidor DHCP.  
En este caso, `enp4s0` corresponde a la **Intranet (20.x)**.

---

### 3.2. Archivo `/etc/dhcp/dhcpd.conf`

```txt
default-lease-time 600;
max-lease-time 7200;
authoritative;

# --- Intranet ---
subnet 192.168.20.0 netmask 255.255.255.0 {
  range 192.168.20.100 192.168.20.200;
  option routers 192.168.20.1;
  option subnet-mask 255.255.255.0;
  option domain-name-servers 192.168.22.1, 8.8.8.8;
  option domain-name "ncc.local";
}
```

### Explicación
- **default-lease-time:** Duración mínima en segundos del alquiler DHCP.  
- **max-lease-time:** Duración máxima del alquiler.  
- **authoritative:** Indica que este servidor es la autoridad para esta red.  
- **subnet:** Define la red 192.168.20.0/24.  
- **range:** Rango de IPs dinámicas: 100–200.  
- **routers:** Gateway → 192.168.20.1  
- **DNS:** Primero el DNS de la DMZ (192.168.22.1), seguido de Google DNS (8.8.8.8).  
- **domain-name:** Dominio interno de la organización.

---

## 4. Reinicio y Comprobación del Servicio

### 4.1. Reiniciar el servicio

```bash
sudo systemctl restart isc-dhcp-server
```

### 4.2. Verificar el estado

```bash
sudo systemctl status isc-dhcp-server
```

### Comportamiento esperado

Salida típica:

```
Active: active (running)
```

### Explicación
El servicio está funcionando correctamente y asignando direcciones IP en la red Intranet.

---

## 5. Resultado Final

Tras completar la instalación y configuración:

- El servidor DHCP asigna IPs automáticamente a la Intranet.  
- La interfaz `enp4s0` actúa como interfaz de servicio.  
- El gateway, máscara, DNS y dominio se distribuyen correctamente.  
- El servicio permanece activo y habilitado al inicio del sistema.  

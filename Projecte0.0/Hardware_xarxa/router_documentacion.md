# Documentación Técnica: Router Ubuntu (R-NCC)

## 1. Descripción del Servicio
Este documento detalla la configuración, despliegue y puesta en marcha del **Router Ubuntu (R-NCC)** utilizado en el proyecto P0.0.  
Este equipo actúa como **router principal**, interconectando la red WAN con la DMZ (22.x) y la INTRANET (20.x), permitiendo el reenvío de paquetes y proporcionando NAT para acceso a Internet.

---

## 1.1. Información General del Host

- **Hostname:** R-NCC  
- **Sistema Operativo:** Ubuntu Desktop / Server  
- **Redes configuradas:**  
  - **WAN:** DHCP (interface `enp1s0`)  
  - **DMZ:** 192.168.22.1/24 (interface `enp3s0`)  
  - **INTRANET:** 192.168.20.1/24 (interface `enp4s0`)  
- **Función:** Enrutamiento entre redes + NAT

---

## 2. Configuración de Red (Netplan)

### 2.1. Archivo configurado
**Ruta:** `/etc/netplan/01-network-manager-all.yaml`

```yaml
network:
  version: 2
  renderer: networkd

  ethernets:
    enp1s0:
      dhcp4: true        # WAN

    enp3s0:
      addresses:
        - 192.168.22.1/24   # DMZ (22.x)
      dhcp4: false

    enp4s0:
      addresses:
        - 192.168.20.1/24   # INTRANET (20.x)
      dhcp4: false

    enp2s0:
      dhcp4: false
```

### Explicación  
Se definen las interfaces del router:  
- `enp1s0` recibe IP pública mediante DHCP (WAN).  
- `enp3s0` se asigna a la DMZ con IP estática 192.168.22.1/24.  
- `enp4s0` se asigna a la INTRANET con IP estática 192.168.20.1/24.  
- `enp2s0` permanece deshabilitada.  

---

## 3. Activación del Enrutamiento IPv4

### 3.1. Archivo configurado  
**Ruta:** `/etc/sysctl.conf`

```txt
net.ipv4.ip_forward=1
```

### Explicación  
Esta línea habilita el **reenvío de paquetes IPv4**, necesario para que Ubuntu funcione como router.

### 3.2. Aplicación inmediata del cambio

```bash
sudo sysctl -p
```

---

## 4. Configuración de NAT

### 4.1. Creación de la regla MASQUERADE

```bash
sudo iptables -t nat -A POSTROUTING -o enp1s0 -j MASQUERADE
```

### Explicación  
- Se habilita NAT para que las redes internas (DMZ e INTRANET) salgan a Internet usando la interfaz WAN.  
- `MASQUERADE` reemplaza la IP interna por la IP pública del router.

---

### 4.2. Comprobación de la tabla NAT

```bash
sudo iptables -t nat -L -n -v
```

Salida esperada:

```
Chain POSTROUTING (policy ACCEPT)
MASQUERADE  all  --  *  enp1s0  0.0.0.0/0  0.0.0.0/0
```

### Explicación  
La regla MASQUERADE aparece correctamente activa.

---

## 5. Persistencia de las reglas iptables

### 5.1. Instalación del paquete

```bash
sudo apt install iptables-persistent -y
```

### 5.2. Guardado de reglas  
Durante la instalación, seleccionar:

**"¿Desea guardar las reglas de IPv4 actuales?" → Sí**

### Explicación  
Esto guarda las reglas en `/etc/iptables/rules.v4`, garantizando que el NAT se mantenga tras reiniciar.

---

## 6. Configuración de DNS

### 6.1. Archivo configurado  
**Ruta:** `/etc/systemd/resolved.conf`

```
[Resolve]
DNS=8.8.8.8 1.1.1.1
FallbackDNS=9.9.9.9
```

### Explicación  
El router utilizará servidores DNS públicos confiables (Google, Cloudflare, Quad9).

---

## 7. Resultado Final

Tras aplicar toda la configuración, el router Ubuntu queda operativo con:

- Enrutamiento IPv4 activado  
- NAT funcional y persistente  
- Interfaces WAN, DMZ e INTRANET correctamente configuradas  
- DNS funcionando adecuadamente  
- Comportamiento completo de router para el proyecto P0.0

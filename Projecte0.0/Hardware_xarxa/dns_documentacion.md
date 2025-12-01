# Documentación Técnica: Servidor DNS (NS-NCC)

## 1. Descripción del Servicio
Este documento detalla la instalación, configuración y puesta en marcha del **Servidor DNS (NS-NCC)** para el proyecto de infraestructura P0.0.  
El servidor DNS se encarga de resolver nombres de dominio internos del dominio **ncc.local** y de reenviar consultas externas a servidores DNS públicos.

---

## 1.1. Información General del Host

- **Hostname:** NS-NCC  
- **Sistema Operativo:** Ubuntu  
- **Servicio Implementado:** BIND9  
- **Dominio interno:** ncc.local  
- **Función:** Resolución de nombres interna y reenvío de consultas externas.

---

## 2. Instalación del Servicio DNS

### 2.1. Instalación de paquetes

```bash
sudo apt install bind9 bind9utils -y
```

### Explicación
Se instalan los paquetes necesarios para el servidor DNS:
- **bind9**: servicio principal de DNS.
- **bind9utils**: utilidades adicionales para gestión y diagnóstico.

---

## 3. Configuración de la Zona DNS

### 3.1. Definición de la zona en `named.conf.local`

**Archivo:** `/etc/bind/named.conf.local`

```txt
zone "ncc.local" {
    type master;
    file "/etc/bind/db.ncc.local";
};
```

### Explicación
- Se define la zona **ncc.local** como **zona maestra**.
- Se indica el fichero de base de datos asociado: `/etc/bind/db.ncc.local`.

---

### 3.2. Creación del archivo de zona

```bash
sudo cp /etc/bind/db.local /etc/bind/db.ncc.local
sudo nano /etc/bind/db.ncc.local
```

### Explicación
- Se copia el archivo de ejemplo `db.local` para usarlo como plantilla.
- A continuación, se edita el fichero nuevo para adaptarlo al dominio **ncc.local**.

---

### 3.3. Contenido del archivo de zona `db.ncc.local`

**Archivo:** `/etc/bind/db.ncc.local`

```txt
$TTL    604800
@       IN      SOA     ns.ncc.local. root.ncc.local. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL

; --- Servidors de la xarxa ---
@       IN      NS      ns.ncc.local.
ns      IN      A       192.168.20.1    ; R-NCC (servidor DNS)

; --- Hosts principals ---
r-ncc   IN      A       192.168.22.1    ; router
w-ncc   IN      A       192.168.22.10   ; servidor web
b-ncc   IN      A       192.168.22.20   ; base de dades / ftp

; Alias per comoditat
www     IN      CNAME   w-ncc
bbdd    IN      CNAME   b-ncc
```

### Explicación
- Se configura el **SOA** (Start of Authority) con el servidor `ns.ncc.local` y el correo de administración `root.ncc.local`.
- Se declara el servidor de nombres principal:
  - Registro **NS** para `ns.ncc.local`.
  - Registro **A** que apunta `ns` a la IP 192.168.20.1 (router/servidor DNS).
- Se definen los hosts principales del CPD:
  - `r-ncc` → router.
  - `w-ncc` → servidor web.
  - `b-ncc` → servidor de base de datos/FTP.
- Se crean alias (CNAME) para facilitar el acceso:
  - `www.ncc.local` → `w-ncc.ncc.local`.
  - `bbdd.ncc.local` → `b-ncc.ncc.local`.

---

## 4. Configuración General de BIND

### 4.1. Archivo `named.conf.options`

**Archivo:** `/etc/bind/named.conf.options`

```txt
options {
    directory "/var/cache/bind";

    recursion yes;
    allow-query { any; };
    listen-on { any; };
    listen-on-v6 { any; };

    forwarders {
        8.8.8.8;
        1.1.1.1;
    };

    dnssec-validation auto;
};
```

### Explicación
- **directory**: ruta de los archivos de caché de BIND.
- **recursion yes**: permite resolver nombres externos (DNS recursivo).
- **allow-query { any; };** permite consultas desde cualquier host de la red.
- **listen-on / listen-on-v6**: el servicio escucha en todas las interfaces IPv4 e IPv6.
- **forwarders**: consultas que el servidor no pueda resolver se enviarán a:
  - 8.8.8.8 (Google DNS)
  - 1.1.1.1 (Cloudflare DNS)
- **dnssec-validation auto**: habilita la validación DNSSEC cuando procede.

---

## 5. Reinicio y Verificación del Servicio DNS

### 5.1. Reinicio del servicio BIND9

```bash
sudo systemctl restart bind9
```

### 5.2. Comprobación del estado

```bash
sudo systemctl status bind9
```

El servicio debe aparecer como:

```
Active: active (running)
```

---

## 6. Pruebas de Resolución DNS

### 6.1. Resolución de un dominio externo

```bash
dig @127.0.0.1 google.com
```

### Resultado esperado
- El servidor DNS responde con la IP pública de `google.com`.
- Se confirma que:
  - La resolución recursiva funciona.
  - Los **forwarders** están operativos.

---

### 6.2. Resolución de un dominio interno

```bash
dig @127.0.0.1 w-ncc.ncc.local
```

### Resultado esperado
- El servidor devuelve la dirección IP interna configurada para `w-ncc`.
- Se verifica que la zona **ncc.local** está correctamente definida y servida por BIND.

---

## 7. Resultado Final

Tras completar la instalación y configuración:

- El servidor BIND9 resuelve correctamente nombres del dominio interno **ncc.local**.  
- Las consultas externas se reenvían a DNS públicos (8.8.8.8 y 1.1.1.1).  
- El servicio está activo y escuchando en todas las interfaces.  
- La infraestructura dispone de un **servidor DNS centralizado** para el CPD.

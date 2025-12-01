# P0.0 - Despliegue de Infraestructura (ASIX)

## 📋 Descripción del Proyecto
Este proyecto forma parte del **Módulo 0379 - Proyecto intermodular de administración de sistemas informáticos en red** del Institut Tecnològic Barcelona.

El objetivo principal es preparar y desplegar la infraestructura necesaria para soportar una aplicación multicapa. El proyecto tiene una duración prevista de 6 semanas y se gestiona mediante una metodología ágil dividida en 3 sprints quincenales.

### 🎯 Objetivos Específicos
* Despliegue de servidores de red y servicios (Web, BBDD, DNS, DHCP, FTP, SSH).
* Configuración de una topología de red segura (DMZ, Intranet, NAT).
* Implementación de una aplicación de prueba que visualice datos de equipamientos educativos de Barcelona.
* Documentación y control de versiones mediante Git/GitHub.

---

## 🏗️ Arquitectura de Red e Infraestructura

Se debe diseñar un diagrama de arquitectura que refleje la siguiente topología de red:

![Diagrama de Arquitectura](/Projecte0.0/Images/Esquema_xarxa.png)
*(Asegúrate de subir tu diagrama y actualizar este enlace)*

### Hardware de Red
* **Router (R-NCC):** Encargado de gestionar el tráfico entre tres zonas de red diferenciadas:
    * **DMZ:** Zona desmilitarizada para servicios expuestos.
    * **Intranet:** Red interna segura.
    * **NAT:** Salida a internet.

### Clientes
Para verificar el funcionamiento, la infraestructura cuenta con 2 clientes:
* 1 Cliente Windows. | [📄 Ver README Client Windows](Projecte0.0/Hardware_de_Serveis/Hardware_Clients/Client_Windows.md) |
* 1 Cliente Linux. | [📄 Ver README Client Ubuntu](Projecte0.0/Hardware_de_Serveis/Hardware_Clients/Client_Ubuntu.md) |

---

## 🚀 Servicios Desplegados

A continuación se detallan los servicios que componen la infraestructura. Haz clic en cada servicio para ver su configuración específica y documentación técnica:

| Servicio / Hostname | Descripción Breve | Enlace a Documentación |
| :--- | :--- | :--- |
| **Router (R-NCC)** | Configuración de enrutamiento, NAT y Firewall. | [📄 Ver README Router](Projecte0.0/Hardware_xarxa/router_documentacion.md) |
| **Web Server (W-NCC)** | Servidor Web para alojar la aplicación multicapa. | [📄 Ver README Web](Projecte0.0/Hardware_de_Serveis/WebServer.md) |
| **Base de Datos (B-NCC)** | MySQL con datos de OpenData BCN cargados. | [📄 Ver README BBDD](Projecte0.0/Hardware_de_Serveis/BBDD.md) |
| **DNS** | Resolución de nombres (incluyendo R-NCC). | [📄 Ver README DNS](Projecte0.0/Hardware_xarxa/dns_documentacion.md) |
| **FTP (F-NCC)** | Servidor de transferencia de archivos. | [📄 Ver README FTP](Projecte0.0/Hardware_de_Serveis/Ftp_Server.md) |
| **DHCP** | asignación de IPs. | [📄 Ver README DHCP](Projecte0.0/Hardware_xarxa/dhcp_documentacion.md) |
---

## 🔐 Requisitos de Acceso y Credenciales

Para garantizar la accesibilidad y corrección del proyecto, todos los equipos y aplicaciones configurados disponen del siguiente usuario de administración:

| Usuario | Contraseña | Notas |
| :--- | :--- | :--- |
| `bchecker` | `bchecker121` | Obligatorio en todos los sistemas. |

> **Nota:** El acceso al repositorio GitHub se realiza mediante validación por par de claves público/privada.

---

## 🧪 Pruebas de Concepto
Como prueba final de la infraestructura, se ha desplegado una pequeña aplicación (generada con soporte de IA) que consulta la base de datos `B-NCC`. Esta base de datos contiene el **Listado de equipamientos de educación de la ciudad de Barcelona** (CSV importado).

---

## 📅 Gestión del Proyecto
* **Planificación:** Proofhub.
* **Metodología:** 3 Sprints de 10 horas cada uno (quincenales).
* **Entrega:** 02/12/2025

## 👥 Autores
* **Grupo:** [G02]
* **Integrantes:** [Ian Frias, Izan Fernandez, Anmolpreet]
* **Fecha:** 01/12/2025


# Arquitectura del Laboratorio

## Descripción general

Este documento describe la arquitectura del entorno virtualizado utilizado para realizar los ejercicios de reconocimiento de red y evaluación básica de vulnerabilidades de este repositorio.

El laboratorio está compuesto por tres máquinas virtuales:

* **Kali Linux:** máquina utilizada para análisis y reconocimiento.
* **Ubuntu:** sistema objetivo.
* **Windows Server:** sistema objetivo.

Las máquinas objetivo utilizan una red **Host-Only** para mantener el entorno de pruebas aislado de la red externa.

Kali Linux dispone además de una interfaz **NAT**, utilizada para tareas que requieren acceso a Internet, como la instalación de paquetes, actualización de herramientas y administración del repositorio de GitHub.

---

## Topología de red

El laboratorio utiliza dos segmentos de red con propósitos diferentes:

<img width="302" height="361" alt="topologia" src="https://github.com/user-attachments/assets/a4ec65e4-9e00-4a6c-b5a8-62cd1a173b7a" />

## Máquinas virtuales

### Kali Linux

**Rol:** Máquina de análisis y reconocimiento.

**Dirección IP en la red del laboratorio:** `192.168.108.130`

Kali Linux es la máquina utilizada para realizar las actividades de reconocimiento, enumeración de servicios y detección de posibles vulnerabilidades.

Dispone de dos interfaces de red:

| Interfaz | Red       | Propósito                              |
| -------- | --------- | -------------------------------------- |
| `eth0`   | Host-Only | Comunicación con los sistemas objetivo |
| `eth1`   | NAT       | Acceso a Internet                      |

La interfaz `eth0` se utiliza para las pruebas de seguridad contra los sistemas objetivo.

La interfaz `eth1` se utiliza para tareas administrativas que requieren conectividad externa.

---

### Ubuntu

**Rol:** Sistema objetivo.

**Dirección IP:** `192.168.108.128`

Ubuntu está configurado como un sistema objetivo vulnerable dentro del laboratorio y expone múltiples servicios de red y aplicaciones web.

Durante las primeras etapas de reconocimiento se identificaron servicios como:

* FTP
* SSH
* HTTP
* RPC
* SMB
* IPP/CUPS
* MySQL
* IRC
* Jetty

El sistema está conectado únicamente a la red Host-Only del laboratorio.

---

### Windows Server

**Rol:** Sistema objetivo.

**Dirección IP:** `192.168.108.129`

Windows Server está configurado como un sistema objetivo vulnerable que expone diversos servicios de Windows y aplicaciones.

Durante las primeras etapas de reconocimiento se identificaron servicios como:

* FTP
* SSH
* IIS
* MSRPC
* SMB
* RDP
* WinRM
* GlassFish
* AJP
* Elasticsearch
* Java RMI

El sistema está conectado únicamente a la red Host-Only del laboratorio.

---

## Configuración de red

La red utilizada para la comunicación entre Kali Linux y los sistemas objetivo corresponde a:

```text
Red: 192.168.108.0/24
```

Los equipos actualmente utilizados son:

| Equipo         | Dirección IP      | Función                   |
| -------------- | ----------------- | ------------------------- |
| Kali Linux     | `192.168.108.130` | Análisis y reconocimiento |
| Ubuntu         | `192.168.108.128` | Sistema objetivo          |
| Windows Server | `192.168.108.129` | Sistema objetivo          |

Los sistemas objetivo no necesitan acceso directo a Internet para las actividades realizadas en este laboratorio.

Esta configuración permite separar el tráfico utilizado para las pruebas de seguridad del tráfico utilizado para tareas administrativas.

---

## Flujo de tráfico

El tráfico utilizado durante las pruebas de seguridad sigue principalmente este flujo:

Kali Linux
    │
    │ Red Host-Only
    │
    ├──────────────► Ubuntu
    │
    └──────────────► Windows Server


Las tareas que requieren acceso a Internet utilizan una ruta independiente:

Kali Linux
    │
    │ NAT
    │
    ▼
Internet


De esta manera:

* **Host-Only:** se utiliza para las pruebas de seguridad contra los sistemas objetivo.
* **NAT:** se utiliza para acceso a Internet y tareas administrativas desde Kali Linux.

---

## Aislamiento del laboratorio

Los sistemas objetivo se encuentran aislados de la red externa y no disponen de una conexión directa a Internet.

Esto es especialmente importante debido a que el proyecto contempla actividades como:

* Descubrimiento de hosts.
* Escaneo de puertos.
* Enumeración de servicios.
* Identificación de versiones.
* Detección de posibles vulnerabilidades.
* Validación manual de determinados hallazgos.

Todas las pruebas documentadas en este repositorio se realizan sobre sistemas propios o bajo control del autor dentro de un entorno de laboratorio.

No se realizan intencionalmente actividades de reconocimiento o evaluación sobre sistemas externos.

---

## Justificación de la arquitectura

La arquitectura fue diseñada para disponer de un entorno controlado donde sea posible practicar conceptos de ciberseguridad sin generar tráfico de pruebas hacia sistemas externos.

La utilización de Kali Linux como máquina de análisis y de Ubuntu y Windows Server como sistemas objetivo permite practicar reconocimiento y enumeración sobre diferentes plataformas.

La separación mediante una red Host-Only también permite mantener los sistemas vulnerables dentro del entorno de laboratorio.

La arquitectura puede ampliarse posteriormente incorporando nuevos sistemas, segmentos de red y controles de seguridad.

---


# network-reconnaissance-lab
Network reconnaissance and vulnerability assessment lab using Nmap

Laboratorio práctico de reconocimiento de redes y evaluación básica de vulnerabilidades utilizando Nmap en un entorno controlado de máquinas virtuales.

El objetivo del proyecto es desarrollar y documentar un flujo de trabajo básico de seguridad ofensiva: descubrimiento de hosts, identificación de puertos y servicios, enumeración, detección de posibles vulnerabilidades, validación manual y documentación de hallazgos.

Nota: Todas las pruebas se realizan sobre máquinas virtuales propias dentro de una red de laboratorio aislada. No se realizan pruebas contra sistemas de terceros.

Objetivos
Practicar reconocimiento de redes con Nmap.
Identificar hosts activos dentro de una red de laboratorio.
Enumerar puertos y servicios expuestos.
Identificar versiones de software.
Utilizar scripts NSE para detectar posibles vulnerabilidades.
Validar manualmente los resultados obtenidos.
Diferenciar entre vulnerabilidades confirmadas, potenciales y falsos positivos.
Documentar evidencias, impacto y recomendaciones.
Construir una metodología reproducible de evaluación.
Entorno del laboratorio

El laboratorio está compuesto por tres máquinas virtuales:

Máquina	Rol	Dirección IP	Red
Kali Linux	Máquina de análisis	192.168.108.130	Host-Only + NAT
Ubuntu	Objetivo	192.168.108.128	Host-Only
Windows Server	Objetivo	192.168.108.129	Host-Only

Kali Linux utiliza:

eth0 → red Host-Only para comunicarse con los objetivos.
eth1 → NAT para acceso a Internet y administración del repositorio.

Las máquinas objetivo permanecen dentro de la red Host-Only.

Topología
                    Internet
                       │
                     NAT
                       │
               ┌───────┴────────┐
               │   Kali Linux   │
               │ 192.168.108.130│
               └───────┬────────┘
                       │
                  Host-Only
                       │
             ┌─────────┴─────────┐
             │                   │
     ┌───────▼────────┐   ┌───────▼────────┐
     │     Ubuntu     │   │ Windows Server │
     │ 192.168.108.128│   │ 192.168.108.129│
     └────────────────┘   └────────────────┘
Metodología

El proyecto sigue un proceso progresivo de reconocimiento y evaluación:

1. Descubrimiento de hosts

Identificación de máquinas activas dentro de la red de laboratorio.

nmap -sn 192.168.108.0/24
2. Escaneo inicial

Identificación rápida de los puertos TCP abiertos.

nmap 192.168.108.128
3. Detección de servicios y versiones

Identificación de los servicios asociados a los puertos abiertos y sus versiones.

nmap -sV 192.168.108.128
4. Enumeración

Obtención de información adicional utilizando los scripts NSE incluidos con Nmap.

nmap -sC -sV 192.168.108.128
5. Detección de vulnerabilidades

Ejecución de scripts NSE orientados a la identificación de posibles vulnerabilidades.

nmap --script vuln 192.168.108.128

Los resultados de esta etapa no se consideran automáticamente vulnerabilidades confirmadas. Cada hallazgo es analizado y, cuando es posible, validado manualmente.

6. Validación manual

Los resultados relevantes se contrastan mediante herramientas como:

curl

y consultas directas a los servicios identificados.

El objetivo es determinar si un resultado corresponde a:

Vulnerabilidad confirmada.
Exposición de información.
Configuración insegura.
Posible vulnerabilidad que requiere investigación adicional.
Falso positivo.
7. Documentación

Los hallazgos se documentan incluyendo:

Sistema afectado.
Servicio y puerto.
Descripción.
Evidencia.
Impacto potencial.
Estado de validación.
Recomendaciones.
Herramientas utilizadas
Herramienta	Uso
Nmap	Reconocimiento, enumeración y detección de vulnerabilidades
Nmap NSE	Automatización de tareas de enumeración y detección
curl	Validación manual de servicios HTTP
Kali Linux	Plataforma de análisis
VMware	Virtualización del laboratorio
Resultados iniciales
Ubuntu — 192.168.108.128

Durante el reconocimiento inicial se identificaron, entre otros, los siguientes servicios:

Puerto	Servicio	Versión detectada
21	FTP	ProFTPD 1.3.5
22	SSH	OpenSSH 6.6.1p1
80	HTTP	Apache 2.4.7
139	NetBIOS	Samba
445	SMB	Samba 4.3.11
631	IPP	CUPS 1.7
3306	MySQL	MySQL
6667	IRC	UnrealIRCd
8080	HTTP	Jetty 8.1.7

Durante la enumeración HTTP se identificó un listado de directorios accesible públicamente que expone recursos como:

/chat/
/drupal/
/payroll_app.php
/phpmyadmin/

Este hallazgo fue validado manualmente mediante solicitudes HTTP.

Windows Server — 192.168.108.129

El reconocimiento inicial identificó una superficie de ataque considerable, incluyendo:

Puerto	Servicio	Información detectada
21	FTP	Microsoft FTP
22	SSH	OpenSSH 7.1
80	HTTP	IIS 7.5
135	MSRPC	Microsoft RPC
139	NetBIOS	Windows
445	SMB	Windows Server
3389	RDP	Remote Desktop
4848	HTTPS	GlassFish
5985	HTTP	WinRM
8009	AJP	Apache JServ
8080	HTTP	GlassFish
9200	HTTP	Elasticsearch 1.1.1

Los servicios identificados serán analizados progresivamente para determinar su configuración, exposición y posibles vulnerabilidades.

Hallazgos

Los resultados detallados se encuentran en:

reports/findings.md

Actualmente se documentan hallazgos relacionados con:

Exposición de listados de directorios.
Exposición de aplicaciones y recursos web.
Versiones antiguas de software.
Posibles vulnerabilidades detectadas mediante NSE.
Resultados que requieren validación adicional.

Los resultados automatizados de Nmap no se consideran evidencia suficiente por sí solos cuando requieren confirmación manual.

Estructura del repositorio
network-reconnaissance-lab/
│
├── README.md
│
├── scans/
│   ├── ubuntu-initial.txt
│   ├── ubuntu-services.txt
│   ├── ubuntu-enumeration.txt
│   ├── ubuntu-vuln.txt
│   ├── windows-initial.txt
│   ├── windows-services.txt
│   ├── windows-enumeration.txt
│   └── windows-vuln.txt
│
└── reports/
    └── findings.md

Los archivos dentro de scans/ contienen los resultados originales obtenidos durante las diferentes fases de reconocimiento.

Los informes dentro de reports/ contienen el análisis de los resultados y la validación de los hallazgos relevantes.


Propósito del proyecto

Este repositorio forma parte de mi proceso de aprendizaje práctico en ciberseguridad, con especial énfasis inicial en:

Linux.
Redes.
Reconocimiento.
Nmap.
Enumeración de servicios.
Análisis básico de vulnerabilidades.
Documentación técnica.

El objetivo es construir progresivamente conocimientos prácticos y documentarlos mediante laboratorios reproducibles, evitando presentar como conocimiento adquirido aquellas tecnologías o metodologías que todavía se encuentran en proceso de aprendizaje.

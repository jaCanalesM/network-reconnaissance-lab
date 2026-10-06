# Security Findings

## U-01 — Directory Listing Exposed

**Target:** 192.168.108.128  
**Service:** HTTP (TCP/80)  
**Finding Type:** Information Disclosure / Security Misconfiguration  
**Status:** Confirmed

### Description

The web server exposes directory listings to unauthenticated users. The root web directory (`/`) can be browsed directly, revealing application components, files, modification dates and file sizes.

The `/uploads/` directory is also accessible and provides a directory listing, although no files are currently present.

### Evidence

Request:

```text
GET http://192.168.108.128/
```


## U-02 — phpMyAdmin 3.5.8 vulnerable a CVE-2013-5003

**Target:** 192.168.108.128
**Service:** HTTP (TCP/80)
**Application:** phpMyAdmin
**Version:** 3.5.8
**CVE:** CVE-2013-5003
**Finding Type:** Vulnerable Software Version
**Status:** Identificado — no explotado

### Descripción

Durante la enumeración del servicio HTTP se identificó una instalación de phpMyAdmin accesible desde:

`http://192.168.108.128/phpmyadmin/`

La documentación disponible en la propia aplicación permitió identificar la versión como **phpMyAdmin 3.5.8**.

La versión identificada se encuentra dentro del rango afectado por **CVE-2013-5003**.

De acuerdo con el advisory oficial de phpMyAdmin, esta vulnerabilidad corresponde a una inyección SQL en las funcionalidades `schema_export.php` y `pmd_pdf.php`, que puede producir una escalada de privilegios mediante el denominado *control user*.

### Evidencia

La aplicación fue identificada mediante una solicitud HTTP:

`GET http://192.168.108.128/phpmyadmin/`

La página de documentación de la aplicación permitió identificar:

`phpMyAdmin 3.5.8 - Documentation`

También se identificó el siguiente stack tecnológico:

* Apache HTTP Server 2.4.7
* PHP 5.4.5
* phpMyAdmin 3.5.8

La versión 3.5.8 fue publicada antes de la versión de corrección 3.5.8.2. El advisory oficial de phpMyAdmin indica que las versiones 3.5.x anteriores a 3.5.8.2 están afectadas por CVE-2013-5003.

### Vulnerabilidad

**CVE-2013-5003**

La vulnerabilidad permite inyectar sentencias SQL mediante parámetros no validados en:

* `schema_export.php`
* `pmd_pdf.php`

Las consultas se ejecutarían con los privilegios del *control user*, pudiendo proporcionar acceso de lectura y escritura sobre determinadas tablas de la base de datos utilizada para el almacenamiento de configuración de phpMyAdmin.

El impacto potencial depende de la configuración del *control user* y de los privilegios que este posea.

### Condiciones de explotación

La vulnerabilidad no es directamente explotable por un usuario no autenticado.

El advisory oficial indica que:

1. El atacante debe haber iniciado sesión en phpMyAdmin.
2. Debe existir un *control user* configurado.
3. El *control user* debe disponer de los privilegios necesarios para que la vulnerabilidad produzca el impacto descrito.

Por este motivo, la identificación de la versión vulnerable **no demuestra por sí sola que el sistema sea explotable en las condiciones actuales del laboratorio**.

### Impacto potencial

En caso de cumplirse las condiciones necesarias, un atacante autenticado podría aprovechar la vulnerabilidad para ejecutar consultas SQL con los privilegios del *control user*.

Esto podría permitir acceso de lectura o escritura sobre información almacenada por phpMyAdmin y, dependiendo de los privilegios configurados, acceso a determinadas tablas de la base de datos MySQL.

### Evaluación

**Severidad:** Potencialmente alta
**Estado:** Vulnerabilidad identificada por versión
**Explotabilidad en este laboratorio:** No validada

La clasificación definitiva del riesgo requiere determinar si existe un *control user*, qué privilegios posee y si las funcionalidades afectadas pueden ser utilizadas bajo las condiciones actuales del laboratorio.

### Recomendación

Actualizar phpMyAdmin a una versión que contenga la corrección de seguridad.

El advisory oficial indica como versiones corregidas:

* phpMyAdmin 3.5.8.2
* phpMyAdmin 4.0.4.2

Para un entorno real, se recomienda utilizar además una versión actualmente soportada y mantener phpMyAdmin y sus dependencias actualizadas.

También se recomienda revisar la configuración del *control user* y aplicar el principio de mínimo privilegio.

### Limitaciones de la validación

En esta evaluación no se realizó explotación de CVE-2013-5003.

Tampoco se determinó si el entorno posee un *control user* configurado ni cuáles son sus privilegios.

Por lo tanto, este hallazgo debe interpretarse como una **identificación de software vulnerable basada en la versión detectada**, y no como una explotación confirmada.

### Referencia

Advisory oficial de phpMyAdmin: PMASA-2013-15 — CVE-2013-5003.

No se realizó explotación ni modificación del sistema objetivo.

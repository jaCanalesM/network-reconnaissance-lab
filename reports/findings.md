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

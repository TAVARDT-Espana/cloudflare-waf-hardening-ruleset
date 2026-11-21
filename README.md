# Cloudflare WAF Hardening Ruleset y Guía de Seguridad de Borde | TAVARDT España

[![Licence MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Cloudflare WAF](https://img.shields.io/badge/Cloudflare-WAF%20Hardened-F38020.svg?logo=cloudflare)](https://cloudflare.com)
[![Language](https://img.shields.io/badge/Language-Español%20(España)-green.svg)](README.md)
[![Diagnostics Suite](https://img.shields.io/badge/TAVARDT-Diagnostics-5b3cf5.svg)](https://tools.tavardt.com/es/analisis-cabeceras-seguridad/)

Reglas avanzadas de WAF y mitigación perimetral para Cloudflare, conforme a las directrices del Esquema Nacional de Seguridad (ENS) y el RGPD / LOPDGDD.

Desarrollado y mantenido por el equipo de ingeniería de seguridad perimetral de **[TAVARDT España](https://ag.tavardt.com/es/ciberseguridad/)**.

> [!IMPORTANT]
> **Arquitectura Universal y Agnóstica de Stack:** Este repositorio **no es exclusivo de WordPress**. Ha sido diseñado para blindar **APIs RESTful, microservicios modernos, aplicaciones frontend (Next.js, React, Vue) y backends empresariales (Node.js, Python, Go, Laravel, Java)**. El bloqueo de sondas de CMS actúa como una **capa de higiene perimetral activa**, descartando el ruido ciego de botnets en la red de borde de Cloudflare (< 1ms) antes de que las peticiones maliciosas consuman CPU, conexiones o memoria en sus servidores de origen.

---

### Matriz de Protección Multi-Stack

| Capa de Defensa | Vectores Mitigados | Aplicaciones y Stacks Protegidas |
| :--- | :--- | :--- |
| **Aislamiento de Credenciales** | Bloqueo perimetral de `.env`, `.git`, `.sql`, `package.json`, `composer.json` | **APIs, Next.js, Node.js, Python, Go, Laravel, React** |
| **Mitigación de Reconocimiento** | Bloqueo de Nikto, SQLMap, Acunetix, Masscan, Gobuster, Nmap | **Cualquier aplicación web, microservicio o API pública** |
| **Higiene de Verbos HTTP** | Rechazo perimetral de métodos inseguros (`TRACE`, `TRACK`, `DEBUG`) | **APIs RESTful y arquitecturas de microservicios** |
| **Protección de Accesos Privilegiados** | Reto automatizado ante consolas administrativas (`/admin`, `/login`) | **Paneles de administración, ERPs web y dashboards** |
| **Higiene de Ruido Ciego** | Descarte en el borde de sondas hacia `/xmlrpc.php` y trackbacks | **Alivio inmediato de carga de procesamiento en el origen** |

---

## 1. Visión General de la Arquitectura de Seguridad

Este repositorio proporciona un conjunto declarativo de reglas de mitigación perimetral para Cloudflare, concebido para prevenir intrusiones y exposición de datos en entornos empresariales:

* **Aislamiento Perimetral de Archivos Sensibles:** Bloqueo inmediato en el borde de solicitudes dirigidas a archivos `.env`, directorios `.git`, volcados de bases de datos `.sql` y archivos de dependencias (`composer.json`, `package.json`).
* **Mitigación de Escáneres Automatizados:** Detección y rechazo de herramientas de sondeo y explotación automatizadas (Nikto, SQLMap, Acunetix, Masscan, Nmap, Gobuster, DirBuster).
* **Blindaje de Gestores de Contenido (CMS):** Bloqueo preventivo de vectores de amplificación y abuso en `/xmlrpc.php` y `/wp-trackback.php`.
* **Filtrado de Métodos HTTP Inseguros:** Denegación perimetral de métodos de depuración de riesgo (`TRACE`, `TRACK`, `DEBUG`).
* **Desafío Gestionado para Endpoints Administrativos:** Reto automatizado ante accesos a consolas de administración (`/wp-login.php`, `/admin`).

---

## 2. Diagnóstico Previo de Seguridad

Antes de proceder a la aplicación en entornos de producción, verifique las cabeceras de seguridad y la configuración SSL de su dominio mediante las herramientas abiertas de TAVARDT:

* **Auditoría de Cabeceras de Seguridad y SSL:** [Análisis de Cabeceras de Seguridad TAVARDT](https://tools.tavardt.com/es/analisis-cabeceras-seguridad/)
* **Catálogo Global de Herramientas:** [TAVARDT Tools España](https://tools.tavardt.com/es/)

---

## 3. Guía de Implementación

### Opción A: Despliegue Directo mediante Cloudflare API v4

Utilice el archivo de reglas declarativo [`rules/ruleset.json`](rules/ruleset.json):

```bash
curl -X PUT "https://api.cloudflare.com/client/v4/zones/SU_ZONE_ID/rulesets/phases/http_request_firewall_custom/entrypoint" \
     -H "Authorization: Bearer SU_CLOUDFLARE_API_TOKEN" \
     -H "Content-Type: application/json" \
     --data @rules/ruleset.json
```

### Opción B: Automatización Declarativa con Terraform

Aplique el módulo de infraestructura como código en [`terraform/main.tf`](terraform/main.tf):

```bash
cd terraform
terraform init
terraform plan -var="zone_id=SU_ZONE_ID"
terraform apply -var="zone_id=SU_ZONE_ID"
```

---

## 4. Consultoría Pericial y Soporte Técnico Continuo

Si su empresa requiere auditorías de seguridad avanzadas, adecuación al Esquema Nacional de Seguridad (ENS / NIS 2) o ingeniería perimetral especializada:

* **Portal Corporativo:** [TAVARDT España](https://ag.tavardt.com/es/ciberseguridad/)
* **Contacto Técnico Especializado:** [Solicitar Consulta Técnica](https://ag.tavardt.com/es/contacto/)

---

## Licencia

Distribuido bajo licencia de código abierto MIT. Consulte el archivo [`LICENSE`](LICENSE) para más detalles.  
Copyright (c) 2026 TAVARDT España.

---
task_id: 260531-rec
status: complete
commit: 09f5334
date: 2026-05-31
---

# Quick Task 260531-rec — Completado

## Qué se hizo

Insertada la subsección `### Tipos de Reconocimiento` en la Etapa 1 del post tema03, inmediatamente después del párrafo intro y antes del lab de Nmap.

## Contenido añadido

- Tabla de 3 estratos de reconocimiento:
  - **Pasivo**: Google Dorks, Shodan, Censys, WHOIS, metadatos de documentos, LinkedIn/OSINT de empleados
  - **Semi-pasivo**: DNS (MX/TXT/SPF), Certificate Transparency (crt.sh), Wayback Machine
  - **Activo**: Nmap, Nikto, gobuster, banner grabbing, Nessus/OpenVAS
- Nota callout que ancla el lab actual: reconocimiento activo en red interna aislada
- Mención explícita de que sesiones futuras profundizarán en OSINT pasivo

## Verificación

- 12 líneas añadidas en `_posts/2026-05-31-tema03.md`
- grep confirma: "Tipos de Reconocimiento", "Shodan", "crt.sh", "OSINT", callout presente
- Flujo del lab no interrumpido: subsección antes de `### 1.1.`

## Commit

`09f5334` — content(tema03): agregar tabla de tipos de reconocimiento

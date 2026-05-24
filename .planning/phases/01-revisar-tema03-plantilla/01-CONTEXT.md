# Phase 1: Revisar tema03 + Plantilla - Context

**Gathered:** 2026-05-24
**Status:** Ready for planning

<domain>
## Phase Boundary

Entregar `_posts/2026-XX-XX-tema03.md` revisado y publicado con: (1) el kill chain SSH brute force original intacto, (2) dos kill chains adicionales verificados contra las OVAs (Jenkins Script Console + EternalBlue/MS17-010), (3) secciones Objetivos + intro teórica por kill chain + tabla MITRE ATT&CK añadidas sin reestructurar el lab existente. El post resultante sirve como plantilla estructural (STR-01) para todas las fases siguientes.

</domain>

<decisions>
## Implementation Decisions

### Gestión de archivos

- **D-01:** Descartar `_drafts/2026-05-24-tema03.md` (contenido de IA en Ciberseguridad). El archivo no encaja en el roadmap del curso (pentesting ofensivo) y puede eliminarse.
- **D-02:** Re-datar el post revisado a 2026 con un nuevo filename (ej. `_posts/2026-MM-DD-tema03.md`, fecha concreta a decidir en plan, consistente con tema01=2026-05-10 y tema02=2026-05-17).
- **D-03:** Eliminar el archivo original `_posts/2025-10-05-tema03.md` una vez publicado el nuevo.
- **D-04:** Añadir `_posts/2025-10-05-tema03.md.backup` (no commiteado, untracked) al `.gitignore` — no requiere acción de contenido.

### Kill chains a añadir

- **D-05:** Kill chain 2 — **Jenkins Script Console RCE** (puerto 8484):
  - Técnica: acceso manual via browser a `http://10.0.2.15:8484/jenkins/script`
  - Demostración: Groovy code execution directo desde la web UI (sin Metasploit)
  - Enfatiza explotación de misconfiguration, no de vulnerabilidad CVE

- **D-06:** Kill chain 3 — **EternalBlue / MS17-010** (puerto 445 SMB):
  - Kill chain completo: `nmap --script vuln` para confirmar → `ms17_010_eternalblue` en Metasploit → shell SYSTEM
  - Esta es una introducción al exploit; la cobertura en profundidad (historia, variantes, defensa) queda para tema07 (Fase 5)
  - MS17-010 CONFIRMADO en las OVAs vía nmap vuln scan

### Estructura del post revisado

- **D-07:** Agregar secciones faltantes **sin reestructurar** el lab existente (1213 líneas preservadas):
  1. Añadir sección `## Objetivos de Aprendizaje` al inicio del post (antes de "Resumen del Laboratorio")
  2. Añadir breve intro teórica antes de **cada** kill chain (~1-2 párrafos: qué es la técnica, por qué funciona, MITRE ATT&CK Tactic + Technique ID)
  3. Añadir sección `## Mapeo MITRE ATT&CK` como tabla resumen al final (antes de Conclusión)
  4. El lab existente (Etapas 1-7) se mantiene intacto — solo agregar, no mover

### MITRE ATT&CK mapping

- **D-08:** Tabla resumen al final del post (antes de la Conclusión existente):
  - Columnas: Etapa | Kill Chain | Táctica MITRE | Técnica MITRE (ID + nombre)
  - Cubrir los 3 kill chains: SSH BF existente + Jenkins nuevo + EternalBlue nuevo
  - Esta tabla se replica (con los datos específicos del tema) en todos los posts siguientes

### Claude's Discretion

- Fecha exacta del nuevo post (debe seguir la secuencia: tema01=2026-05-10, tema02=2026-05-17, tema03=2026-05-24 o posterior)
- Longitud de la introducción teórica por kill chain (guideline: ~1-2 párrafos, no más del 40% del total)
- IDs exactos de técnicas MITRE ATT&CK a incluir en la tabla (researcher debe verificar los IDs correctos)
- Redacción concreta de los Objetivos de Aprendizaje

</decisions>

<canonical_refs>
## Canonical References

**Downstream agents MUST read these before planning or implementing.**

### Planificación del proyecto

- `.planning/ROADMAP.md` §Phase 1 — Goal, success criteria (5 criterios de éxito), requirements scope
- `.planning/REQUIREMENTS.md` §CONT-01, §STR-01, §STR-02, §STR-03 — requisitos activos para esta fase

### Archivos fuente a modificar / referenciar

- `_posts/2025-10-05-tema03.md` — Post base a revisar. Estructura actual: Recon (Nmap) → Enumeración → SSH BF (Metasploit) → Acceso SSH → SAM dump (vssown.vbs). Mantener intacto, agregar secciones.
- `_posts/2026-05-10-tema01.md` — Referencia de estructura y estilo para el post revisado (incluye `math: true` en front matter, links de grabación al inicio)
- `_posts/2026-05-17-tema02.md` — Segunda referencia de estructura; ver tratamiento de secciones teoría/lab

### Archivo a descartar

- `_drafts/2026-05-24-tema03.md` — Eliminar. Contenido sobre IA en Ciberseguridad, no relacionado con el pentesting lab. No hay dependencias.

</canonical_refs>

<code_context>
## Existing Code Insights

### Reusable Assets

- **Kill chain 1 existente** (`_posts/2025-10-05-tema03.md`): Nmap recon → Metasploit ssh_enumusers → hydra SSH BF → SSH login → vssown.vbs shadow copy → SAM/SYSTEM dump → John/Hashcat. Todos los comandos con outputs reales de las OVAs. Preservar íntegramente.
- **Front matter pattern**: `math: true` habilitado en tema01/02, útil si se añaden fórmulas en teoría.

### Established Patterns

- **Filename convention**: `YYYY-MM-DD-tema##.md` en `_posts/`
- **Post opening**: Link de grabación de clase al principio del post (ver tema01, tema02)
- **Lab IPs confirmadas**: Kali Linux = `10.0.2.9/24`, Metasploitable3 = `10.0.2.15/24` en NAT Network VirtualBox

### Integration Points

- Jekyll/Chirpy builds automáticamente con GitHub Actions al hacer push a main
- Posts en `_drafts/` no se publican — usar `_posts/` con la fecha correcta

### Servicios Metasploitable3 relevantes para los nuevos kill chains

- Puerto 445/SMB: Windows Server 2008 R2 — **MS17-010 (EternalBlue) CONFIRMADO** en nmap vuln scan
- Puerto 8484: Jenkins (Jetty) — Dashboard expuesto sin autenticación, Script Console accesible directamente

</code_context>

<specifics>
## Specific Ideas

- **Jenkins kill chain**: el alumno abre `http://10.0.2.15:8484/jenkins/script` en el browser, escribe código Groovy para ejecutar un comando del sistema (ej. `"cmd /c whoami".execute().text`), demuestra RCE sin exploits. Énfasis: misconfiguration, no CVE.
- **EternalBlue kill chain**: primero confirmar con `nmap --script vuln -p 445 10.0.2.15` que MS17-010 es explotable, luego Metasploit `use exploit/windows/smb/ms17_010_eternalblue` → `set RHOSTS 10.0.2.15` → `run` → shell SYSTEM.
- **Tabla MITRE**: el template de esta tabla (3 kill chains, columnas: Etapa | Kill Chain | Táctica | Técnica ID) se convierte en el estándar para todos los posts siguientes.
- **Fecha del post revisado**: la siguiente fecha lógica en la secuencia es 2026-05-31 (semana siguiente a tema02=2026-05-17), pero si la clase real tiene fecha diferente usar esa.

</specifics>

<deferred>
## Deferred Ideas

- **Tomcat Manager WAR upload** (puerto 8282): mencionado como opción de kill chain, diferido a Fase 3 (tema05 — Explotación Web). No incluir en tema03.
- **`_posts/2025-10-05-tema03.md.backup`**: añadir al `.gitignore`; la limpieza del repo puede hacerse en cualquier momento sin afectar la fase.
- **Cobertura en profundidad de EternalBlue** (historia WannaCry, variantes, defensa, detección): diferido a Fase 5 (tema07 — EternalBlue/SMB). Tema03 solo ejecuta el exploit.

</deferred>

---

*Phase: 1-Revisar tema03 + Plantilla*
*Context gathered: 2026-05-24*

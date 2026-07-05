# Roadmap: Curso de Ciberseguridad UNTELS 2026

## Overview

El roadmap produce 10 posts de sesión (tema03-tema12) publicados como entradas Jekyll/Chirpy. Cada fase entrega uno o dos posts listos para publicar, verificados contra las OVAs del laboratorio. La Fase 1 establece la plantilla estructural que se aplica en todas las fases siguientes. El orden sigue la progresión pedagógica del curso: desde revisión del ataque base hasta el CTF final.

## Phases

**Phase Numbering:**

- Integer phases (1-8): Trabajo planificado del milestone
- Decimal phases: Inserciones urgentes vía `/gsd:phase insert`

- [ ] **Phase 1: Revisar tema03 + Plantilla** - Publicar tema03 revisado con kill chains adicionales; establece la estructura estándar de posts
- [ ] **Phase 2: Reconocimiento (tema04)** - Post completo sobre Nmap avanzado, OSINT y footprinting
- [x] **Phase 3: Threat Intelligence Management (tema05)** - Post sobre TIM con OpenCTI: teoría + lab slim (repo untels-tim-lab) + ejercicios de investigación de IoCs *(el plan original "Explotación Web" se movió a Deferred/Backlog)*
- [ ] **Phase 4: Buffer Overflows (tema06)** - Post reservado sobre stack-based overflows y shellcode
- [ ] **Phase 5: EternalBlue/SMB (tema07)** - Post sobre MS17-010 y explotación SMB completa
- [ ] **Phase 6: Post-explotación y Privesc (tema08+09)** - Dos posts sobre Meterpreter, persistencia y escalación de privilegios
- [ ] **Phase 7: Seguridad Web (tema10)** - Post sobre OWASP Top 10, SQLi y XSS en Metasploitable3
- [ ] **Phase 8: Defensa y CTF Final (tema11+12)** - Dos posts de cierre: detección/defensa y proyecto evaluativo

## Phase Details

### Phase 1: Revisar tema03 + Plantilla

**Goal:** tema03 publicado en `_posts/` con ≥2 kill chains adicionales verificados, mapeo MITRE completo y estructura estándar que sirve de referencia para todas las fases
**Mode:** mvp
**Depends on:** Nothing (first phase)
**Requirements:** CONT-01, STR-01, STR-02, STR-03
**Success Criteria** (what must be TRUE):

  1. tema03 movido de `_drafts/` a `_posts/` y visible en el sitio publicado
  2. El post incluye ≥2 kill chains alternativos funcionales en Metasploitable3 (ej. EternalBlue/MS17-010, Jenkins Script Console)
  3. Cada etapa del lab tiene su técnica MITRE ATT&CK mapeada (Recon → Exfiltration)
  4. Todos los comandos y outputs del post coinciden con lo que el alumno ve en las OVAs proporcionadas
  5. El post sigue la estructura: Objetivos → Teoría [40%] → Lab guiado [60%] → MITRE mapping → Conclusión

**Plans:** 2/4 plans executed
Plans:
**Wave 1**

- [x] 01-01-PLAN.md — Añadir `_posts/*.backup` al .gitignore (D-04)
- [x] 01-02-PLAN.md — Walking Skeleton: crear `_posts/2026-05-31-tema03.md` con front matter canónico + lab existente preservado + Objetivos + disclaimer legal + intro teórica KC1

**Wave 2** *(blocked on Wave 1 completion)*

- [ ] 01-03-PLAN.md — Insertar Kill Chain 2 (Jenkins Script Console RCE) + Kill Chain 3 (EternalBlue/MS17-010) + tabla `## Mapeo MITRE ATT&CK` con 11 filas

**Wave 3** *(blocked on Wave 2 completion)*

- [ ] 01-04-PLAN.md — Limpieza (eliminar `_posts/2025-10-05-tema03.md` y `_drafts/2026-05-24-tema03.md`) + checkpoint humano de verificación visual del post servido localmente

### Phase 2: Reconocimiento (tema04)

**Goal:** tema04 publicado — reconocimiento avanzado y OSINT completo con lab guiado sobre Metasploitable3
**Mode:** mvp
**Depends on:** Phase 1
**Requirements:** CONT-02
**Success Criteria** (what must be TRUE):

  1. tema04 publicado en `_posts/` con filename `YYYY-MM-DD-tema04.md`
  2. El lab cubre Nmap avanzado (scripts NSE, timing, evasión de detección) aplicado a Metasploitable3
  3. El post incluye técnicas de footprinting pasivo (Shodan, Google dorks) con ejemplos reales
  4. Todos los comandos Nmap y outputs son verificados contra las OVAs en NAT Network

**Plans:** TBD

### Phase 3: Threat Intelligence Management (tema05) — ✅ COMPLETADA

**Goal:** tema05 publicado — Threat Intelligence Management con OpenCTI: teoría del ciclo de inteligencia y un lab práctico donde el alumno levanta su propio TIM e investiga IoCs reales
**Mode:** mvp
**Depends on:** Phase 2
**Requirements:** CONT-03 *(reinterpretado: de "Explotación Web" a "Threat Intelligence")*
**Ejecutada vía:** quick task `260705-acg` (2026-07-05, commit `0f71ba2`)
**Success Criteria** (what must be TRUE):

  1. ✅ tema05 publicado en `_posts/2025-11-22-tema05.md` (slug `tema05`)
  2. ✅ El post cubre teoría TIM: ciclo de inteligencia, IoC vs TTP vs CTI, Pirámide del Dolor, STIX 2.1, OpenCTI, fuentes OSINT, MITRE ATT&CK
  3. ✅ El lab (repo separado <https://github.com/ollerenacastro/untels-tim-lab>) levanta un stack slim de OpenCTI sin módulos LLM/GPU en VM Kali de 8GB: OpenCTI 6.4 + MITRE ATT&CK + CISA KEV + feeds URLhaus/Feodo
  4. ✅ 4 ejercicios de investigación de IoCs anclados en OilRig/APT34 (continuidad con tema04)

**Nota:** El lab NO vive en este repo Jekyll — es el repositorio GitHub separado `ollerenacastro/untels-tim-lab` (stack Docker), enlazado desde el post.

**Plans:** N/A (ejecutado como quick task, no como plan de fase)

### Phase 4: Buffer Overflows (tema06)

> ⚠️ **Nota (2026-07-05):** el post existente `_posts/2025-11-29-tema06.md` fue **despublicado** (`published: false`) por decisión del profesor. El archivo se conserva; ya no aparece en el sitio.

**Goal:** tema06 publicado — introducción a stack-based buffer overflows con lab controlado y generación de shellcode
**Mode:** mvp
**Depends on:** Phase 3
**Requirements:** CONT-04
**Success Criteria** (what must be TRUE):

  1. tema06 publicado en `_posts/` con filename `YYYY-MM-DD-tema06.md`
  2. El post explica stack layout, control de EIP y shellcode con diagramas o bloques ASCII
  3. El lab guía al alumno desde crash hasta shell usando msfvenom en entorno controlado
  4. El ejercicio funciona dentro de las OVAs proporcionadas sin dependencias externas

**Plans:** TBD

### Phase 5: EternalBlue/SMB (tema07)

> ⚠️ **Nota (2026-07-05):** el archivo `_posts/2025-12-15-tema07.md` (actualmente "Examen Final") fue **despublicado** (`published: false`). Nota adicional: el slug `tema07` planeado para EternalBlue/SMB choca con el archivo existente; resolver la numeración al planear esta fase.

**Goal:** tema07 publicado — explotación completa de MS17-010 contra Metasploitable3, desde detección hasta shell SYSTEM
**Mode:** mvp
**Depends on:** Phase 4
**Requirements:** CONT-05
**Success Criteria** (what must be TRUE):

  1. tema07 publicado en `_posts/` con filename `YYYY-MM-DD-tema07.md`
  2. El post contextualiza EternalBlue/WannaCry con historia y alcance real
  3. El lab documenta detección con `nmap --script vuln` y muestra output confirmando MS17-010
  4. El lab guía la explotación completa con Metasploit hasta obtener shell SYSTEM verificada en las OVAs

**Plans:** TBD

### Phase 6: Post-explotación y Privesc (tema08 + tema09)

**Goal:** tema08 y tema09 publicados — cadena completa desde sesión Meterpreter hasta escalación de privilegios en Windows
**Mode:** mvp
**Depends on:** Phase 5
**Requirements:** CONT-06, CONT-07
**Success Criteria** (what must be TRUE):

  1. tema08 publicado en `_posts/` con filename `YYYY-MM-DD-tema08.md`
  2. tema09 publicado en `_posts/` con filename `YYYY-MM-DD-tema09.md`
  3. tema08 cubre comandos Meterpreter de reconocimiento post-acceso, persistencia (registry/service) y exfiltración de datos
  4. tema09 demuestra ≥2 técnicas de escalación de privilegios funcionales en Metasploitable3 (ej. token impersonation, AlwaysInstallElevated)
  5. Ambos posts encadenan sus labs — tema09 parte de donde tema08 termina

**Plans:** TBD

### Phase 7: Seguridad Web (tema10)

**Goal:** tema10 publicado — OWASP Top 10 aplicado sobre servicios reales de Metasploitable3 con Burp Suite
**Mode:** mvp
**Depends on:** Phase 6
**Requirements:** CONT-08
**Success Criteria** (what must be TRUE):

  1. tema10 publicado en `_posts/` con filename `YYYY-MM-DD-tema10.md`
  2. El lab demuestra SQL injection funcional sobre MySQL/WampServer en Metasploitable3
  3. El lab demuestra XSS stored y/o reflected en un servicio real de Metasploitable3
  4. El post mapea los vectores explotados a OWASP Top 10 e incluye uso de Burp Suite Community

**Plans:** TBD

### Phase 8: Defensa y CTF Final (tema11 + tema12)

**Goal:** tema11 y tema12 publicados — el curso cierra con detección/defensa y un escenario evaluativo completo sin guía
**Mode:** mvp
**Depends on:** Phase 7
**Requirements:** CONT-09, CONT-10
**Success Criteria** (what must be TRUE):

  1. tema11 publicado en `_posts/` con filename `YYYY-MM-DD-tema11.md`
  2. tema12 publicado en `_posts/` con filename `YYYY-MM-DD-tema12.md`
  3. tema11 enseña al alumno a detectar en logs de Windows (Event IDs clave) los ataques practicados en el curso
  4. tema12 describe el escenario CTF completo con criterios de evaluación claros y formato de reporte profesional esperado
  5. Los 10 posts (tema03-tema12) están publicados — el blog del curso está completo

**Plans:** TBD

## Progress

**Execution Order:** 1 → 2 → 3 → 4 → 5 → 6 → 7 → 8

| Phase | Plans Complete | Status | Completed |
|-------|----------------|--------|-----------|
| 1. Revisar tema03 + Plantilla | 2/4 | In Progress|  |
| 2. Reconocimiento (tema04) | 0/? | Not started | - |
| 3. Threat Intelligence Mgmt (tema05) | quick 260705-acg | ✅ Complete | 2026-07-05 |
| 4. Buffer Overflows (tema06) | 0/? | Not started | - |
| 5. EternalBlue/SMB (tema07) | 0/? | Not started | - |
| 6. Post-explotación y Privesc (tema08+09) | 0/? | Not started | - |
| 7. Seguridad Web (tema10) | 0/? | Not started | - |
| 8. Defensa y CTF Final (tema11+12) | 0/? | Not started | - |

## Deferred / Backlog

| Item | Origen | Estado | Nota |
|------|--------|--------|------|
| **Explotación Web** (Jenkins Script Console RCE, Tomcat WAR upload, GlassFish) con MITRE mapping | Plan original de Phase 3 (tema05), reemplazado por TIM el 2026-07-05 | Diferido | Contenido válido; asignar un tema-número libre si se quiere recuperar como clase futura. Success criteria original preservado en el historial git de este ROADMAP. |

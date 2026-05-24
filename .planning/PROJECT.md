# Curso de Ciberseguridad UNTELS 2026

## What This Is

Blog de curso de ciberseguridad publicado en GitHub Pages (Jekyll/Chirpy), dirigido a estudiantes de pregrado con base en redes y Linux. El curso cubre 12 sesiones de 3 horas cada una, combinando teoría (40%) con laboratorios prácticos guiados (60%) usando Kali Linux y Metasploitable3. Todo el contenido está en español y se publica como posts de Jekyll.

## Core Value

Cada sesión produce un escenario de ataque o defensa completamente documentado que el alumno ejecutó en su propia máquina virtual, aplicando metodología MITRE ATT&CK en un entorno controlado.

## Requirements

### Validated

- ✓ **tema01**: Introducción al curso, framework GitHub Pages + Jekyll + Chirpy explicado con diagrama — sesión 1
- ✓ **tema02**: Guía de setup del laboratorio (VirtualBox, OVAs de Kali Linux y Metasploitable3, NAT Network, networking en VirtualBox) — sesión 2

### Active

- [ ] **TEMA03-REV**: Revisar tema03 — agregar 2-3 kill chains alternativos (EternalBlue, Jenkins RCE, Tomcat Manager), profundizar mapeo MITRE ATT&CK, verificar precisión técnica de comandos/outputs
- [ ] **TEMA04**: Reconocimiento avanzado y OSINT — Nmap avanzado, enumeración de servicios, Shodan, técnicas de footprinting pasivo/activo
- [ ] **TEMA05**: Explotación de servicios web en Metasploitable3 — Jenkins, Tomcat, GlassFish, WampServer/PHP
- [ ] **TEMA06**: Buffer overflows — stack-based buffer overflows, shellcode, control de EIP (ya reservado)
- [ ] **TEMA07**: Explotación de SMB / EternalBlue (MS17-010) — el exploit de WannaCry presente en Metasploitable3
- [ ] **TEMA08**: Post-explotación — Meterpreter, pivoting, persistencia, transferencia de archivos
- [ ] **TEMA09**: Escalación de privilegios en Windows — token impersonation, misconfigurations, UAC bypass
- [ ] **TEMA10**: Seguridad en aplicaciones web — OWASP Top 10, SQL injection, XSS sobre servicios en Metasploitable3
- [ ] **TEMA11**: Defensa y detección — análisis de logs, IDS/IPS básico, respuesta a incidentes
- [ ] **TEMA12**: CTF / Proyecto final — escenario completo sin guía, evaluación práctica

### Out of Scope

- Cloud/AWS security — el lab solo soporta VMs locales, agregar servicios cloud complicaría setup
- Herramientas de defensa avanzadas (SIEM, EDR) — solo cobertura conceptual; el foco es metodología ofensiva
- Desarrollo de aplicaciones vulnerables — los alumnos no aprenden a construir apps; usan Metasploitable3
- Lab con más de 2 VMs simultáneas — mantener setup simple para equipos de estudiantes con hardware limitado
- Técnicas contra sistemas reales o en internet — todos los ataques van contra Metasploitable3 en red aislada

## Context

**Plataforma:** GitHub Pages + Jekyll 4.x + Chirpy theme
- Remote: `git@github-ollerenacastro:ollerenacastro/ollerenacastro.github.io.git` (SSH alias multi-cuenta)
- Posts publicados: `_posts/YYYY-MM-DD-tema##.md` | Borradores: `_drafts/` (published:false no funciona en Chirpy)
- Grabaciones de clase: Google Drive, links al inicio de cada post

**Audiencia:** Pregrado universitario, base en redes TCP/IP y Linux básico; sin experiencia previa en pentesting

**Duración:** 12 sesiones × 3 horas | Ratio 40% teoría / 60% laboratorio

**Lab:**
- Atacante: Kali Linux (OVA proporcionado) — `10.0.2.9/24` en NAT Network de VirtualBox
- Target: Metasploitable3 Windows Server 2008 R2 sin parchar (OVA proporcionado) — `10.0.2.15/24`
- Servicios explotables confirmados en Metasploitable3:
  - Puerto 22: OpenSSH 7.1 — sin rate limiting (usado en tema03)
  - Puerto 445/SMB: Windows Server 2008 R2 — **MS17-010 (EternalBlue) CONFIRMADO** en nmap vuln scan
  - Puerto 3000: Ruby on Rails/WEBrick (Ruby 2.3.3)
  - Puerto 3306: MySQL 5.5.20 (acceso anónimo posible)
  - Puerto 3389: RDP
  - Puerto 4848/8080: Oracle GlassFish 4.0
  - Puerto 8282: Apache Tomcat 8.0.33
  - Puerto 8484: Jenkins (Jetty) — Dashboard expuesto, Script Console accesible
  - Puerto 8585: WampServer + Apache 2.2.21 + PHP 5.3.10
  - Puerto 9200: Elasticsearch 1.1.1 (sin autenticación)

**Marco metodológico:** MITRE ATT&CK — cada laboratorio mapea fases Recon → Initial Access → Execution → Persistence → Privilege Escalation → Credential Access → Exfiltration

## Constraints

- **Tech (Jekyll/Chirpy)**: No plugins personalizados más allá de los soportados por GitHub Pages — usar markdown estándar + HTML permitido
- **Lab (entorno)**: Todos los ejercicios prácticos deben funcionar en las OVAs proporcionadas; no depender de internet dentro del lab
- **Sesiones**: Ningún tema puede ocupar más de 2 sesiones — 12 sesiones para 12 temas (algunos temas = 1 sesión, ninguno > 2)
- **Idioma**: Todo el contenido para estudiantes en español
- **Legal**: Técnicas ofensivas solo demostradas contra Metasploitable3 en red aislada — nunca contra sistemas reales

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| tema06 = Buffer Overflows | Reservado por el profesor, no negociable | — Pending |
| 40% teoría / 60% lab | Maximiza el hands-on para pregrado; teoría suficiente para contextualizar | — Pending |
| Kali + Metasploitable3 como único lab | Students receive OVAs — sin dependencia de internet ni servicios externos | — Pending |
| Max 2 sesiones por tema | Evita arrastre del curso; mantiene ritmo | — Pending |
| _drafts/ en lugar de published:false | published:false ignorado por GitHub Pages con Chirpy | ✓ Good |
| SSH alias `github-ollerenacastro` en remote | Multi-cuenta GitHub en mismo servidor — necesario para push correcto | ✓ Good |

## Evolution

Este documento evoluciona en transiciones de fase y hitos de milestone.

**Después de cada fase** (via `/gsd:discuss-phase`):
1. ¿Requisitos invalidados? → Mover a Out of Scope con razón
2. ¿Requisitos validados (post publicado)? → Mover a Validated con referencia de sesión
3. ¿Requisitos nuevos emergieron? → Agregar a Active
4. ¿Decisiones a registrar? → Agregar a Key Decisions
5. ¿"What This Is" sigue preciso? → Actualizar si drifted

**Después de cada milestone** (via `/gsd:complete-milestone`):
1. Revisión completa de todas las secciones
2. Core Value check — ¿sigue siendo la prioridad correcta?
3. Auditar Out of Scope — ¿las razones siguen vigentes?
4. Actualizar Context con estado actual

---
*Last updated: 2026-05-24 after initialization*

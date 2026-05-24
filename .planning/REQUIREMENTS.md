# Requirements: Curso de Ciberseguridad UNTELS 2026

**Defined:** 2026-05-24
**Core Value:** Cada sesión produce un escenario de ataque/defensa completamente documentado que el alumno ejecutó en su propia VM, aplicando metodología MITRE ATT&CK en entorno controlado.

## v1 Requirements

### Contenido de Sesiones

- [x] **CONT-00a**: Publicar tema01 — Introducción al curso, framework GitHub Pages + Jekyll + Chirpy *(ya validado)*
- [x] **CONT-00b**: Publicar tema02 — Setup del laboratorio (VirtualBox, OVAs, NAT Network) *(ya validado)*
- [ ] **CONT-01**: Revisar tema03 (actualmente en _drafts) — Agregar ≥2 kill chains alternativos realizables en Metasploitable3 (ej. EternalBlue/MS17-010, Jenkins Script Console, Tomcat WAR upload), profundizar mapeo MITRE ATT&CK por etapa, verificar precisión técnica de todos los comandos y outputs contra las OVAs
- [ ] **CONT-02**: Crear tema04 — Reconocimiento avanzado y OSINT: Nmap avanzado (scripts NSE, timing, evasión), banner grabbing, enumeración de servicios, footprinting pasivo (Shodan, Google dorks); lab guiado sobre Metasploitable3
- [ ] **CONT-03**: Crear tema05 — Explotación de servicios web en Metasploitable3: Jenkins Script Console RCE (puerto 8484), Tomcat Manager WAR upload (puerto 8282), GlassFish exploits (puerto 4848/8080); comparar vectores y MITRE mapping
- [ ] **CONT-04**: Crear tema06 — Buffer overflows: stack-based buffer overflow, control de EIP, generación de shellcode con msfvenom, explotación básica en entorno controlado
- [ ] **CONT-05**: Crear tema07 — SMB y EternalBlue (MS17-010): historia del exploit (WannaCry), detección con nmap --script vuln, explotación completa con Metasploit contra Metasploitable3 (confirmado vulnerable), obtención de shell SYSTEM
- [ ] **CONT-06**: Crear tema08 — Post-explotación: sesiones Meterpreter, comandos de reconocimiento post-acceso, escalación básica, persistencia (registry/service), pivoting conceptual, exfiltración de datos
- [ ] **CONT-07**: Crear tema09 — Escalación de privilegios en Windows: enumeración de misconfigurations, token impersonation (Incognito/Meterpreter), UAC bypass, AlwaysInstallElevated, servicios con permisos débiles
- [ ] **CONT-08**: Crear tema10 — Seguridad en aplicaciones web (OWASP Top 10): SQL injection (sobre MySQL/WampServer en Metasploitable3), XSS stored/reflected, CSRF, herramientas (Burp Suite Community)
- [ ] **CONT-09**: Crear tema11 — Defensa y detección: análisis de logs de Windows (Event IDs clave), conceptos IDS/IPS, Wireshark para detectar ataques vistos en el curso, incident response básico
- [ ] **CONT-10**: Crear tema12 — CTF / Proyecto final: escenario de pentesting completo contra Metasploitable3 sin guía, documentación de hallazgos en formato reporte profesional, evaluación práctica

### Estructura y Calidad

- [ ] **STR-01**: Cada post sigue estructura consistente: (1) Objetivos de aprendizaje → (2) Teoría + fundamentos [40%] → (3) Laboratorio guiado con comandos [60%] → (4) Mapeo MITRE ATT&CK → (5) Conclusión y recomendaciones
- [ ] **STR-02**: Todos los comandos y outputs de cada lab son verificados contra las OVAs proporcionadas (Kali Linux + Metasploitable3 en NAT Network VirtualBox)
- [ ] **STR-03**: Cada post publicado en `_posts/` con filename `YYYY-MM-DD-tema##.md`; borradores en `_drafts/` hasta la fecha de clase

## v2 Requirements

### Mejoras futuras

- **V2-01**: Versión en inglés de los posts (para difusión internacional)
- **V2-02**: Videos explicativos embebidos (más allá del link a grabación de clase)
- **V2-03**: Quiz/ejercicios de autoevaluación al final de cada post
- **V2-04**: Lab con tercer VM (Windows 10 para tema de escalación más realista)

## Out of Scope

| Feature | Reason |
|---------|--------|
| Cloud/AWS security (práctico) | Lab solo soporta VMs locales sin internet en el ejercicio |
| Herramientas defensivas avanzadas (SIEM, EDR) | Solo cobertura conceptual; foco es metodología ofensiva |
| Desarrollo de aplicaciones vulnerables | Alumnos no aprenden a construir apps; usan Metasploitable3 |
| Lab con >2 VMs simultáneas | Equipos de estudiantes con hardware limitado |
| Ataques contra sistemas reales | Solo contra Metasploitable3 en red aislada |
| Currículo en inglés (v1) | Todo el contenido para estudiantes en español en esta versión |

## Traceability

| Requirement | Phase | Status |
|-------------|-------|--------|
| CONT-01 | Phase 1 — Revisar tema03 + Plantilla | Pending |
| STR-01 | Phase 1 — Revisar tema03 + Plantilla | Pending (carries forward) |
| STR-02 | Phase 1 — Revisar tema03 + Plantilla | Pending (carries forward) |
| STR-03 | Phase 1 — Revisar tema03 + Plantilla | Pending (carries forward) |
| CONT-02 | Phase 2 — Reconocimiento (tema04) | Pending |
| CONT-03 | Phase 3 — Explotación Web (tema05) | Pending |
| CONT-04 | Phase 4 — Buffer Overflows (tema06) | Pending |
| CONT-05 | Phase 5 — EternalBlue/SMB (tema07) | Pending |
| CONT-06 | Phase 6 — Post-explotación y Privesc (tema08+09) | Pending |
| CONT-07 | Phase 6 — Post-explotación y Privesc (tema08+09) | Pending |
| CONT-08 | Phase 7 — Seguridad Web (tema10) | Pending |
| CONT-09 | Phase 8 — Defensa y CTF Final (tema11+12) | Pending |
| CONT-10 | Phase 8 — Defensa y CTF Final (tema11+12) | Pending |

**Coverage:**
- v1 requirements: 13 total (excl. 2 already validated)
- Mapped to phases: 13/13
- Unmapped: 0 ✓

---
*Requirements defined: 2026-05-24*
*Last updated: 2026-05-24 after roadmap creation (8 phases, standard granularity)*

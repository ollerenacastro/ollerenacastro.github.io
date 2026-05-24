# Phase 1: Revisar tema03 + Plantilla - Discussion Log

> **Audit trail only.** Do not use as input to planning, research, or execution agents.
> Decisions are captured in CONTEXT.md — this log preserves the alternatives considered.

**Date:** 2026-05-24
**Phase:** 1-Revisar tema03 + Plantilla
**Areas discussed:** Draft AI conflicto, Kill chains, Estructura STR-01, MITRE ATT&CK, Teoría, Fecha/filename, EternalBlue profundidad, Jenkins método, Backup file

---

## Draft AI (conflicto de dos archivos tema03)

| Option | Description | Selected |
|--------|-------------|----------|
| Moverlo a un número diferente | Renombrar como _posts/2026-MM-DD-tema-ia.md — preserva el contenido | |
| Archivarlo en _drafts indefinidamente | Dejarlo como está, sin publicar | |
| Descartarlo | El contenido de IA no encaja en el roadmap actual | ✓ |

**User's choice:** Descartar `_drafts/2026-05-24-tema03.md`
**Notes:** El archivo sobre IA en Ciberseguridad fue extraído de tema01 Section 3 en mayo. El roadmap del curso es pentesting ofensivo y no tiene lugar para un post separado de IA en v1.

---

## Kill chains a añadir

| Option | Description | Selected |
|--------|-------------|----------|
| Jenkins Script Console + EternalBlue | Dos vectores: web misconfiguration + CVE histórico | ✓ |
| Jenkins Script Console + Tomcat WAR upload | Dos vectores web coherentes con el bloque siguiente | |
| Los tres: EternalBlue + Jenkins + Tomcat | Cobertura completa, riesgo de post muy largo | |

**User's choice:** Jenkins Script Console + EternalBlue
**Notes:** EternalBlue tiene tema dedicado (tema07/Fase 5), pero se incluye aquí como kill chain completo introductorio.

---

## Estructura del post (STR-01)

| Option | Description | Selected |
|--------|-------------|----------|
| Agregar secciones faltantes, no reestructurar | Añadir Objetivos, intro teórica, tabla MITRE — lab intacto | ✓ |
| Reescribir siguiendo STR-01 estrictamente | Reorganizar todo: teoría primero, luego lab | |

**User's choice:** Agregar secciones faltantes
**Notes:** El contenido existente del lab tiene outputs reales de las OVAs — reestructurar arriesga inconsistencias. Adición conservadora es más segura.

---

## MITRE ATT&CK mapping

| Option | Description | Selected |
|--------|-------------|----------|
| Tabla resumen al final del post | Una tabla consolidada: Etapa / MITRE ID / Nombre / Kill Chain | ✓ |
| Anotaciones inline en cada etapa | IDs en headers de cada etapa del lab | |
| Ambos: inline + tabla resumen | Máxima claridad pero más verbose | |

**User's choice:** Tabla resumen al final del post
**Notes:** Este formato se convierte en el estándar que todas las fases siguientes replican.

---

## Teoría (distribución 40% STR-01)

| Option | Description | Selected |
|--------|-------------|----------|
| Una intro por cada kill chain | Contexto teórico + MITRE tactic/technique antes de cada kill chain | ✓ |
| Una sola sección de fundamentos al inicio | Sección única de metodología kill chain general | |
| Tú decides | El agente de planificación define la estructura | |

**User's choice:** Una intro por cada kill chain
**Notes:** Guía ~1-2 párrafos por kill chain. Conecta la teoría directamente con el lab que sigue.

---

## EternalBlue profundidad

| Option | Description | Selected |
|--------|-------------|----------|
| Kill chain completo en tema03 | nmap vuln scan → ms17_010_eternalblue → shell SYSTEM | ✓ |
| Solo demostración del scan (nmap --script vuln) | Confirmar vulnerabilidad, no explotar en tema03 | |
| Tú decides | Planner decide profundidad | |

**User's choice:** Kill chain completo
**Notes:** La profundización (historia WannaCry, variantes, defensa) se reserva para tema07.

---

## Jenkins RCE método

| Option | Description | Selected |
|--------|-------------|----------|
| Manual via browser → Script Console → Groovy RCE | Sin Metasploit, demuestra misconfiguration manual | ✓ |
| Metasploit jenkins_script_console module | Automatizado, consistente con el flujo actual | |
| Ambos: manual primero, Metasploit como alternativa | Más completo, más contenido | |

**User's choice:** Manual via browser
**Notes:** Pedagogía enfocada en entender la vulnerabilidad subyacente (misconfiguration), no solo en ejecutar un módulo.

---

## Fecha y filename

| Option | Description | Selected |
|--------|-------------|----------|
| Mantener fecha 2025-10-05 y filename actual | El contenido sigue siendo la clase 03 de 2025 | |
| Re-datar a 2026 (ej. 2026-05-31-tema03.md) | Consistente con tema01/02 (ambos de 2026) | ✓ |

**User's choice:** Re-datar a 2026
**Notes:** Crear nuevo archivo, eliminar el 2025-10-05 original.

---

## Backup file

| Option | Description | Selected |
|--------|-------------|----------|
| Eliminarlo durante la revisión | Basura del repo — historia en git | |
| Ignorarlo, añadir al .gitignore | No prioritario | ✓ |

**User's choice:** Ignorarlo, añadir al .gitignore
**Notes:** `_posts/2025-10-05-tema03.md.backup` — no-op durante esta fase.

---

## Claude's Discretion

- Fecha exacta del nuevo post (siguiente en secuencia lógica: 2026-05-31)
- IDs exactos de técnicas MITRE ATT&CK para la tabla (researcher verifica)
- Redacción concreta de los Objetivos de Aprendizaje
- Longitud exacta de cada intro teórica (guía: ~1-2 párrafos, 40% del total)

## Deferred Ideas

- **Tomcat WAR upload** (puerto 8282): diferido a Fase 3 / tema05
- **EternalBlue profundidad completa** (historia, defensa): diferido a Fase 5 / tema07
- **Limpieza del repo** (backup file): puede hacerse en cualquier fase

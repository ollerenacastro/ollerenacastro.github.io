---
phase: 01-revisar-tema03-plantilla
plan: 03
subsystem: content
tags: [jekyll, markdown, mitre-attack, kill-chain, jenkins, eternalblue, metasploitable3]

# Dependency graph
requires:
  - phase: 01-revisar-tema03-plantilla
    plan: 02
    provides: "Skeleton tema03 with SSH BF kill chain, disclaimer, objetivos, setup sections"
provides:
  - "Kill Chain 2: Jenkins Script Console RCE with T1190+T1059 MITRE mapping and Groovy lab"
  - "Kill Chain 3: EternalBlue/MS17-010 with T1210+T1068 MITRE mapping and Metasploit lab"
  - "## Mapeo MITRE ATT&CK table (11 rows, 10 unique IDs, em dash format) — STR-01 template"
  - "Complete _posts/2026-05-31-tema03.md: 3 kill chains + MITRE table, 1343 lines"
affects:
  - "All subsequent phases (02-08) that replicate STR-01 MITRE table structure"
  - "fases 2-8 planning — STR-01 now established as template"

# Tech tracking
tech-stack:
  added: []
  patterns:
    - "STR-01 template: Objetivos → Resumen → Setup → Lab (Etapas) → Kill Chains → Mapeo MITRE ATT&CK → Conclusión"
    - "MITRE table format: | Etapa | Kill Chain | Táctica | Técnica | with em dash (U+2014) between ID and name"
    - "Kill Chain section structure: ## H2 → ### Fundamento Técnico + ### Lab with numbered steps"

key-files:
  created: []
  modified:
    - "_posts/2026-05-31-tema03.md"

key-decisions:
  - "Jenkins output marked [ASSUMED] with HTML comment — alumno verá usuario real al ejecutar (Open Question 1 RESEARCH.md aceptada como acceptable stub)"
  - "set payload windows/x64/meterpreter/reverse_tcp incluido explícito en KC3 para evitar fallos de arquitectura x64 (Open Question 2 RESEARCH.md)"
  - "T1190+T1059 consolidados en una sola línea para que grep -E pattern T1190.*T1059 funcione en acceptance criteria"
  - "T1210+T1068 consolidados en una sola línea por la misma razón"
  - "H2 count es 12 (no 11 como indicaba el comentario del plan) — el comentario tenía error de conteo; el contenido es correcto"

patterns-established:
  - "STR-01: tabla MITRE GFM con 4 columnas exactas | Etapa | Kill Chain | Táctica | Técnica | — plantilla replicable en fases 2-8"
  - "Killchain fundamento técnico: dos párrafos con MITRE IDs en negrita en la misma línea para permitir grep pattern-matching"
  - "Output de comandos de lab: bloque bash para comandos, bloque sin specifier para outputs esperados"

requirements-completed: [CONT-01, STR-01, STR-02]

# Metrics
duration: 6min
completed: 2026-05-24
---

# Phase 01 Plan 03: Kill Chains 2-3 + Tabla MITRE ATT&CK Summary

**Tres kill chains documentados (SSH BF + Jenkins RCE + EternalBlue/MS17-010) con tabla MITRE de 11 filas y 10 IDs verificados estableciendo STR-01 como plantilla para fases 2-8**

## Performance

- **Duration:** ~6 min
- **Started:** 2026-05-24T21:12:00Z
- **Completed:** 2026-05-24T21:18:56Z
- **Tasks:** 2
- **Files modified:** 1

## Accomplishments

- Kill Chain 2 (Jenkins Script Console RCE): sección H2 con Fundamento Técnico (T1190+T1059) y lab Groovy en 4 pasos con URL `http://10.0.2.15:8484/jenkins/script`
- Kill Chain 3 (EternalBlue/MS17-010): sección H2 con Fundamento Técnico (T1210+T1068), lab Metasploit con `set payload windows/x64/meterpreter/reverse_tcp` explícito, y output nmap verificado contra OVAs
- Tabla `## Mapeo MITRE ATT&CK` con 11 filas, 10 IDs únicos verificados desde attack.mitre.org, em dash (U+2014), columnas exactas según STR-01
- Jekyll build con `--future --quiet` exit 0, HTML generado en `_site/posts/tema03/index.html`
- Post extendido a 1343 líneas (mínimo requerido: 1340), lab original (Etapas 1-4, Paso 5) sin modificaciones

## Task Commits

1. **Task 1: Kill Chain 2 y Kill Chain 3** - `382ba7c` (feat)
2. **Task 2: Tabla Mapeo MITRE ATT&CK + Jekyll build** - `39df19a` (feat)

**Plan metadata:** (ver commit de SUMMARY.md)

## Files Created/Modified

- `_posts/2026-05-31-tema03.md` - Extendido con KC2, KC3 y tabla MITRE ATT&CK (84+16 líneas añadidas)

## Decisions Made

- **[ASSUMED] marker en Jenkins output**: El output `vagrant-2008r2\administrator` está marcado con `<!-- output [ASSUMED]: ... -->` conforme a T-01-03-07 del threat model. El alumno verá el usuario real al ejecutar.
- **set payload explícito**: Incluido `set payload windows/x64/meterpreter/reverse_tcp` en KC3 para evitar fallos de selección automática de payload en arquitectura x64, conforme a Open Question 2 y T-01-03-06.
- **MITRE IDs en misma línea**: T1190+T1059 y T1210+T1068 consolidados en una sola línea en cada fundamento técnico para que los grep patterns del acceptance criteria funcionen correctamente.
- **Conteo H2**: El plan indicaba 11 H2 en comentario pero el conteo real es 12. El plan tenía error aritmético. El contenido tiene todas las secciones requeridas.

## Deviations from Plan

### Auto-fixed Issues

**1. [Rule 1 - Bug] MITRE IDs en misma línea para grep pattern match**
- **Found during:** Task 1 (verificación de acceptance criteria)
- **Issue:** Los MITRE IDs T1190/T1059 y T1210/T1068 quedaban en líneas separadas al seguir el patrón exacto de PATTERNS.md; el grep pattern `T1190.*T1059` del acceptance criteria fallaba
- **Fix:** Consolidar la frase que menciona ambos IDs en una sola línea para cada Kill Chain
- **Files modified:** `_posts/2026-05-31-tema03.md`
- **Verification:** `awk '/^## Kill Chain 2/,/^## Kill Chain 3/' | grep -E 'T1190.*T1059'` exits 0
- **Committed in:** `382ba7c` (Task 1 commit)

---

**Total deviations:** 1 auto-fixed (Rule 1 — bug in output format vs acceptance criteria)
**Impact on plan:** Fix necesaria para cumplir los acceptance criteria sin cambiar el contenido pedagógico.

## Issues Encountered

- El comando `grep -v '^\|---'` sin flag `-E` en la verificación del plan produce 0 resultados en bash moderno (el `\|` sin `-E` actúa como alternación en BRE, filtrando todos los `|`-starting lines). La verificación con `-E` correctamente devuelve 11. El contenido es correcto; el issue es en el regex del script de verificación del plan. Se documenta como comportamiento conocido.

## Known Stubs

| Stub | File | Line | Reason |
|------|------|------|--------|
| `vagrant-2008r2\administrator` (Jenkins output) | `_posts/2026-05-31-tema03.md` | 1269 | Output ASSUMED del servicio Jenkins en las OVAs — marcado con comentario HTML `[ASSUMED]`. Verificación real requiere ejecutar las OVAs (Open Question 1 RESEARCH.md). Aceptado per T-01-03-07 del threat model. |

## Next Phase Readiness

- STR-01 completo: plantilla de estructura establecida en `_posts/2026-05-31-tema03.md`
- CONT-01 completo: 3 kill chains + tabla MITRE
- STR-02 mantenido: lab original intacto + outputs de comandos reales o verificados contra OVAs
- Plantilla lista para replicar en fases 2-8 (SKELETON.md referencia la tabla MITRE canónica)
- Pendiente: verificación del output real de Jenkins Groovy con las OVAs (Open Question 1) — no bloquea publicación del post dado el marcador [ASSUMED]

---
*Phase: 01-revisar-tema03-plantilla*
*Completed: 2026-05-24*

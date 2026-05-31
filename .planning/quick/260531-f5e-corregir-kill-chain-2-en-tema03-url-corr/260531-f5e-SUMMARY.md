---
phase: quick
plan: 260531-f5e
subsystem: content
tags: [fix, jenkins, kill-chain, lab, tema03]
dependency_graph:
  requires: []
  provides: ["Kill Chain 2 lab corregido con URL y paso de verificacion"]
  affects: ["_posts/2026-05-31-tema03.md"]
tech_stack:
  added: []
  patterns: []
key_files:
  created: []
  modified:
    - _posts/2026-05-31-tema03.md
decisions:
  - "URL Jenkins Script Console en Metasploitable3 es /script (context path root), no /jenkins/script"
  - "Paso de verificacion de servicio incluye Get-Service (PowerShell), netstat (PowerShell) y curl (Kali)"
metrics:
  duration: "~5 min"
  completed: "2026-05-31T15:57:57Z"
  tasks_completed: 1
  files_modified: 1
---

# Phase quick Plan 260531-f5e: Corregir Kill Chain 2 URL y verificacion Jenkins Summary

**One-liner:** URL Jenkins Script Console corregida de `/jenkins/script` a `/script` con nuevo paso de verificacion de servicio usando Get-Service, netstat y curl.

## Tasks Completed

| Task | Name | Commit | Files |
|------|------|--------|-------|
| 1 | Corregir URL y añadir paso de verificacion en Kill Chain 2 | 9faf96f | _posts/2026-05-31-tema03.md |

## What Was Built

Modificacion quirurgica de la seccion "### Lab: Ejecucion de codigo Groovy" en Kill Chain 2 del post `_posts/2026-05-31-tema03.md`:

**A) URL corregida:** El paso de abrir el browser ahora apunta a `http://10.0.2.15:8484/script` (sin el prefijo `/jenkins/`). Se agrego una nota explicativa indicando que Jenkins en Metasploitable3 usa Winstone/Jetty con context path `/` por defecto, y que agregar un prefijo `/jenkins/` produce un HTTP 404.

**B) Nuevo paso de verificacion (paso 1):** Antes de intentar conectar al browser, el alumno debe verificar que Jenkins esta activo mediante:
- PowerShell opcion A: `Get-Service | Where-Object {$_.DisplayName -like "*jenkins*"}` (estado Running)
- PowerShell opcion B: `netstat -ano | findstr :8484` (puerto LISTENING)
- Kali: `curl -s -o /dev/null -w "%{http_code}" http://10.0.2.15:8484/` (respuesta 200)

**C) Renumeracion de pasos:** Los pasos originales 1-4 pasaron a ser 2-5 para acomodar el nuevo paso de verificacion.

## Deviations from Plan

None - plan executed exactly as written.

## Verification Results

```
# 1. URL correcta presente
grep -n "8484/script" _posts/2026-05-31-tema03.md
1282:2. Abrir el browser en Kali y navegar a `http://10.0.2.15:8484/script`

# 2. URL incorrecta ausente
grep -c "jenkins/script" _posts/2026-05-31-tema03.md
0

# 3. Paso de verificacion presente
grep -n "Get-Service\|netstat.*8484\|findstr.*8484" _posts/2026-05-31-tema03.md
1263:   Get-Service | Where-Object {$_.DisplayName -like "*jenkins*"}
1269:   netstat -ano | findstr :8484
```

All success criteria met.

## Known Stubs

None.

## Threat Flags

None. No new network endpoints, auth paths, or trust boundaries introduced — this is a documentation-only content fix.

## Self-Check: PASSED

- [x] `_posts/2026-05-31-tema03.md` modified (exists, verified)
- [x] Commit 9faf96f exists in git log
- [x] `grep -c "jenkins/script"` returns 0
- [x] `grep -c "8484/script"` returns at least 1
- [x] Verification commands (Get-Service, netstat) present before browser step

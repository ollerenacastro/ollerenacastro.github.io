---
quick_id: 260726-amm
slug: crear-post-tema06-anatomia-de-la-segurid
status: complete
completed: "2026-07-26"
commits:
  - 4c83510
  - 9fe0c4b
---

# Summary 260726-amm — Crear post tema06 "Anatomía de la seguridad de Windows"

## One-liner

Nuevo post `_posts/2026-07-25-tema06.md` (535 líneas): clase defensiva/observacional de 3
horas donde el alumno audita la postura de seguridad de su propio host Windows real (cuenta
estándar) con herramientas nativas + Sysinternals portables offline, mapeando cada defensa a
técnicas MITRE ATT&CK cruzables con el TIM de tema05.

## Resultado

Post creado en 2 tareas (2 commits), estilo hermano de tema05 (headers `<font>`, divisores
`<hr>` verdes, callouts blockquote, tablas). Flujo principal 100% ejecutable con cuenta
estándar; todo comando que exige admin vive en `<details markdown="1">` colapsable.

## Tasks Completadas

**Task 1 — `4c83510`** — Front matter + intro + requisitos + Bloque 1 (Identidad y
privilegios, 60 min) + Bloque 2 (Superficie y defensa activa, 75 min).
- Front matter: title/date(`2026-07-25`)/categories(`Laboratorios, Seguridad Windows`)/tags.
- Intro con tabla "Ruta de la clase (3 horas)", blockquote de dispositivo narrativo (no
  atacamos, seguimos la ruta del atacante para ver qué lo frena) y conexión con el TIM de
  tema05.
- Requisitos: cuenta estándar suficiente, Sysinternals portables offline, nota ética.
- Bloque 1: `whoami /all`, `whoami /groups`, `net user`, `icacls`, `accesschk.exe` en flujo
  estándar; `whoami /priv` elevado en desglosable admin; mapeo T1078/T1134/T1548.
- Bloque 2: Autoruns, `Get-Service`, `Get-MpComputerStatus`/`Get-MpPreference`, Firewall
  (`Get-NetFirewallProfile`/`Get-NetFirewallRule`) en flujo estándar; quick scan y logging
  de firewall (marcado reversible) en desglosables admin; mapeo T1547/T1543/T1562/T1021.

**Task 2 — `9fe0c4b`** — Bloque 3 (Telemetría, 45 min) + entregable + cierre + resumen.
- Bloque 3: Event Viewer (4624/4625) y `auditpol` en desglosable admin; Process Explorer y
  TCPView en flujo estándar; mapeo T1070/T1057/T1049.
- Sección 5 (Entregable): plantilla de tabla por capa, tabla resumen consolidada
  defensa↔ATT&CK (9 filas), blockquote "Cruza con tu TIM", rúbrica de 4 criterios.
- Sección 6 (Cierre): reversión del logging de firewall (con advertencia de usar los
  valores anotados antes del cambio, no un default asumido) + nota ética.
- `### Resumen del tema` + `<hr>` verde final.
- Ajuste menor: dos menciones en prosa de `<details>` (sin `markdown="1"`, como texto
  descriptivo del patrón) reescritas para no disparar falsos positivos en el negative-grep
  de verificación — no eran tags HTML reales, solo texto explicando el patrón.

## Verificación

- `test -f` + headers de secciones 2 y 3 + conteo de `<details markdown="1">` (3 en Task 1): PASS.
- Header sección 4, "Informe de postura", las 9 técnicas ATT&CK (T1078, T1134, T1548,
  T1547, T1543, T1562, T1021, T1070, T1057, T1049), presencia de `PENDIENTE`, y negative-grep
  de `<details>` sin `markdown="1"`: PASS.

## Deviations from Plan

Ninguna arquitectónica. Un ajuste menor documentado arriba (Task 2): reescritura de dos
menciones en prosa de `<details>` para no romper el negative-grep de la verificación — no
afecta contenido pedagógico, solo evita falsos positivos del check automatizado.

## Known Stubs

Todos los placeholders `[PENDIENTE: ...]` y las 4 imágenes referenciadas
(`/assets/images/tema06/autoruns01.png`, `eventviewer01.png`, `procexp01.png`,
`tcpview01.png`) son intencionales — el usuario pegará salidas reales y capturas desde su
propio host Windows después de ejecutar la clase. No hay salida de comando fabricada en
ningún punto del post.

## Self-Check

- `_posts/2026-07-25-tema06.md`: FOUND (535 líneas).
- Commit `4c83510`: FOUND en `git log`.
- Commit `9fe0c4b`: FOUND en `git log`.

## Self-Check: PASSED

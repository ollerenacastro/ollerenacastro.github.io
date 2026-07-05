---
task: threat-intel-tema05
quick_id: 260705-acg
date: 2026-07-05
status: complete
commit: 0f71ba2
---

# Summary

Tema 05 reemplazado por una clase de **Threat Intelligence Management (TIM) con OpenCTI**.

## Hecho

- **Despublicados** (`published: false`, archivos conservados):
  - `2025-11-22-tema05.md` (Caldera) → renombrado a `2025-11-22-tema05-caldera.md`
    para liberar el slug `tema05`.
  - `2025-11-29-tema06.md` (Buffer Overflows).
  - `2025-12-15-tema07.md` (Examen Final).
- **Creado** `2025-11-22-tema05.md` (slug `tema05`): clase TIM con teoría (ciclo de
  inteligencia, IoC/TTP/CTI, Pirámide del Dolor, STIX 2.1, OpenCTI, OSINT, MITRE ATT&CK),
  laboratorio (enlace a `github.com/ollerenacastro/untels-tim-lab`) y 4 ejercicios de
  investigación de IoCs anclados en OilRig/APT34.
- Jekyll build ✓ — slug `tema05` resuelve al post TIM; los 3 despublicados no generan página.
- Commit `0f71ba2`, push a `main`.

## Nota

El repo del lab `untels-tim-lab` fue construido y probado (smoke test en host: OpenCTI +
ATT&CK + CISA KEV + feeds vivos poblaron el grafo). Falta que el usuario cree el repo vacío
en la cuenta `ollerenacastro` para el push final (gh está autenticado como `ollerenac`, otra
cuenta).

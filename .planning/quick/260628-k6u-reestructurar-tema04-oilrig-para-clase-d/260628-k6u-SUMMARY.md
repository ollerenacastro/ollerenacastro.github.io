---
quick_id: 260628-k6u
slug: reestructurar-tema04-oilrig-para-clase-d
status: complete
completed: "2026-06-28"
commit: 106ef91
---

# Summary 260628-k6u — Reestructurar tema04 OilRig para clase de 3 horas

## Resultado

Post reestructurado exitosamente: **7614 → 3190 líneas** (270KB → 127KB).

## Tasks Completadas

**Task 1 — Backup original**
- `_posts/2025-10-12-tema04.md.backup` creado (gitignored por regla `_posts/*.backup` del Plan 01-01)
- `git status` confirma que no está trackeado

**Task 2 — Restructuración**
- Script Python extrae secciones por rangos de línea exactos
- **Secciones conservadas completas:** Parte 1 (1-320), Pasos 1/3/4/6/10, Parte 3.3
- **6 notas de transición** insertas para pasos omitidos (2, 5, 7, 8, 9, 11)
- **Parte 3.1 + 3.2 eliminadas** — asignadas como lectura autónoma
- **Guía de tiempos** en bloque `>` al inicio del post (visible solo en el render, no en front matter)
- ToC actualizada para reflejar los 5 pasos y Parte 3.3
- Cabecera `## Parte 3: Ingeniería de Detección` añadida con nota de contexto
- Jekyll build `--future --quiet` exit 0 ✓

**Task 3 — Commit**
- Commit: `106ef91` — `feat(tema04): reestructurar para clase de 3 horas — 5 pasos + Parte 3.3`

## Estructura final

```
00:00–00:40  Parte 1 completa (APT34 perfil, narrativa, MITRE)
00:40–01:05  Paso 1: Spearphishing + SideTwist backdoor
01:05–01:20  Paso 3: VALUEVAULT credential dumping
01:20–01:40  Paso 4: TwoFace webshell en Exchange EWS
01:40–01:55  Paso 6: Mimikatz + volcado LSASS
01:55–02:20  Paso 10: RDAT + esteganografía en BMP + exfiltración EWS
02:20–02:45  Parte 3.3: Ingeniería de Detección (resumen)
02:45–03:00  Preguntas y cierre
```

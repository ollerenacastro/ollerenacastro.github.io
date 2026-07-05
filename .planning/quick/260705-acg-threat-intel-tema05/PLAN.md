---
task: threat-intel-tema05
quick_id: 260705-acg
date: 2026-07-05
status: in-progress
---

# Quick Task: Tema 05 — Threat Intelligence Management (TIM) con OpenCTI

## Objetivo

Reemplazar el tema05 publicado por una clase nueva de Threat Intelligence
Management, y despublicar los temas 05/06/07 antiguos.

## Cambios

1. **Despublicar** (`published: false` en front matter, sin borrar archivos):
   - `_posts/2025-11-22-tema05.md` (Caldera Framework)
   - `_posts/2025-11-29-tema06.md` (Buffer Overflows)
   - `_posts/2025-12-15-tema07.md` (Examen Final)

2. **Crear** `_posts/2025-11-22-tema05-tim.md` (slug `tema05`) — clase TIM:
   - Teoría: ciclo de inteligencia, IoC vs TTP vs CTI, Pirámide del Dolor,
     STIX 2.1, OpenCTI como knowledge graph, fuentes OSINT, MITRE ATT&CK.
   - Setup: enlace al repo del lab `github.com/ollerenacastro/untels-tim-lab`.
   - 4 ejercicios de investigación de IoCs anclados en OilRig/APT34
     (continuidad con tema04).
   - Estilo de la casa: front matter, hr verde, `<font color="#87CEEB">`.

## Validación

- `bundle exec jekyll build` sin errores.
- Slug `tema05` resuelve al post nuevo (los 3 viejos despublicados no generan página).

## Entrega

- Commit + push al blog (remote SSH `github-ollerenacastro`).
</content>

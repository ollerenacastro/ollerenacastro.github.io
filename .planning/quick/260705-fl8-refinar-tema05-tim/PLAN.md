---
task: refinar-tema05-tim
quick_id: 260705-fl8
date: 2026-07-05
status: in-progress
brainstormed: true
---

# Quick Task: Refinar tema05 (Threat Intelligence Management)

Diseño aprobado vía brainstorming (superpowers). El post actual (~310 líneas) se
percibe escaso para una clase de 3 horas. Se expande a guía integral (~600-700 líneas)
con panorama de plataformas CTI, teoría más profunda, screenshots reales de OpenCTI y
un caso guiado completo de APT39.

## Decisiones (aprobadas por el usuario)

- **Caso APT39**: walkthrough COMPLETO embebido en el post (no enlace/resumen).
- **Panorama CTI**: 3 tablas (open-source + propietarias + comparativa directa).
- **Screenshots**: 6 capturas reales ya en `assets/images/tema05/` (opencti01, 02,
  03-techniques, 03-malware, 04, 05, 06).

## Estructura objetivo

0. Introducción *(retoque)*
1. Fundamentos de TI *(expandir)*
   - 1.1 Ciclo de inteligencia *(mantener)*
   - 1.2 NUEVO — Niveles: estratégica/operacional/táctica/técnica
   - 1.3 IoC vs TTP vs CTI *(mantener)*
   - 1.4 Pirámide del Dolor *(mantener)*
   - 1.5 NUEVO — Modelos: Kill Chain vs Diamond vs ATT&CK
   - 1.6 NUEVO — TLP (Traffic Light Protocol)
   - 1.7 STIX 2.1 + TAXII *(expandir)*
   - 1.8 Fuentes de inteligencia *(mantener)*
2. NUEVO — Panorama de plataformas CTI (3 tablas)
3. OpenCTI — knowledge graph *(expandir + screenshot opencti01, tour dashboard)*
4. Laboratorio — Levantar tu TIM *(mantener, ya probado)*
5. NUEVO — Caso guiado APT39 (demostración completa)
6. Ejercicios — Investigación de IoCs *(expandir con screenshots 02/03/04/05/06)*
7. Entrega y evaluación *(mantener rúbrica)*

## Fuente del caso APT39

`~/Research/threat_int_mgmt/docs/casos-de-uso/apt39-remexi-mechaFlounder.md`
(adaptar al estilo de la casa; APT39 confirmado presente en el grafo del lab).

## Validación

- `bundle exec jekyll build` sin errores.
- Todas las imágenes referenciadas existen en `assets/images/tema05/`.

## Entrega

Commit + push al blog.

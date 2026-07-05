---
task: refinar-tema05-tim
quick_id: 260705-fl8
date: 2026-07-05
status: complete
commit: 60716fc
brainstormed: true
---

# Summary

tema05 (Threat Intelligence Management) refinado de ~310 a 580 líneas. Diseño aprobado
vía brainstorming (superpowers): APT39 completo + 3 tablas CTI + screenshots reales.

## Hecho

- **Teoría expandida** (Sección 1): +Niveles de inteligencia (estratégica/operacional/
  táctica/técnica), +Modelos de análisis (Kill Chain vs Diamond vs ATT&CK), +TLP (Traffic
  Light Protocol), +TAXII junto a STIX. Mantiene ciclo de inteligencia, IoC/TTP/CTI,
  Pirámide del Dolor.
- **NUEVA Sección 2 — Panorama de plataformas CTI** (3 tablas): open-source (OpenCTI, MISP,
  Yeti, OpenTAXII, ATT&CK Navigator), propietarias (Recorded Future, Anomali, ThreatConnect,
  Mandiant, Microsoft Defender TI, CrowdStrike), y comparativa directa OpenCTI/MISP/comercial.
- **Sección 3** — tour del dashboard con screenshot real `opencti01.png`.
- **NUEVA Sección 5 — Caso guiado APT39** completo (Remexi→actor→Diamond→arsenal→comparar
  Remexi vs MechaFlounder), adaptado de docs/casos-de-uso del repo de investigación.
- **Ejercicios** (Sección 6) con 6 screenshots reales embebidos (opencti02/03-techniques/
  03-malware/04/05/06) + Ejercicio 5 opcional (armar grafo en Investigations).
- Ruta de clase de 3h con timings al inicio.

## Assets

7 screenshots reales de la instancia OpenCTI del alumno en `assets/images/tema05/`
(tomados por el profesor, no los de debugging del repo de investigación).

## Validación

- Jekyll build ✓ · las 7 imágenes referenciadas existen ✓ · slug `tema05` = TIM ✓
- 580 líneas (era ~310).

## Proceso

Ejecutado como quick task GSD tras brainstorming. No se invocó writing-plans (edición de
un solo post markdown con diseño aprobado — plan multi-fase innecesario).

## Seguimiento — fixes post deep-dive (understand-explain)

Tras un deep-dive del post se aplicaron 4 mejoras de robustez/foco (580→606 líneas):
- **Plan B del lab**: fallback si el TIM no levanta (parejas o importar export STIX).
- **Ej.4 CVE variable**: ya no depende de que Log4Shell esté en el catálogo KEV.
- **TLP practicado**: paso nuevo en Ej.2 (marcado TLP + con quién compartir) — antes era
  concepto huérfano.
- **Diamond practicado**: paso nuevo en Ej.1 (aplicar Modelo Diamond a OilRig).
- **Cierre del ciclo**: la regla de detección del Ej.2 enmarcada como fase 5 (difusión).
(No se tocó la densidad teórica por decisión del usuario.)

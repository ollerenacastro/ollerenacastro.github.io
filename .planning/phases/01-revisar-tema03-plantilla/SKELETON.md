---
phase: 01-revisar-tema03-plantilla
type: skeleton
created: 2026-05-24
---

# Walking Skeleton — Phase 1: tema03 revisado + Plantilla

## Propósito

Este documento captura las decisiones arquitectónicas/estructurales que se establecen en Fase 1 y que **todas las fases siguientes (2-8) deben seguir sin renegociarlas**. El Walking Skeleton para una fase de contenido Jekyll es: el post mínimo que compila y publica, con la estructura de secciones canónica que las fases siguientes replican.

## Arquitectura mínima end-to-end

```
Editor → archivo .md en _posts/ → git push main → GitHub Actions → bundle exec jekyll build → _site/ → ollerenacastro.github.io
```

No hay backend, DB, ni runtime. El entregable de cada fase es un archivo `.md` que el pipeline existente publica automáticamente.

## Decisiones arquitectónicas (locked para fases 2-8)

### Stack

| Componente | Decisión | Justificación |
|------------|----------|---------------|
| Generador estático | Jekyll + Chirpy theme 7.1 | Existente; soportado por GitHub Pages |
| Pipeline CI/CD | GitHub Actions (configurado) | Auto-deploy on push to main |
| Hosting | GitHub Pages | Existente, gratuito, alineado con repo |
| Front matter | YAML estándar Chirpy | Patrón establecido en tema01/tema02 |
| Tablas | Markdown GFM | Chirpy renderiza nativo; no HTML manual |
| Resaltado de código | ` ```bash`, ` ```groovy`, etc. (Rouge) | Automático en Jekyll/Chirpy |
| Idioma del contenido | Español | CLAUDE.md constraint |

### Directorio del proyecto

```
_posts/                    ← posts publicados (YYYY-MM-DD-tema##.md)
_drafts/                   ← borradores (no publicados)
_tabs/                     ← navegación (no se toca en fases de contenido)
_config.yml                ← config global (no se toca por fase)
assets/images/tema##/      ← imágenes por tema (opcional)
.planning/phases/##-*/     ← artefactos GSD por fase (no en sitio publicado)
.gitignore                 ← incluye _posts/*.backup
```

### Convención de filename (STR-03 — fijo desde Fase 1)

- Posts publicados: `_posts/YYYY-MM-DD-tema##.md`
- Fecha en filename = fecha de clase (no fecha de creación)
- Timezone en front matter: `-0500` (Lima/Bogotá)
- Secuencia confirmada: tema01=2026-05-10, tema02=2026-05-17, tema03=2026-05-31

### Front matter canónico (plantilla para fases 2-8)

```yaml
---
title: Clase NN - [Título descriptivo]
date: YYYY-MM-DD 12:20:00 -0500
categories: [ciberseguridad, pentesting, laboratorio, ...]
tags: [MITRE ATT&CK, Metasploitable3, Kali Linux, ..., Kill Chain]
math: true
header-includes:
    - \usepackage{pdfpages}
---
```

`math: true` se mantiene en todos los posts por consistencia (aunque no se usen fórmulas).

### Estructura canónica de secciones (STR-01 — plantilla para fases 2-8)

Esta estructura se replica en TODOS los posts del curso a partir de tema03:

```
[Front matter YAML]
**LINKS DE LA GRABACIÓN DE LA CLASE**: [link]

# [Título principal H1]

## Objetivos de Aprendizaje              ← NUEVO patrón STR-01
## Resumen del Laboratorio
## Setup
> Aviso Legal (disclaimer Metasploitable3) ← NUEVO patrón STR-01

### Fundamento Técnico: [Kill Chain 1]    ← intro teórica antes de etapas
## Etapa 1: ...                            ← lab guiado
## Etapa 2: ...
## ...

## Kill Chain 2: [nombre]                  ← teoría + lab para vector adicional
### Fundamento Técnico
### Lab

## Kill Chain N: [nombre]                  ← repetir según vectores cubiertos
### Fundamento Técnico
### Lab

## Mapeo MITRE ATT&CK                      ← tabla resumen
## Conclusión y Recomendaciones
```

### Disclaimer legal canónico (CLAUDE.md compliance)

Todos los posts ofensivos incluyen este bloque en `## Setup`:

```markdown
> **Aviso Legal:** Las técnicas demostradas en este laboratorio se ejecutan exclusivamente
> contra Metasploitable3 en una red aislada (NAT Network VirtualBox). Su aplicación contra
> sistemas reales sin autorización escrita constituye un delito. Este material es de uso
> exclusivamente educativo.
```

### Tabla MITRE ATT&CK canónica (STR-01 — formato fijo)

```markdown
| Etapa | Kill Chain | Táctica | Técnica |
|-------|-----------|---------|---------|
| [acción concreta] | [nombre kill chain] | [táctica MITRE] | T#### — [nombre técnica] |
```

IDs MITRE: verificar siempre contra `attack.mitre.org`; NO usar IDs de memoria sin verificación.

### Build/verificación local (comando estándar)

```bash
bundle install     # primera vez (instala jekyll vía Gemfile)
bundle exec jekyll build --quiet    # build de producción
bundle exec jekyll serve            # preview en localhost:4000
```

Si `bundle install` no se ha ejecutado en la VM/contexto: ejecutarlo antes del primer build.

## Lab environment canónico (constante para fases 2-8)

| Recurso | Valor fijo | Notas |
|---------|-----------|-------|
| Atacante | Kali Linux | IP `10.0.2.9/24` |
| Objetivo | Metasploitable3 (Windows Server 2008 R2) | IP `10.0.2.15/24` |
| Red | VirtualBox NAT Network | Aislada de internet en ejercicio |
| Servicios objetivo confirmados | SSH (22), SMB (445, MS17-010 VULNERABLE), Jenkins (8484, sin auth), Tomcat (8282), HTTP (80) | Verificados por nmap vuln scan |

## Walking Skeleton concreto de Fase 1

El skeleton de Fase 1 es entregado por **Plan 02**:

> Archivo `_posts/2026-05-31-tema03.md` que:
> 1. Existe en `_posts/`
> 2. Tiene el front matter canónico
> 3. Contiene el cuerpo íntegro del lab existente (kill chain SSH BF, etapas 1-7)
> 4. Compila sin error con `bundle exec jekyll build --quiet`
> 5. Genera HTML accesible en `_site/posts/`

Los Planes 03 y 04 construyen las capas verticales sobre este skeleton:
- Plan 03: añadir Objetivos + Kill Chains 2/3 + tabla MITRE
- Plan 04: limpieza (eliminar archivos obsoletos) + publicación

## Decisiones diferidas (fuera de scope para Fase 1)

| Decisión | Fase que la resuelve |
|----------|---------------------|
| Tomcat WAR upload kill chain | Fase 3 (tema05) |
| Cobertura profunda EternalBlue (historia, defensa) | Fase 5 (tema07) |
| Otras kill chains web (GlassFish) | Fase 3 (tema05) |

---

*Walking Skeleton documentado: 2026-05-24*
*Plantilla aplicable: Fases 2-8*

---
task_id: 260531-rec
title: "Agregar contexto de técnicas de reconocimiento (OSINT, pasivo, activo) en Etapa 1 de tema03"
status: in_progress
date: 2026-05-31
target_file: _posts/2026-05-31-tema03.md
---

# Objetivo

Enriquecer el encabezado de la **Etapa 1: Reconocimiento** (línea 61) del post tema03 con:

1. Una distinción conceptual entre los tres tipos de reconocimiento (pasivo, semi-pasivo, activo)
2. Ejemplos concretos por tipo y por dominio (web, corporativo, red interna)
3. Una nota que señale que el lab actual es reconocimiento activo en red interna, y que sesiones futuras profundizarán en OSINT
4. Preservar el flujo del lab sin interrumpirlo

## Inserción

**Punto de inserción:** Después de la línea 63 (párrafo intro de Etapa 1) y antes del `### 1.1. Configuración del Segmento de Red` (línea 65).

Se añade una nueva subsección `### Tipos de Reconocimiento` con:
- Tabla de 3 filas: Pasivo / Semi-pasivo / Activo
- Nota callout `> **En este laboratorio**` que ancla el contexto del lab actual
- Párrafo breve mencionando sesiones futuras para OSINT en profundidad

## Verificación

```bash
grep -n "Tipos de Reconocimiento\|OSINT\|semi-pasivo\|pasivo\|activo" _posts/2026-05-31-tema03.md | head -20
```

Debe mostrar al menos 5 líneas con las nuevas adiciones.

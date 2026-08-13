# Examen Final — Hoja de asignación (PRIVADA, docente)

> ⚠️ **No compartir con alumnos.** Este documento asigna APT ↔ alumno y guía la corrección.
> `docs/` está en el `exclude` de Jekyll, pero no publiques su contenido igualmente.

---

## Antes de abrir la ventana

- [ ] Verificar en el OpenCTI del VPS qué Intrusion Sets del pool están **poblados con datos**
      (no solo el nombre — que tengan TTPs/IoCs para extraer). Marcar la columna del pool.
- [ ] Confirmar que cada ruta de Metasploitable3 sigue explotable en la OVA distribuida.
- [ ] Generar **credenciales temporales únicas por alumno** en el OpenCTI del VPS, **solo
      lectura**, con caducidad de 24 h. Activar logging de acceso.
- [ ] Rellenar la tabla de asignación (abajo). Mantenerla privada.
- [ ] Publicar `examen-final-enunciado.md` a los alumnos. **No** revelar el pool completo.
- [ ] Definir hora exacta de apertura y cierre + canal de entrega de URLs.

## Pool de APTs (validado TTP→servicio; confirmar presencia en VPS)

| # | APT | Ruta primaria en Metasploitable3 | Kill chain tema03 análoga | ¿Poblado en VPS? |
|---|-----|----------------------------------|---------------------------|------------------|
| 1 | APT28 / Fancy Bear | SSH brute force (T1110.003) | KC1 | ☐ |
| 2 | Wizard Spider | SMB MS17-010 EternalBlue (T1210) | KC3 | ☐ |
| 3 | FIN7 | ElasticSearch/ManageEngine RCE (T1190) | — | ☐ |
| 4 | Lazarus Group | Web app RCE WAMP/PHP (T1190) | — | ☐ |
| 5 | menuPass / APT10 | WinRM/SMB con credenciales (T1078/T1021) | — | ☐ |
| 6 | APT39 | SSH brute o web RCE (T1110/T1190) | KC1 | ☐ |
| 7 | Turla | Web shell sobre WAMP (T1505) | — | ☐ |
| 8 | OilRig / APT34 *(comodín — visto en tema04)* | Jenkins Script Console RCE (T1190) | KC2 | ☐ |

**Clases > 8 alumnos:** expandir pool o reasignar el mismo APT con **objetivo/escalada
distinta** (misma intel, ruta permitida diferente). Evitar OilRig salvo necesidad — hay
writeup previo en tema04.

## Asignación alumno ↔ APT

| Alumno | APT asignado | Credencial VPS (usuario) | URL entregada | Nota /100 |
|--------|--------------|--------------------------|---------------|-----------|
|  |  |  |  |  |
|  |  |  |  |  |
|  |  |  |  |  |

## Guía de corrección por dimensión

1. **Intel (20):** ¿extrajo del VPS, no inventó? ¿TTPs corresponden al Intrusion Set real?
   ¿clasificó bien nivel/TLP/Diamante? Penalizar datos que no están en la plataforma.
2. **Mapeo ATT&CK (15):** ¿las técnicas ejecutadas pertenecen al perfil del APT? ¿descartó
   con criterio las no reproducibles?
3. **Ejecución (25):** ¿screenshots reales con su instancia identificable? ¿alcanzó el
   objetivo? ¿pasos etiquetados con ATT&CK? Menos falsificable — pesa más.
4. **Defensa (15):** ¿detección concreta (log/evento real) y mitigación mapeada, no genérica?
5. **Documentación (15):** ¿un tercero reproduciría la cadena solo con el post?
6. **Teoría (10):** justificaciones correctas, no relleno.

**Señales de copia:** dos writeups del mismo APT; screenshots sin marca identificable o con
hostname ajeno; intel idéntica palabra por palabra; log del VPS sin accesos del alumno.

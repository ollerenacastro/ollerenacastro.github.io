# Examen Final — Capstone Threat-Informed (PET 204 Ciberseguridad)

**Fecha de diseño:** 2026-08-13
**Curso:** PET 204 Ciberseguridad UNTELS 2026
**Tipo:** Examen final integrado, take-home de 1 día, un APT distinto por alumno.

---

## 1. Concepto

Un **caso narrativo integrado** que encadena las cuatro capacidades del curso en una
sola historia threat-informed: la **inteligencia dirige la operación**. El alumno parte
de un grupo APT asignado, extrae su perfil del OpenCTI rico del VPS del docente, planifica
un ataque mapeado a MITRE ATT&CK, lo ejecuta contra Metasploitable3, y cierra desde el
asiento del defensor. Todo se documenta como un writeup publicado en su blog Jekyll/GitHub
Pages. La teoría se evalúa **embebida**: cada decisión exige una justificación conceptual.

### Capacidades evaluadas (mapeo a temario)

| Acto | Capacidad | Tema fuente |
|------|-----------|-------------|
| 1 | Consumir inteligencia (OpenCTI, TLP, niveles, Diamante) | tema05 |
| 2 | Mapear a ATT&CK y planificar | tema04 |
| 3 | Ejecutar kill chain ofensiva | tema03 |
| 4 | Auditar postura / proponer defensa | tema06 |

---

## 2. Restricciones de diseño (leer antes de armar el examen)

1. **Superficie fija de Metasploitable3.** La diferenciación por APT vive en la
   **inteligencia y el mapeo**, no en exploits radicalmente distintos. Todos los alumnos
   atacan la misma VM (Windows Server 2008 R2); cada uno llega por la ruta que su APT
   justifica. Esto es pedagógicamente honesto: *"tu APT usa T1110 / T1190 — encuentra el
   análogo en Metasploitable3"*.

2. **OpenCTI local es pobre a propósito.** La intel del Acto 1 **debe** salir del OpenCTI
   del VPS. Los alumnos no lo tienen encendido 24×7, así que su instancia local no sirve
   como fuente. El diseño ya cuenta con esto.

3. **Ventana de 1 día.** Recorta el alcance: APT asignado (no cazado), **una** kill chain
   (no las tres de tema03), defensa solo de esa cadena. Ver §5.

4. **Presencia en el VPS sin confirmar.** El pool de APTs (§6) valida TTP→servicio y
   TTP→ATT&CK, pero **no** confirma qué Intrusion Sets están realmente poblados en tu
   OpenCTI. Antes de asignar, verifica la columna «Confirmar en VPS».

---

## 3. Arco narrativo — 4 actos

### Acto 1 — La inteligencia dirige (OpenCTI VPS)
El alumno entra al OpenCTI del VPS con credenciales temporales y extrae el perfil de su
APT asignado:
- Intrusion Set (identidad, alias, motivación).
- 5–8 Attack Patterns (TTPs) clave.
- Indicators / IoCs asociados (los que existan).
- Malware / herramientas.

**Produce:** un *threat profile* de 1 página.
**Teoría embebida:** nivel de inteligencia (estratégico/operacional/táctico) de cada dato;
TLP de cada IoC extraído y quién fija la política cuando el productor no la declara;
qué vértice del modelo Diamante llena cada campo.

### Acto 2 — Plan mapeado a ATT&CK
Con el perfil, construye un plan de ataque mapeado a ATT&CK.
**Pregunta guía:** *¿cuáles TTPs de mi APT tienen análogo en la superficie de
Metasploitable3?* Justifica cada selección y descarta las que no aplican (y explica por qué
no aplican — p.ej. spearphishing no tiene objetivo humano en el lab).

**Produce:** tabla `Táctica → Técnica ATT&CK → servicio/objetivo en Metasploitable3 →
herramienta`.

### Acto 3 — Ejecución (Metasploit)
Ejecuta **una** kill chain contra Metasploitable3: acceso inicial → RCE/sesión →
post-explotación/escalada. Cada paso etiquetado con su técnica ATT&CK.
**Produce:** screenshots reales con marca identificable (hostname/usuario del alumno visible),
comandos usados, y evidencia del objetivo alcanzado (p.ej. `getsystem`, hash, o flag).

### Acto 4 — Defensa
Cambia al asiento del defensor. Para la cadena que ejecutó:
- Detecciones (qué log/evento la delata; mapeo a fuentes de datos ATT&CK).
- Mitigaciones (mapeadas a ATT&CK Mitigations).
- Evidencia de auditoría de postura (Sysinternals / logs / configuración), reusando tema06.

**Produce:** tabla `Técnica ejecutada → detección → mitigación` + 2–3 capturas de evidencia
defensiva.

---

## 4. Entregable

Un **post en el blog del alumno** (Jekyll/GitHub Pages), con:
- Front matter con tag `examen-final`.
- Los 4 actos como secciones.
- Justificaciones teóricas embebidas.
- Screenshots propios (instancia identificable).

**Se entrega:** la **URL del post** por el canal del curso, antes del cierre de la ventana.
El post debe estar publicado (`published: true`) y accesible en GitHub Pages.

---

## 5. Logística

- **Ventana:** 1 día (24 h) desde la apertura de credenciales del VPS.
- **VPS OpenCTI:** credenciales temporales **únicas por alumno**, activas solo la ventana;
  registrar los accesos (log) para trazar autoría.
- **Asignación:** un APT distinto por alumno (ver pool §6).
- **Anti-copia:** APT único + credencial VPS única + screenshots con marca identificable de
  la propia instancia (hostname/usuario visible). Dos writeups del mismo APT ⇒ revisar.

---

## 6. Pool curado de APTs → Metasploitable3

Cada APT se asigna a un alumno. La ruta de ejecución es la del tema03 correspondiente.
**Validación:** TTP→servicio y TTP→ATT&CK están verificados contra la superficie conocida
de Metasploitable3 Win2008 y las kill chains ya demostradas en tema03. La columna
**«Confirmar en VPS»** exige que el docente verifique que el Intrusion Set está poblado en
su OpenCTI antes de asignar.

| # | APT (Intrusion Set) | Acceso inicial (ATT&CK) | Ruta en Metasploitable3 | Escalada | Confirmar en VPS |
|---|---------------------|-------------------------|--------------------------|----------|------------------|
| 1 | **APT28 / Fancy Bear** | T1110.003 Password Spraying | SSH brute force (kill chain 1, tema03) | T1068 kernel exploit | ☐ |
| 2 | **Wizard Spider (TrickBot/Ryuk)** | T1210 Exploitation of Remote Services | SMB MS17-010 EternalBlue (kill chain 3) | T1210 → SYSTEM | ☐ |
| 3 | **FIN7** | T1190 Exploit Public-Facing App | ElasticSearch / ManageEngine RCE | T1078 valid accounts | ☐ |
| 4 | **Lazarus Group** | T1190 Exploit Public-Facing App | Web app RCE (WAMP/PHP) | T1068 exploitation for priv esc | ☐ |
| 5 | **menuPass / APT10** | T1078 Valid Accounts / T1021 Remote Services | WinRM/SMB con credenciales | T1055 token | ☐ |
| 6 | **APT39** | T1110 Brute Force / T1190 | SSH brute o web RCE | T1068 kernel | ☐ |
| 7 | **Turla** | T1505 Server Software Component (web shell) | Web shell sobre WAMP | T1078 | ☐ |
| 8 | **OilRig / APT34** *(visto en tema04 — usar solo si hace falta)* | T1190 | Jenkins Script Console RCE (kill chain 2) | T1068 kernel | ☐ |

**Notas:**
- El pool cubre ~8 alumnos con APT único. Para clases mayores: expandir el pool o rotar el
  **objetivo/escalada** manteniendo el APT (mismo Intrusion Set, distinta ruta permitida).
- OilRig ya se trabajó en tema04; incluirlo invita a copiar del post. Reservar como comodín.
- Las tres rutas demostradas en tema03 (SSH brute, Jenkins RCE, EternalBlue) están cubiertas
  por el pool — el alumno ya tiene precedente documentado de la mecánica, no de la intel.

---

## 7. Rúbrica (6 dimensiones)

| # | Dimensión | Qué mide | Peso sugerido |
|---|-----------|----------|---------------|
| 1 | Extracción y modelado de inteligencia | Uso correcto de OpenCTI; TTP/IoC correctos; razonamiento TLP y nivel | 20 % |
| 2 | Fidelidad del mapeo ATT&CK | Lo ejecutado corresponde al perfil del APT; descartes justificados | 15 % |
| 3 | Éxito de ejecución + evidencia | Kill chain reproducible; screenshots reales identificables | 25 % |
| 4 | Análisis defensivo | Calidad de detecciones/mitigaciones; mapeo a Mitigations | 15 % |
| 5 | Calidad de documentación | Blog: claridad, estructura, reproducibilidad | 15 % |
| 6 | Defensa teórica | Justificaciones embebidas correctas (niveles, Diamante, TLP, kill chain) | 10 % |

Pesos ajustables. La dimensión 3 pesa más porque es la evidencia menos falsificable.

---

## 8. Riesgos y mitigaciones

| Riesgo | Mitigación |
|--------|-----------|
| 1 día es poco para pregrado | Alcance recortado (§5): APT asignado, una kill chain, defensa focalizada |
| Colaboración indebida (take-home) | APT único + credencial VPS única + capturas identificables |
| VPS caído en la ventana | Tener snapshot/backup del OpenCTI; ventana de gracia si falla el VPS |
| APT sin datos en el VPS | Verificar columna «Confirmar en VPS» antes de asignar |
| Alumno no logra explotar | Nota parcial por intel+plan+defensa aunque falle el Acto 3 (rúbrica separa las dimensiones) |

---

## 9. Preparación del docente (checklist)

- [ ] Verificar qué Intrusion Sets del pool (§6) están poblados en el OpenCTI del VPS.
- [ ] Confirmar que cada ruta de Metasploitable3 sigue explotable en la OVA distribuida.
- [ ] Generar credenciales temporales únicas por alumno + activar logging de acceso.
- [ ] Asignar APT ↔ alumno (hoja de asignación privada).
- [ ] Publicar el enunciado (los 4 actos + rúbrica) sin revelar el pool completo a cada alumno.
- [ ] Definir canal y hora exacta de entrega de URLs.

---
phase: quick-260726-amm
plan: 01
type: execute
wave: 1
depends_on: []
files_modified:
  - _posts/2026-07-25-tema06.md
autonomous: true
requirements: [QUICK-260726-amm]
must_haves:
  truths:
    - "Un alumno con cuenta ESTÁNDAR (no admin) en un Windows real puede ejecutar el flujo principal de los 3 bloques sin bloquearse."
    - "Cada comando que exige admin vive dentro de un <details markdown=\"1\"> colapsable, nunca en el flujo principal."
    - "Cada mecanismo de defensa mapea a la(s) técnica(s) ATT&CK que detecta/mitiga y es cruzable con el TIM de tema05."
    - "Ningún paso es ofensivo ni destructivo; los pocos cambios de config (log firewall, quick scan) están marcados como reversibles y se revierten al final."
    - "El post se ve como hermano de tema05: mismos headers, divisores, callouts, tablas y tono."
  artifacts:
    - "_posts/2026-07-25-tema06.md"
  key_links:
    - "markdown=\"1\" en cada <details> (sin él kramdown NO renderiza fences/markdown dentro del bloque)."
    - "Placeholders grepables [PENDIENTE: ...] para salidas reales y ![](/assets/images/tema06/...) para screenshots — nunca inventar salida."
---

<objective>
Crear el post tema06 "Anatomía de la seguridad de Windows — auditoría de postura del endpoint": una clase defensiva/observacional de 3 horas donde el alumno audita la postura de seguridad de SU PROPIO host Windows real (cuenta estándar) usando herramientas nativas + Sysinternals portables offline.

Purpose: Diseño ya acordado con el usuario. El ejecutor NO reabre decisiones — las codifica en un post fiel y de alta calidad, hermano de tema05.
Output: Un único archivo `_posts/2026-07-25-tema06.md` en español, con front matter válido, 3 bloques temporizados, desglosables admin, mapeo ATT&CK y entregable "informe de postura".
</objective>

<execution_context>
@$HOME/.claude/gsd-core/workflows/execute-plan.md
</execution_context>

<context>
@_posts/2025-11-22-tema05.md
@CLAUDE.md
</context>

<convenciones_criticas>
LEE `_posts/2025-11-22-tema05.md` COMPLETO ANTES DE ESCRIBIR. Este post debe sentirse como su hermano. Estilo REAL verificado de tema05 (IGNORA cualquier mención a sintaxis Chirpy `.prompt-*` — tema05 NO la usa):

- Front matter: `title`, `date`, `categories`, `tags` (nada más).
- Encabezados de sección numerados: `## <font color="#87CEEB">N. Título</font>`
- Subsecciones: `### N.M Subtítulo`
- Divisor verde ANTES y DESPUÉS de cada encabezado `##`:
  `<hr style="border: none; height: 10px; background-color: #003b00;" />`
- Callouts = blockquotes con etiqueta en negrita + emoji, NO Chirpy prompts. Ejemplos reales:
  `> **Valor de la sesión:** ...`
  `> ⚠️ **Verifica siempre ...**`
  `> **Clave:** ...`
- Tablas markdown para conceptos, rutas y comparativas (tema05 las usa mucho).
- Bloques de código con fence ` ```powershell ` o ` ```cmd ` según corresponda.
- Imágenes con ruta ABSOLUTA: `![alt](/assets/images/tema06/nombre.png)` (crea el subdir tema06 conceptualmente; solo referencias).
- Cierra con una sección "Resumen del tema" en viñetas y un `<hr>` final.

Idioma: TODO en español. Tono: didáctico, directo, orientado a que el alumno ejecute en su propia máquina.
</convenciones_criticas>

<patron_desglosable>
PATRÓN OBLIGATORIO para comandos que requieren admin (el alumno tiene cuenta estándar). El flujo PRINCIPAL de cada ejercicio son comandos que corren como usuario estándar; lo admin va colapsado:

```html
<details markdown="1">
<summary>🔐 <strong>Requiere admin</strong> — vista completa de privilegios elevados</summary>

Estos comandos solo funcionan desde una consola "Ejecutar como administrador". Si no tienes
admin en tu host, LEE la salida esperada pero no podrás reproducirla.

```powershell
whoami /priv
```

</details>
```

REQUISITO DE CORRECCIÓN: el atributo `markdown="1"` es OBLIGATORIO en cada `<details>`. Sin él,
kramdown (GitHub Pages) NO renderiza markdown ni fences de código dentro del bloque. Deja SIEMPRE
una línea en blanco después de `<summary>` y antes de `</details>`.
</patron_desglosable>

<placeholders>
El usuario pegará salidas y screenshots reales desde su VM Win10 después. NO FABRIQUES salida de
comandos como si fuera real. Donde iría salida/captura real, inserta placeholders grepables:

- Salida de comando: `> **[PENDIENTE: pegar salida real de \`whoami /all\` desde la VM Win10]**`
- Screenshot: `![Autoruns mostrando entradas de arranque](/assets/images/tema06/autoruns01.png)`
  (la imagen no existe aún — es una referencia placeholder, está bien).

Sí puedes describir en prosa QUÉ debe observar el alumno en la salida (p.ej. "fíjate en el
Integrity Level = Medium"), pero sin inventar el volcado literal.
</placeholders>

<tasks>

<task type="auto">
  <name>Task 1: Front matter + intro + requisitos + Bloque 1 (Identidad) + Bloque 2 (Superficie/defensa)</name>
  <files>_posts/2026-07-25-tema06.md</files>
  <action>
Crea el archivo. Sigue <convenciones_criticas>, <patron_desglosable> y <placeholders> al pie de la letra.

1) FRONT MATTER (misma forma que tema05):
   - title: `Tema 06 - Anatomía de la seguridad de Windows — auditoría de postura del endpoint`
   - date: `2026-07-25 08:00:00 -0500`
   - categories: `[Laboratorios, Seguridad Windows]`  (reutiliza "Laboratorios" como tema05; segunda categoría descriptiva)
   - tags: `[windows, endpoint-security, defensa, blue-team, sysinternals, whoami, icacls, autoruns, windows-defender, windows-firewall, uac, integrity-levels, event-viewer, mitre-attack, hardening, cybersecurity]`

2) INTRO (sección 0, con `<font>` y `<hr>` como tema05):
   - Objetivo de aprendizaje: el alumno aprende a AUDITAR la postura de seguridad de su propio endpoint Windows sin atacar nada.
   - Por qué importa la defensa de endpoint (el endpoint es la primera y última línea).
   - Conexión con tema05/TIM: en tema05 cargaron MITRE ATT&CK en su TIM (OpenCTI); aquí cada capa de defensa se mapea a la técnica ATT&CK que detecta/mitiga y podrán cruzar cada técnica en su TIM corriendo.
   - Dispositivo narrativo (explícalo en un blockquote `> **Idea clave:**`): NO atacamos — seguimos la ruta que UN atacante tomaría y en cada paso observamos qué capa de Windows lo frena o lo registra. Cero ofensiva, máximo aprendizaje ATT&CK.
   - Blockquote `> **Valor de la sesión:**` al estilo tema05: al terminar cada alumno entrega un informe de postura de SU propio host con mapeo defensa→ATT&CK.
   - Tabla "Ruta de la clase (3 horas)" con las 3 filas de bloques + tiempos (60m / 75m / 45m) igual que la tabla de ruta de tema05.

3) SECCIÓN "Requisitos / preparación" (`## <font ...>1. Requisitos y preparación</font>`):
   - Host Windows 10/11 real. Cuenta ESTÁNDAR es suficiente para el flujo principal; lo admin es opcional (desglosables).
   - Sysinternals portables en USB, offline: descargar SysinternalsSuite UNA vez con internet, llevar en USB, ejecutar los .exe directamente (sin instalar). Herramientas usadas: Autoruns, Process Explorer, TCPView, AccessChk.
   - Blockquote `> ⚠️` : algunas vistas completas requieren "Ejecutar como administrador" (se marca por ejercicio); el lab NO necesita internet.
   - Nota de seguridad/ética (blockquote): solo tu propio host, solo inspección, cualquier cambio de config se revierte al final.

4) BLOQUE 1 — `## <font ...>2. Identidad y privilegios (60 min)</font>` — capa de autenticación/autorización.
   Estructura de cada sub-tema: concepto → comando(s) usuario estándar → desglosable admin → "qué frena/registra" → mapeo ATT&CK.
   - Concepto: identidad, SID, grupos, privilegios, integrity levels (Medium para usuario estándar), UAC.
   - Comandos estándar (fences powershell/cmd + placeholder de salida):
     `whoami /all` (observar SID, grupos, privilegios, Integrity Level = Medium),
     `whoami /groups`, `net user`, `net user %USERNAME%`, explorar cuentas locales.
     `icacls C:\Users\%USERNAME%\Documents` vs `icacls C:\Windows\System32` (ACLs NTFS: escribible vs protegido).
     `accesschk.exe -accepteula <ruta>` (Sysinternals, lectura como estándar).
   - Desglosable admin (`<details markdown="1">`): `whoami /priv` elevado (SeDebugPrivilege/SeBackupPrivilege visibles), `net user <admin>`, ver perfiles de otros usuarios.
   - "Qué frena/registra": UAC + integrity levels impiden que un proceso Medium toque recursos High sin consentimiento; ACLs NTFS niegan escritura en rutas protegidas.
   - Mapeo ATT&CK (tabla o lista): T1078 (Valid Accounts), T1134 (Access Token Manipulation), T1548 (Abuse Elevation Control Mechanism / UAC).
   - Entregable parcial (blockquote `> **Entregable parcial 1:**`): mapa de identidad (usuario, SID, grupos, integrity level) + tabla de permisos sobre carpetas clave.

5) BLOQUE 2 — `## <font ...>3. Superficie y defensa activa (75 min)</font>` — capa ejecución/autoarranque/AV/firewall.
   Misma estructura por sub-tema.
   - Autoruns (Sysinternals): dónde persiste el software al arrancar (claves Run, servicios, tareas programadas). Usuario estándar ve HKCU + gran parte de HKLM en solo-lectura. Screenshot placeholder de Autoruns. Concepto firmado vs no firmado.
   - Servicios: `Get-Service`, mención `services.msc`. Firmado vs no firmado.
   - Windows Defender (lectura como estándar): `Get-MpComputerStatus` (protección en tiempo real, edad de firmas), `Get-MpPreference` (exclusiones — por qué una exclusión es el sueño de un atacante). Placeholders de salida.
   - Quick scan en desglosable admin (`<details markdown="1">`): `Start-MpScan -ScanType QuickScan` (puede requerir admin). Concepto SmartScreen.
   - Windows Firewall (lectura estándar): `Get-NetFirewallProfile`, `Get-NetFirewallRule -Direction Inbound -Enabled True | Select DisplayName,Action` (o similar legible). Perfiles Domain/Private/Public.
   - Habilitar logging del firewall en desglosable admin, MARCADO reversible: `Set-NetFirewallProfile -LogAllowed True -LogBlocked True` con nota de cómo revertir (volver a los valores por defecto). Recuérdale revertir en la sección de cierre.
   - "Qué frena/registra": Defender bloquea/aísla ejecución maliciosa; firewall filtra inbound; autoruns revela persistencia.
   - Mapeo ATT&CK (tabla): T1547 (Boot or Logon Autostart Execution), T1543 (Create or Modify System Process — servicios), T1562 (Impair Defenses — exclusiones Defender), T1021 (Remote Services vs firewall).
   - Entregable parcial (blockquote `> **Entregable parcial 2:**`): inventario de autoarranques + estado Defender/Firewall.

Al final de Task 1 el archivo debe llegar hasta el fin del Bloque 2. Task 2 continúa el MISMO archivo.
  </action>
  <verify>
    <automated>test -f _posts/2026-07-25-tema06.md &amp;&amp; grep -q 'font color="#87CEEB">2. Identidad' _posts/2026-07-25-tema06.md &amp;&amp; grep -q 'font color="#87CEEB">3. Superficie' _posts/2026-07-25-tema06.md &amp;&amp; grep -c 'details markdown="1"' _posts/2026-07-25-tema06.md</automated>
  </verify>
  <done>Archivo creado con front matter válido (title/date/categories/tags), intro con ruta de 3h, requisitos/Sysinternals, Bloque 1 y Bloque 2 completos, cada uno con comandos estándar en flujo principal, extras admin en &lt;details markdown="1"&gt;, "qué frena/registra", mapeo ATT&CK y entregable parcial. Placeholders grepables para salidas/screenshots. Estilo idéntico a tema05.</done>
</task>

<task type="auto">
  <name>Task 2: Bloque 3 (Telemetría) + Entregable informe + tabla defensa↔ATT&CK + Resumen + cierre</name>
  <files>_posts/2026-07-25-tema06.md</files>
  <action>
Continúa el MISMO archivo (append). Mantén las convenciones.

6) BLOQUE 3 — `## <font ...>4. Telemetría y auditoría (45 min)</font>` — capa logging/detección.
   Misma estructura por sub-tema (concepto → estándar → desglosable admin → qué revela → ATT&CK).
   - Event Viewer (Visor de eventos), log de Seguridad: eventos de logon 4624 (éxito) / 4625 (fallo). Nota: leer el Security log suele requerir admin → si aplica, ponlo en desglosable admin; el concepto se explica en el flujo principal. Screenshot placeholder del Event Viewer.
   - `auditpol /get /category:*` (lectura de la política de auditoría) — desglosable admin si lo exige.
   - Process Explorer (Sysinternals, estándar): verificación de firmas, integrity level por proceso, DLLs cargadas; el alumno inspecciona sus propios procesos. Screenshot placeholder.
   - TCPView (Sysinternals, estándar): conexiones activas → concepto de beaconing/C2 (sin ejecutar nada malicioso, solo observar). Screenshot placeholder.
   - Mapea cada fuente de telemetría → técnica ATT&CK que revelaría.
   - Mapeo ATT&CK (tabla): T1070 (Indicator Removal — borrado de logs), T1057 (Process Discovery), T1049 (System Network Connections Discovery).

7) `## <font ...>5. Entregable — Informe de postura de seguridad</font>`:
   - Explica que consolida los 3 bloques.
   - Incluye una PLANTILLA de tabla que el alumno rellena, con columnas tipo: `Capa | Hallazgo observado | Comando/herramienta | Técnica ATT&CK relacionada | Riesgo/observación`.
   - Incluye la TABLA RESUMEN "defensa ↔ ATT&CK" consolidando todas las técnicas de los 3 bloques: columnas `Mecanismo de defensa Windows | Qué frena/registra | Técnica(s) ATT&CK`. Filas: UAC/Integrity Levels→T1548/T1134; Valid Accounts/ACLs→T1078; Autoruns/persistencia→T1547; Servicios→T1543; Defender/exclusiones→T1562; Firewall→T1021; Event logs 4624/4625→T1070; Process Explorer→T1057; TCPView→T1049.
   - Blockquote `> **Cruza con tu TIM:**` : el alumno busca cada técnica ATT&CK del informe en su OpenCTI de tema05 para ver qué actores/malware la usan.
   - Rúbrica sugerida (tabla, estilo tema05): criterios como completitud del mapeo defensa→ATT&CK, corrección del uso de comandos como estándar vs admin, calidad del informe, cruce con el TIM.

8) `## <font ...>6. Cierre — revertir cambios y ética</font>` (o incorpóralo antes del Resumen):
   - Lista de reversión: deshabilitar logging de firewall si se activó (valores por defecto), no dejar nada modificado.
   - Nota ética/seguridad: solo tu propio host, solo inspección, nunca contra sistemas de terceros.

9) `### Resumen del tema` (viñetas, estilo tema05) sintetizando las 3 capas (identidad, superficie/defensa, telemetría) y la idea de mapear cada defensa a ATT&CK. Cierra con el `<hr>` verde final.

Verifica al terminar que TODO `<details>` tiene `markdown="1"` y línea en blanco tras `<summary>`; que no hay salida de comando fabricada (solo placeholders `[PENDIENTE: ...]`); que las 9 técnicas ATT&CK aparecen.
  </action>
  <verify>
    <automated>grep -q 'font color="#87CEEB">4. Telemetría' _posts/2026-07-25-tema06.md &amp;&amp; grep -q 'Informe de postura' _posts/2026-07-25-tema06.md &amp;&amp; for t in T1078 T1134 T1548 T1547 T1543 T1562 T1021 T1070 T1057 T1049; do grep -q "$t" _posts/2026-07-25-tema06.md || { echo "FALTA $t"; exit 1; }; done &amp;&amp; grep -q 'PENDIENTE' _posts/2026-07-25-tema06.md &amp;&amp; ! grep -Pzo '<details(?![^>]*markdown="1")' _posts/2026-07-25-tema06.md</automated>
  </verify>
  <done>Bloque 3, sección Entregable (con plantilla + tabla resumen defensa↔ATT&CK + rúbrica + cruce con TIM), sección de reversión/ética y Resumen del tema añadidos. Las 9 técnicas ATT&CK presentes. Todo &lt;details&gt; con markdown="1". Ninguna salida fabricada; solo placeholders grepables. Post cierra con &lt;hr&gt; verde.</done>
</task>

</tasks>

<verification>
- Front matter válido con las 4 claves (title, date, categories, tags); date 2026-07-25 → slug/filename tema06.
- Post 100% en español, hermano visual de tema05 (headers `<font>`, `<hr>` verdes, callouts blockquote).
- 3 bloques con presupuesto de tiempo (60/75/45 = 3h), cada ejercicio con flujo estándar + admin en `<details markdown="1">`.
- Las 9 técnicas ATT&CK (T1078, T1134, T1548, T1547, T1543, T1562, T1021, T1070, T1057, T1049) presentes y mapeadas.
- Cero ofensiva/destrucción; cambios de config marcados reversibles y revertidos en el cierre.
- Solo placeholders `[PENDIENTE: ...]` y referencias `![](/assets/images/tema06/...)`; ninguna salida inventada.
- Build sanity (opcional si hay bundler): `bundle exec jekyll build` no rompe por el nuevo post.
</verification>

<success_criteria>
`_posts/2026-07-25-tema06.md` existe, en español, estilo tema05, con intro+requisitos+3 bloques+entregable informe+tabla defensa↔ATT&CK+resumen; desglosables admin correctos (`markdown="1"`); flujo principal ejecutable por cuenta estándar; placeholders grepables sin salida fabricada.
</success_criteria>

<output>
Create `.planning/quick/260726-amm-crear-post-tema06-anatomia-de-la-segurid/260726-amm-SUMMARY.md` when done
</output>

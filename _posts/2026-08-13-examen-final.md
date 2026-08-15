---
title: "Examen Final — Capstone Threat-Informed"
date: 2026-08-13 09:00:00 -0500
categories: [Examen, Threat Intelligence]
tags: [examen-final, opencti, mitre-attack, metasploit, metasploitable3]
published: true
---

> **Modalidad:** individual · take-home · **ventana de 1 día (24 h)**
> **Entrega:** URL de un post publicado en tu blog, tag `examen-final`
> **Peso:** 100 % de la nota del examen final
{: .prompt-info }

## El encargo

Eres analista de un equipo de ciberseguridad. Se te ha asignado un **grupo APT**. Tu misión
tiene una regla de oro: **la inteligencia dirige la operación**. No atacas a ciegas —
primero entiendes cómo opera tu adversario, y solo entonces reproduces su comportamiento en
el laboratorio.

Documentarás todo como un **writeup en tu blog**. Ese post *es* tu examen.

Tu **APT asignado** está en la tabla de la sección siguiente. Tus **credenciales temporales**
del OpenCTI del servidor del curso las recibes al abrir la ventana; **solo funcionan durante
las 24 h** del examen.

## Asignación de APT por alumno

Busca tu nombre. Ese es **tu** grupo APT para todo el examen — es único: dos writeups del
mismo APT se revisan por copia. La tabla te da el **punto de partida** (técnica, servicio
objetivo y módulo sugerido); tu trabajo es *justificarlo con la intel del Acto 1*,
*ejecutarlo* y *defenderlo* — no basta con copiar la fila.

| Alumno | APT asignado | Técnica ATT&CK | Servicio en Metasploitable3 | Módulo Metasploit sugerido |
|--------|--------------|----------------|-----------------------------|----------------------------|
| MELVIN ACUÑA CHAVEZ | APT28 / Fancy Bear | T1110.003 Password Spraying | SSH (22) | `scanner/ssh/ssh_login` |
| Jesus Apaza | APT29 / Cozy Bear | T1078 Valid Accounts + T1021.006 | WinRM (5985) | `winrm_login` → `winrm_cmd` |
| KATHERINE FABIOLA CABIA RAMIREZ | Wizard Spider | T1210 Exploitation of Remote Services | SMB MS17-010 (445) | `ms17_010_eternalblue` |
| DANIEL JOSUE CONDOR GARCIA | APT41 | T1190 + T1505.003 Web Shell | GlassFish (4848/8080) | `glassfish_deployer` (WAR) |
| RAFAEL ESPINO CAMPOS | Sandworm Team | T1190 Exploit Public-Facing App | Jenkins (8484) | `jenkins_script_console` |
| NEVARDO ALEJANDRO MARCAS CASTILLO | APT27 / Emissary Panda | T1190 + web shell | ManageEngine Desktop Central (8383) | `manageengine_connectionid_write` |
| ROBERTH GERMAN MORALES TIRADO | menuPass / APT10 | T1021.002 SMB + T1078 | SMB (445) | `psexec` con credenciales |
| MARIA CRISTINA OCHANTE LEON | Turla / Snake | T1505.003 Web Shell | WAMP / WordPress (80) | `wp_admin_shell_upload` |
| ROLANDO PAZ PURISACA | APT33 / Elfin | T1110.003 Password Spraying | MySQL (3306) | `scanner/mysql/mysql_login` |
| FRANCISCO QUISPE PINTO QUISPE | Lazarus Group | T1190 | ElasticSearch (9200) | `elasticsearch_script_mvel_rce` |
| SERGIO ALEJANDRO ROMERO PUERTAS | APT39 | T1110 Brute Force + T1078 | WordPress admin (80) | `wordpress_login_enum` + brute |
| SILVIO EDGAR SILVERIO FLORES | OceanLotus / APT32 | T1078 Valid Accounts | SSH (22) con credenciales | `ssh_login` con credenciales |
| JHON KARLESSY VIOLETA MOREYRA | Dragonfly / Energetic Bear | T1190 | Apache/WAMP PHP RCE (80) | web RCE / `php_cgi` |

> El módulo es **un** punto de partida sugerido, no la única solución. Debes **conectar** esa
> técnica con la intel real de tu APT (Acto 1), **ejecutarla** con evidencia identificable
> (Acto 3) y **defenderla** (Acto 4). Copiar la fila sin ese trabajo no suma.
{: .prompt-warning }

## Los cuatro actos

Tu post tendrá exactamente estas cuatro secciones.

### Acto 1 — La inteligencia dirige (OpenCTI del curso)

Entra al OpenCTI del servidor del curso con tus credenciales temporales y construye el
**perfil de tu APT**:

- **Intrusion Set:** identidad, alias, motivación.
- **5–8 TTPs clave** (Attack Patterns) que usa el grupo.
- **IoCs / Indicators** asociados que encuentres.
- **Malware / herramientas** del grupo.

**Debes justificar (teoría embebida):**

- ¿Qué **nivel de inteligencia** (estratégico / operacional / táctico) es cada dato?
- ¿Qué **TLP** trae cada IoC? Si sale `NONE`, ¿quién debería fijar la política y por qué?
- ¿Qué **vértice del modelo Diamante** llena cada dato del perfil?

> Tu OpenCTI local (el de tu Kali) **no sirve** como fuente: está vacío de este contexto.
> La inteligencia sale del OpenCTI del curso.
{: .prompt-warning }

### Acto 2 — Plan de ataque mapeado a ATT&CK

Con el perfil, arma tu plan. La pregunta que respondes:

> **¿Cuáles TTPs de mi APT tienen un análogo real en la superficie de Metasploitable3?**

Entrega una **tabla**:

| Táctica | Técnica ATT&CK | Servicio/objetivo en Metasploitable3 | Herramienta |
|---------|----------------|--------------------------------------|-------------|
|         |                |                                      |             |

Y **descarta explícitamente** las TTPs que NO puedes reproducir, explicando por qué
(ej.: «spearphishing no tiene un humano objetivo en el lab»).

### Acto 3 — Ejecución

Ejecuta **una** kill chain contra **Metasploitable3**: acceso inicial → sesión/RCE →
post-explotación / escalada de privilegios.

- **Etiqueta cada paso** con su técnica ATT&CK.
- **Screenshots reales**, con tu **hostname o usuario visible** en la terminal (evidencia de
  que es tu instancia).
- Muestra el objetivo alcanzado: `getsystem`, un hash, un archivo, una flag — lo que
  demuestre control.

> **Análisis del payload — OBLIGATORIO para optar a la nota máxima.** Extrae el payload de
> tu módulo e interprétalo con **dos herramientas de análisis distintas** — una **estática**
> y una **dinámica**:
>
> - **Extrae los bytes:** `msfvenom -p <tu-payload> LHOST=... -f raw -o payload.bin` (o
>   captúralos del módulo Ruby / del tráfico).
> - **Estática** (`ndisasm -b64`, `objdump -D -b binary`, Ghidra): muestra el *decoder stub*
>   y dónde termina y empieza la parte cifrada.
> - **Dinámica** (`scdbg -f payload.bin`, `speakeasy`): lista las **APIs de Windows** que el
>   shellcode invocaría (`LoadLibrary`, `WSASocket`, `connect`…).
>
> Responde en el post: ¿es **shellcode crudo** o un **command payload**? Si está codificado
> (p. ej. `x86/shikata_ga_nai`), ¿por qué la estática por sí sola **no** basta para ver el
> *egg* real? ¿Qué APIs delatan la conexión reversa? **Cruza esas APIs con tu detección del
> Acto 4.**
>
> Todo el análisis es **offline, dentro del lab aislado** — nada de sandboxes online (no
> subas tu payload a terceros).
{: .prompt-warning }

### Acto 4 — Defensa

Cambia de bando. Para la cadena que ejecutaste:

| Técnica ejecutada | ¿Cómo la detectarías? (log/evento, fuente de datos ATT&CK) | Mitigación (ATT&CK Mitigation) |
|-------------------|------------------------------------------------------------|--------------------------------|
|                   |                                                            |                                |

Añade **2–3 capturas de evidencia defensiva** (Sysinternals, Visor de eventos, configuración
de postura — lo de tema06).

## Cómo entregar

1. Publica el post en tu blog (`published: true`), accesible en tu GitHub Pages.
2. Front matter con el tag `examen-final`.
3. Envía **la URL del post** por el canal del curso **antes del cierre de la ventana**.

```yaml
---
title: "Examen Final — [tu APT]"
date: 2026-08-15
categories: [Examen, Threat Intelligence]
tags: [examen-final]
---
```

## Cómo se califica (100 pts)

| # | Dimensión | Puntos |
|---|-----------|--------|
| 1 | Extracción y modelado de inteligencia (OpenCTI, TTP/IoC, TLP, nivel) | 20 |
| 2 | Fidelidad del mapeo ATT&CK (con descartes justificados) | 15 |
| 3 | Éxito de ejecución + **interpretación del payload** + evidencia (screenshots reales identificables) | 25 |
| 4 | Análisis defensivo (detección + mitigación mapeadas) | 15 |
| 5 | Calidad de documentación (claridad, estructura, reproducibilidad) | 15 |
| 6 | Defensa teórica (niveles, Diamante, TLP, kill chain correctos) | 10 |

Cada dimensión se califica **por separado**: si no logras explotar (Acto 3), aún sumas por
inteligencia, plan y defensa. Documenta lo que intentaste.

> **La interpretación del payload (Acto 3) es obligatoria para la nota máxima.** Un writeup
> que explota pero **no** interpreta su payload con una herramienta estática **y** una
> dinámica limita la dimensión 3 a **15/25**. Con el análisis completo y bien cruzado con la
> detección del Acto 4, opta a los 25.
{: .prompt-danger }

## Reglas

- **Individual.** Tu APT y tus credenciales son únicos. Dos writeups del mismo APT se revisan.
- Solo contra **tu Metasploitable3**, en red aislada. Nunca contra sistemas reales ni contra
  el servidor del curso.
- Las credenciales del OpenCTI del curso son de **solo lectura** y temporales: úsalas para
  extraer inteligencia, no para escribir ni atacar esa plataforma.
- Si el servidor del curso falla durante tu ventana, avisa de inmediato por el canal del curso.

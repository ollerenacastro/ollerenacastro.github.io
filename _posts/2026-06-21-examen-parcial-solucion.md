---
title: "Examen Parcial — Solución"
date: 2026-06-21 09:00:00 -0500
categories: [Examen, Solucionario]
tags: [examen-parcial, jekyll, virtualbox, nmap, metasploit, metasploitable3, mitre-attack]
published: true
---

> Respuestas de referencia del **Examen Parcial 01** (PET 204 Ciberseguridad).
> Basado en los Temas 01, 02 y 03. Respuestas cortas — se valora precisión, no extensión.
{: .prompt-info }

## Unidad 1 — Jekyll, Git y GitHub Pages

**1. Rol de GitHub Actions (de `git push` a post visible).**
El `push` dispara el workflow de Actions → **build** (Jekyll convierte Markdown en HTML) → **deploy** a GitHub Pages → el post queda visible. El visitante solo recibe HTML estático (no corre Ruby ni Jekyll).

**2. Campos del Front Matter (y por qué obligatorios).**
- `title`: título del post.
- `date`: fecha/hora de publicación (ordena y fija la URL).
- `categories`: agrupa el post.
- `tags`: etiquetas para búsqueda/relación.

Chirpy los exige para generar y ordenar el post correctamente.

**3. Orden correcto de los comandos.**
`git add .` (prepara los cambios) → `git commit -m "..."` (guarda una versión con mensaje) → `git push` (sube al repo y dispara la publicación).

## Unidad 2 — Setup de Laboratorio y VirtualBox

**4. NAT vs NAT Network.**
NAT da internet a la VM pero las VMs quedan aisladas (no se ven). NAT Network: las VMs comparten una red interna (`10.0.2.0/24`) y **sí se comunican**. NAT simple no basta porque Kali no puede alcanzar a Metasploitable.

**5. Peligro de Bridged Adapter.**
La VM recibe IP real de la red universitaria y aparece como un equipo más. Riesgo: Metasploitable (vulnerable a propósito) queda expuesta a toda la red — cualquiera puede atacarla o usarla como pivote.

**6. Snapshot y por qué es obligatorio.**
Captura del estado de la VM en un punto. Obligatorio antes de una Kill Chain para **revertir** la VM al estado limpio tras la explotación, que la altera o daña.

**7. Comandos Nmap.**
- `(a) nmap -sn 10.0.2.0/24`: *host discovery* — lista los hosts vivos e IPs de la red, sin escanear puertos.
- `(b) nmap -sV -p22 10.0.2.15`: detecta el **servicio y su versión** en el puerto 22 (revela OpenSSH 7.1).

**8. Credenciales de Metasploitable3 y `slmgr.vbs /rearm`.**
Usuario/contraseña: `vagrant` / `vagrant`. `slmgr.vbs /rearm` rearma el periodo de activación de la licencia de Windows. Necesario al primer ingreso porque la licencia de evaluación ya expiró y, sin rearmar, la VM se apaga.

## Unidad 3 — Kill Chains, Metasploit y MITRE ATT&CK

**9. Tipos de reconocimiento.**
- **Pasivo**: sin tráfico al objetivo, solo info pública. Ej: Shodan, WHOIS.
- **Semi-pasivo**: terceros consultan al objetivo de forma indirecta. Ej: consultas DNS, crt.sh.
- **Activo**: tráfico directo al objetivo. Ej: Nmap, Nikto.

**10. `nmap -A -p- 10.0.2.15`.**
`-A`: escaneo agresivo (SO, versión de servicios, scripts por defecto, traceroute). `-p-`: escanea los 65 535 puertos. Output: puertos abiertos + servicio/versión + SO.

**11. OpenSSH en Metasploitable3.**
Versión **7.1**, puerto **22**. Vulnerable a fuerza bruta porque por defecto no limita intentos ni bloquea la cuenta. Medidas ausentes: **límite de intentos** (rate-limit/fail2ban) y **bloqueo de cuenta** tras N fallos.

**12. Secuencia `ssh_enumusers`.**
`use` carga el scanner de enumeración de usuarios SSH; `set RHOSTS` fija el objetivo; `set USER_FILE` da la lista de usuarios a probar; `run` ejecuta. Output: qué usuarios de la lista son **válidos** en el SSH.

**13. VSS y `vssown.vbs`.**
VSS crea copias instantáneas (*shadow copies*) de un volumen. Se usa `vssown.vbs` porque SAM y SYSTEM están **bloqueados** por el kernel mientras Windows corre — no se copian directo, ni con sesión SSH. La shadow copy da una versión desbloqueada.

**14. `copy ...ShadowCopy1...\SAM`.**
Copia SAM desde la shadow copy. Necesario porque en `C:\Windows\System32\config\` los archivos están en uso/bloqueados por el SO; la shadow copy es un snapshot no bloqueado del que sí se puede leer.

**15. Flags de John the Ripper.**
`--format=NT`: hashes en formato NT/NTLM. `--wordlist=kaonashi14M.txt`: diccionario. `hashes.txt`: archivo de hashes a crackear. `--fork=4`: 4 procesos en paralelo.

**16. Vulnerabilidad del Kill Chain 2 (Jenkins).**
La Jenkins Script Console ejecuta código Groovy/comandos del SO arbitrarios (**RCE**) sin control de acceso. Crítico porque da ejecución remota de código con los privilegios del servicio → control del host.

**17. `println "cmd /c whoami".execute().text`.**
Ejecuta `whoami` vía `cmd` en el host e imprime el **usuario actual** (p. ej. `vagrant`). Demuestra RCE.

**18. Privilegio de EternalBlue (MS17-010).**
Privilegios **SYSTEM** (el máximo). Implica control total del sistema: leer/modificar cualquier archivo, credenciales y servicios.

**19. Secuencia `ms17_010_eternalblue`.**
`use` carga el exploit; `set RHOSTS`=víctima; `set LHOST`=atacante (callback); `set payload`=meterpreter x64 reverse_tcp; `run` lanza. Resultado: sesión **meterpreter con privilegios SYSTEM**.

**20. Tácticas MITRE ATT&CK (con ejemplo del lab).**
- **Initial Access**: primer punto de entrada. Ej: fuerza bruta SSH / EternalBlue.
- **Credential Access**: robar credenciales. Ej: volcar hashes SAM y crackear con John.
- **Privilege Escalation**: subir de privilegios. Ej: obtener SYSTEM vía EternalBlue.

---

*Ciberseguridad UNTELS 2026-I — Solucionario Examen Parcial 01*

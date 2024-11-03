---
title: Examen Parcial
date: 2024-10-31 06:20:00 -0500
categories: [Cybersecurity,Ethical Hacking,Pentesting,Exploitation Techniques]
tags: [Examen Parcial, evaluation]     # TAG names should always be lowercase
---

<!-- <hr style="border: none; height: 10px; background-color: #003b00;" />

# <font color="#87CEEB">Examen Parcial.</font>

<hr style="border: none; height: 10px; background-color: #003b00;" /> -->

## 0. Preliminares

### 0.1. En la máquina Metasploitable 3

- Asegúrese de tener la máquina virtual en modo Nat Network.

- Asegúrese de tener el Firewall de Windows **desactivado**.

  - Click en `Inicio`, escribir Firewall, seleccionar `Windows Firewall`.
  - En el lado izquierdo, click en la opción `Turn Windows Firewall on or off`. 
  - Para ambas opciones, click en `Turn off windows Firewall (not recommended)`.
  - Finalmente, click en `Ok`.

- Realizar los siguientes pasos ya que, por defecto, la máquina virtual Metasploitable 3 no tiene el servicio SMBv1 habilitado.

    - Abrir el Server Manager, ícono que está a la derecha del botón `Start`.

    ![alt text](/assets/images/server-manager.png)

    - Click en `Add Roles`.

    ![alt text](/assets/images/server-manager-add-roles.png)

    - Click en `Next`.

    - En `Server Roles`, habilitar el rol `File Services`.

    ![alt text](/assets/images/server-manager-enable-file-services.png)

    - En File Click en `Next`.

    - En `File Services`, click en `Next`.

    - En `File Services > Role Services`, click en `Next`.

    - En `Confirmation`, click en `Install`.
    - En `Results`, click en `Close`.
    - Reiniciar el servidor.

### 0.2. En la máquina Kali Linux

- Asegúrese de tener la máquina virtual en modo Nat Network.

### 0.3. Setup del laboratorio

- El setup es como se muestra en la figura:

![alt text](/assets/images/setup-lab.png)

## 1. Pregunta 1 (8 puntos).

- Link de cuestionario: [Cuestionario](https://forms.gle/6bN8Yxf9RF3DEzbJ7)


## 2. Pregunta 2 (12 puntos).

  _**Eres un auditor de seguridad que tiene la tarea de realizar un pentesting sobre un servidor de una empresa que ejecuta Windows Server 2008 R2. Este servidor tiene habilitado el servicio SMBv1, y tu objetivo es identificar vulnerabilidades, explotar el servicio SMB para obtener acceso y luego exfiltrar los archivos SAM y SYSTEM. Siguiendo los pasos a continuación, documenta tu procedimiento y las herramientas empleadas.**_

### Instrucciones:

### 2.1. Escaneo de Red y Enumeración de Servicios (2 puntos)

- Utiliza **Nmap** desde tu máquina atacante para descubrir servicios activos en la máquina objetivo. Debes identificar los puertos relacionados con **SMB** y verificar si **SMBv1** está habilitado.

- **Tip**: Investiga cuáles son los 02 puertos asociados con el servicio **SMB**. Luego, investiga como puedes usar `nmap` con el flag `--script` para una enumeración detallada. Al analizar el output del comando, buscar específicamente el término **SMBv1** o el protocolo **NT LM 0.12** en la lista de protocolos. Ello indicará la presencia del servicio **SMBv1**.

- Escribe el comando que usaste en Nmap y explica brevemente cómo determina si SMBv1 está activo.

### 2.2. Exploración de la Vulnerabilidad (2 puntos)

- Utiliza los resultados del escaneo para identificar si el servicio SMBv1 presenta alguna vulnerabilidad conocida. Indica si el servidor es vulnerable al exploit EternalBlue (MS17-010).

- **Tip**: Para explotar esta vulnerabilidad, usa el framework Metasploit y selecciona el módulo específico `exploit/windows/smb/ms17_010_eternalblue`, con el objetivo de establecer una conexión reverse shell hacia la máquina atacante.

- Explica brevemente el funcionamiento del script `exploit/windows/smb/ms17_010_eternalblue` y por qué se selecciona este módulo en particular para obtener acceso remoto.

### 2.3. Configuración del Exploit en el Framework Metasploit (2 puntos)

- Configura el exploit mencionado anteriormente, especificando los parámetros necesarios, como las direcciones IP del atacante y la víctima.

- **Tip**: Configura los parámetros **RHOSTS** para la IP de la máquina objetivo y **LHOST** para tu máquina atacante. Estos son esenciales para establecer la conexión.

- **Tip adicional**: También debes configurar el **payload** para el reverse shell. Busca en el metasploit un payload (que ya anteriormente hemos usado en clase) para obtener un reverse shell connection.

- Explica brevemente cada parámetro crítico que configuraste, como **LHOST** y **RHOSTS**, y justifica por qué esos valores son importantes para el ataque.

### 2.4. Ejecución del Exploit (2 puntos)

- Ejecuta el exploit para obtener una **conexión Meterpreter** con el servidor. Si la explotación es exitosa, documenta qué mensaje o salida te confirma que la conexión ha sido establecida.

- **Tip**: Al ejecutar el exploit, Metasploit intentará establecer una sesión Meterpreter en la máquina víctima si el ataque es exitoso.

- **Tip adicional**: Documenta cualquier mensaje en la salida de Metasploit que indique éxito, como `Meterpreter session X` opened. Esto confirma la conexión.

### 2.5. Exfiltración de Archivos SAM y SYSTEM (2 puntos)

- Utilizando la sesión de Meterpreter establecida, explica los pasos para infiltrar un script o herramienta que te permita crear una copia de volumen de sombra (Volume Shadow Copy) de la unidad principal (C:) del servidor.

- Link de descarga del archivo `vssown.vbs`: [Descargar archivo](/assets/files/vssown.vbs)

- Describe brevemente cómo utilizarías esta copia de sombra para acceder a los archivos **SAM** y **SYSTEM**. Enumera los comandos necesarios en Meterpreter o la shell de Windows para:

  - Crear la copia de sombra.
  - Copiar los archivos SAM y SYSTEM a un directorio accesible.
  - Descargar los archivos a tu máquina atacante.

- **Tip**: Utiliza el enlace de descarga proporcionado para obtener el script `vssown.vbs` e infíltralo en la máquina víctima usando Meterpreter.

- **Tip adicional**: Después de ejecutar el script `vssown.vbs`, toma nota de la ubicación de la **copia de volumen de sombra** que se crea (ruta similar a `\\?\GLOBALROOT\Device\HarddiskVolumeShadowCopyX`).

### 2.6. Análisis de los Archivos Exfiltrados (2 puntos)

- Una vez que hayas descargado los archivos **SAM** y **SYSTEM**, describe el procedimiento que seguirías en tu máquina atacante para extraer los hashes de contraseñas. Especifica qué herramientas emplearías para realizar esta tarea.

- **Tip**: Una vez descargados los archivos SAM y SYSTEM a tu máquina Kali Linux, usa herramientas como `samdump2` para extraer los hashes.

- **Tip adicional**: Con los hashes extraídos, puedes intentar descifrarlos con herramientas como **John the Ripper** o **Hashcat**.

### 2.7. Requisitos de Documentación

- Documentar en su respectivo blog.

- Para cada paso, proporciona capturas de pantalla y explica en tus propias palabras qué estás haciendo y por qué es relevante en un proceso de auditoría de seguridad.

- Asegúrate de usar herramientas comunes en entornos de pentesting (por ejemplo, **Nmap**, **Metasploit**, **samdump2**, **John the Ripper**).

- **Tip**: Asegúrate de capturar cada paso clave con capturas de pantalla y explica en tu blog lo que hace cada comando o herramienta.

<!-- - **Consejo**: Piensa en cómo documentarías el proceso para que alguien sin conocimientos previos entienda tu flujo de trabajo. Enfócate en los objetivos y en el impacto de cada paso dentro del proceso de auditoría de seguridad -->
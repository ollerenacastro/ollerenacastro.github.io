---
title: "Troubleshooting: OpenSSH Detenido en Metasploitable3"
date: 2026-06-08
categories: [Laboratorio, Troubleshooting]
tags: [metasploitable3, openssh, virtualbox, windows, laboratorio]
---

Este artículo cubre el diagnóstico y solución del síntoma **"OpenSSHd Stopped + falla al iniciar"** en la máquina virtual Metasploitable3 (Windows Server 2008 R2) después de importar el `.ova`. Es una guía de resolución de problemas para alumnos del laboratorio de ciberseguridad.

## Síntoma

Al ejecutar `get-service *ssh*` en PowerShell como Administrador, el servicio aparece como `Stopped`:

```powershell
PS C:\Users\Administrator> get-service *ssh*

Status   Name               DisplayName
------   ----               -----------
Stopped  OpenSSHd           OpenSSH Server
```

Al intentar iniciarlo con `Start-Service OpenSSHd`, el comando falla con error `StartServiceFailed`.

---

> **Causas más probables:** Basado en el comportamiento observado en el laboratorio, sospecho primero de la **Causa 1** (el servicio está configurado en inicio Manual y simplemente no arrancó solo). Si la Causa 1 no resuelve el problema, la siguiente sospecha es la **Causa 5** (recursos insuficientes de la VM). Sin embargo, antes de llegar a la Causa 5 conviene agotar las causas intermedias — el diagnóstico por log (Causa 3) suele revelar el problema exacto sin necesidad de apagar y reconfigurar VMs.

---

## Diagnóstico previo: obtener el error real

Antes de aplicar cualquier solución, ejecuta esto en PowerShell para ver el motivo exacto del fallo:

```powershell
# Ver los últimos eventos del sistema relacionados con el servicio
Get-EventLog -LogName System -Newest 30 | Where-Object {
    $_.Message -like "*ssh*" -or $_.Source -like "*ssh*" -or $_.Source -like "*cygwin*"
} | Select-Object TimeGenerated, Source, EntryType, Message | Format-List
```

Es muy probable que obtenga un output. Tome un screenshot y coloque en su reporte de troubleshooting (un .md con este troubleshooting).

```powershell
# Ver también el Application log
Get-EventLog -LogName Application -Newest 30 | Where-Object {
    $_.Message -like "*ssh*" -or $_.Source -like "*cyg*"
} | Select-Object TimeGenerated, Source, EntryType, Message | Format-List
```

Anota el mensaje de error — te indicará cuál de las causas aplica.

En mi caso, obtengo algo así:

![Output](/assets/images/opensshd-troubleshooting-01.png)

> **Nota:** En mi caso todos son logs de información porque mi servicio **sí funciona**. Compara tu output con este: si aparecen entradas de tipo `Error`, esas líneas señalan la causa del fallo.

---

### Inspeccionar la configuración del servicio

Ejecuta estos dos comandos desde `cmd.exe` (no PowerShell) para ver exactamente qué binario intenta lanzar el servicio y dónde escribe sus errores:

```cmd
"C:\Program Files\OpenSSH\bin\cygrunsrv.exe" --query OpenSSHd
```

```cmd
reg query "HKLM\SYSTEM\CurrentControlSet\Services\OpenSSHd\Parameters"
```

![Configuración del servicio OpenSSHd](/assets/images/opensshd-troubleshooting-03.png)

> **Nota:** En mi caso el servicio aparece como `Running`. En tu caso debería aparecer `Stopped`. Lo importante es comparar los valores `AppPath` y `StdErr` — deben coincidir con los de esta imagen.

Los valores clave que debes anotar:
- **`AppPath`**: la ruta al binario real de sshd (en Cygwin: `/usr/sbin/sshd`)
- **`StdErr`**: la ruta del log de errores — aquí se registra el motivo del fallo

### Revisar el log de errores del servicio

El valor `StdErr` apunta a `/var/log/OpenSSHd.log`. En Windows esa ruta corresponde a:

```powershell
type "C:\Program Files\OpenSSH\var\log\OpenSSHd.log"
```

**Si el archivo existe y tiene contenido**, el mensaje de error ahí es el diagnóstico definitivo. Úsalo para identificar cuál de las causas aplica.

---

## Causa 1: Tipo de inicio en Manual o Deshabilitado

**Probabilidad: Alta.** Algunos OVAs configuran servicios en `Manual` por defecto. El servicio existe y puede funcionar, pero no arranca solo al encender la VM.

### Cómo detectarlo

```powershell
(Get-WmiObject win32_service -filter "name='OpenSSHd'").StartMode
```

Si devuelve `Manual` o `Disabled`, esta es la causa.

### Solución

```powershell
# Cambiar a inicio automático y arrancar
Set-Service OpenSSHd -StartupType Automatic
Start-Service OpenSSHd

# Verificar
Get-Service OpenSSHd
```

> Si el comando anterior falla con error diferente a "Disabled", continúa con las siguientes causas.

---

## Causa 2: Binario del servicio o launcher inexistente

**Probabilidad: Media.** El servicio OpenSSHd usa `cygrunsrv.exe` como launcher y `sshd.exe` como proceso real. Si alguno de los dos no existe en la ruta esperada, el servicio falla al arrancar.

### Cómo detectarlo

```powershell
# Verificar el launcher (cygrunsrv)
(Get-WmiObject win32_service -filter "name='OpenSSHd'").PathName

Test-Path "C:\Program Files\OpenSSH\bin\cygrunsrv.exe"

# Verificar el binario real de sshd
Test-Path "C:\Program Files\OpenSSH\usr\sbin\sshd.exe"
```

Ambos `Test-Path` deben devolver `True`.

![Verificación de binarios OpenSSH](/assets/images/opensshd-troubleshooting-02.png)

> **Nota:** En mi caso ambos devuelven `True` y el servicio funciona. Verifica que en tu máquina ocurra lo mismo. Si alguno devuelve `False`, esa es probablemente la causa del fallo.

### Solución

Si `cygrunsrv.exe` o `sshd.exe` no existen, la instalación de OpenSSH está incompleta o corrompida. Buscar los archivos en el disco:

```powershell
Get-ChildItem -Path C:\ -Filter "sshd.exe" -Recurse -ErrorAction SilentlyContinue
Get-ChildItem -Path C:\ -Filter "cygrunsrv.exe" -Recurse -ErrorAction SilentlyContinue
```

Si no se encuentran en ninguna ruta, reinstalar OpenSSH para Windows desde la VM no es viable en un entorno de laboratorio. En ese caso, restaurar la VM desde un snapshot limpio o reimportar el OVA.

---

## Causa 3: Errores en el log de sshd (claves faltantes o fallo de fork)

**Probabilidad: Media-Alta.** El log de errores del servicio revela dos tipos de problemas que pueden impedir el arranque de sshd.

### Cómo leer el log

```powershell
Get-Content "C:\Program Files\OpenSSH\var\log\OpenSSHd.log"
```

En mi caso (servicio **funcionando**) el log muestra esto:

![Log de OpenSSHd en máquina funcional](/assets/images/opensshd-troubleshooting-04.png)

> **Nota:** Este es el output de mi máquina donde el servicio **sí arranca**. Tu log puede contener los mismos errores o versiones más graves. Compara línea por línea.

---

### Error tipo A: `Could not load host key`

```
Could not load host key: /etc/ssh_host_ed25519_key
```

En mi caso falta la clave `ed25519`, pero sshd arranca igual porque tiene otras claves de respaldo (rsa, dsa, ecdsa). **Si en tu máquina faltan TODAS las claves**, sshd no puede arrancar.

**Diagnóstico:**

```powershell
# Verificar qué claves existen (en Windows corresponden a C:\Program Files\OpenSSH\etc\)
Get-ChildItem "C:\Program Files\OpenSSH\etc\" -Filter "ssh_host_*"
```

Si el directorio está vacío o no existe ninguna clave `ssh_host_*_key`, esa es la causa.

**Solución — regenerar las claves:**

```cmd
"C:\Program Files\OpenSSH\usr\bin\ssh-keygen.exe" -t rsa -f "C:\Program Files\OpenSSH\etc\ssh_host_rsa_key" -N ""
"C:\Program Files\OpenSSH\usr\bin\ssh-keygen.exe" -t dsa -f "C:\Program Files\OpenSSH\etc\ssh_host_dsa_key" -N ""
"C:\Program Files\OpenSSH\usr\bin\ssh-keygen.exe" -t ecdsa -f "C:\Program Files\OpenSSH\etc\ssh_host_ecdsa_key" -N ""
```

Luego intentar arrancar el servicio:

```powershell
Start-Service OpenSSHd
Get-Service OpenSSHd
```

---

### Error tipo B: `fork: child -1 - CreateProcessW failed`

```
0 [main] sshd 2928 fork: child -1 - CreateProcessW failed for 'C:\Program Files\OpenSSH\usr\sbin\sshd.exe'
```

Este error significa que Cygwin no puede hacer `fork()` porque otra DLL del sistema está ocupando la dirección de memoria que necesita. En mi máquina son errores transitorios — sshd arranca igual. En tu máquina pueden ser fatales si el conflicto es más grave (diferente versión de Windows, software adicional instalado, o configuración de ASLR distinta).

**Solución — rebase de las DLLs de Cygwin:**

Cierra **todos** los procesos de Cygwin/OpenSSH y ejecuta desde `cmd.exe` como Administrador:

```cmd
"C:\Program Files\OpenSSH\bin\rebaseall.exe"
```

Si `rebaseall.exe` no existe en esa ruta, búscalo:

```powershell
Get-ChildItem -Path "C:\Program Files\OpenSSH\" -Filter "rebaseall*" -Recurse
```

Tras ejecutar `rebaseall`, reinicia la VM y vuelve a intentar arrancar el servicio.

---

## Causa 4: Puerto 22 ya en uso

**Probabilidad: Baja.** Si otro proceso está usando el puerto 22, sshd no puede enlazarse a él y falla al arrancar.

### Cómo detectarlo

```powershell
# Ver qué proceso usa el puerto 22
netstat -ano | findstr :22
```

Si hay una línea con `LISTENING` en el puerto 22, anota el PID (último número) y verifica qué proceso es:

```powershell
Get-Process -Id <PID>
```

### Solución

Si el proceso que ocupa el puerto 22 no es OpenSSHd o SSHd, detenerlo:

```powershell
Stop-Process -Id <PID> -Force
Start-Service OpenSSHd
```

---

## Causa 5: Recursos insuficientes (RAM y CPU)

**Probabilidad: Media** — especialmente si tienes Kali y Metasploitable3 corriendo al mismo tiempo con recursos mínimos.

### Por qué ocurre

En el laboratorio es común tener ambas VMs activas simultáneamente con configuraciones muy ajustadas (1 GB de RAM y 1 CPU cada una). Esto puede causar que el servicio OpenSSH no arranque por dos razones:

1. **Windows detiene servicios por memoria baja.** El Service Control Manager de Windows puede no iniciar servicios no esenciales cuando la RAM disponible está por debajo de cierto umbral. Con 1 GB de RAM total para Windows Server 2008 R2, el sistema operativo consume la mayor parte solo para arrancar.

2. **El fork() de Cygwin falla por falta de memoria.** El error `fork: child -1 - CreateProcessW failed` del log (Causa 3 - Error tipo B) también puede ser causado directamente por memoria insuficiente para duplicar el proceso sshd, no solo por conflictos de DLLs.

### Cómo verificarlo

Verifica los recursos asignados a cada VM con VirtualBox **cerradas** (no en modo suspendido):

**En VirtualBox → seleccionar la VM → Configuración → Sistema:**

| VM | RAM mínima recomendada | CPU mínima recomendada |
|---|---|---|
| Metasploitable3 (Windows) | 2048 MB | 2 CPUs |
| Kali Linux | 2048 MB | 2 CPUs |

Si alguna tiene 1024 MB o menos, esa es probablemente la causa.

También verifica dentro de la VM cuánta RAM está disponible:

```powershell
(Get-WmiObject Win32_OperatingSystem).FreePhysicalMemory
```

Si el valor es menor a 200,000 KB (≈200 MB libres) con el sistema recién iniciado y sin carga, la RAM es insuficiente.

### Solución

1. Apagar ambas VMs
2. En VirtualBox: clic derecho → Configuración → Sistema → aumentar **Memoria base** a 2048 MB mínimo para Metasploitable3
3. Aumentar también los **Procesadores** a 2 si el equipo host lo permite
4. Reiniciar la VM y verificar el servicio:

```powershell
Start-Service OpenSSHd
Get-Service OpenSSHd
```

> **Nota:** Para aumentar los recursos de las VMs, el equipo host necesita tener RAM suficiente. Si el host tiene 8 GB de RAM, asignar 2 GB a cada VM es viable. Si el host tiene 4 GB o menos, considera ejecutar solo una VM a la vez.

---

## Flujo de diagnóstico recomendado

Seguir este orden para diagnosticar el problema de manera eficiente:

```
1. Revisar Event Log (Get-EventLog) → ¿hay mensaje de error explícito?
      ↓ sí → ir directamente a la causa indicada
      ↓ no → continuar
2. Verificar StartMode → ¿está en Manual/Disabled?
      ↓ sí → Causa 1 (cambiar a Automatic)
      ↓ no → continuar
3. Verificar ruta del binario → ¿existe el .exe registrado en el servicio?
      ↓ no → Causa 2 (re-registrar servicio)
      ↓ sí → continuar
4. Ejecutar sshd -t desde Cygwin → ¿hay errores de permisos?
      ↓ sí → Causa 3 (corregir permisos)
      ↓ no → continuar
5. Verificar puerto 22 → ¿hay otro proceso usando :22?
      ↓ sí → Causa 4 (liberar el puerto)
      ↓ no → Causa 5 (verificar RAM asignada)
```

---

## Verificación final

Una vez aplicada la solución, confirmar que el servicio arrancó correctamente:

```powershell
# Estado del servicio
Get-Service OpenSSHd

# Confirmar que escucha en puerto 22
netstat -ano | findstr :22

# Desde Kali: test de conectividad
ssh vagrant@<IP-de-la-VM>
```

Si la conexión SSH desde Kali responde con el banner SSH y solicita contraseña, el problema está resuelto.

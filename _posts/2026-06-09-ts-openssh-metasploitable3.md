---
title: "Troubleshooting: OpenSSH Detenido en Metasploitable3"
date: 2026-06-09
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

![Output](/assets/images/Openssh-stopped-troubleshooting-01.png)

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
Get-Content "C:\Program Files\OpenSSH\var\log\OpenSSHd.log"
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

![Verificación de binarios OpenSSH](/assets/images/get-wmiobject-opensshd.png)

> **Nota:** En mi caso ambos devuelven `True` y el servicio funciona. Verifica que en tu máquina ocurra lo mismo. Si alguno devuelve `False`, esa es la causa del fallo.

### Solución

Si `cygrunsrv.exe` o `sshd.exe` no existen, la instalación de OpenSSH está incompleta o corrompida. Buscar los archivos en el disco:

```powershell
Get-ChildItem -Path C:\ -Filter "sshd.exe" -Recurse -ErrorAction SilentlyContinue
Get-ChildItem -Path C:\ -Filter "cygrunsrv.exe" -Recurse -ErrorAction SilentlyContinue
```

Si no se encuentran en ninguna ruta, reinstalar OpenSSH para Windows desde la VM no es viable en un entorno de laboratorio. En ese caso, restaurar la VM desde un snapshot limpio o reimportar el OVA.

---

## Causa 3: Error de configuración o permisos en OpenSSH

**Probabilidad: Media.** La instalación de OpenSSH en `C:\Program Files\OpenSSH\` incluye directorios con permisos POSIX emulados sobre NTFS. Si esos permisos se corrompieron durante la importación del OVA, `sshd` falla al arrancar.

### Cómo detectarlo

Primero revisa el log (si aún no lo has hecho):

```powershell
Get-Content "C:\Program Files\OpenSSH\var\log\OpenSSHd.log"
```

Mensajes típicos de permisos incorrectos:
```
/var/empty must be owned by root and not group or world-writable.
Bad owner or permissions on /etc/ssh/sshd_config
```

También puedes ejecutar `sshd` en modo test directamente desde `cmd.exe`:

```cmd
"C:\Program Files\OpenSSH\usr\sbin\sshd.exe" -t
```

Si hay errores de configuración o permisos, los mostrará en la consola.

### Solución

Abrir `cmd.exe` **como Administrador** y ejecutar los comandos de la shell Cygwin incluida:

```cmd
"C:\Program Files\OpenSSH\bin\bash.exe" --login -c "chown root /var/empty && chmod 755 /var/empty"
```

```cmd
"C:\Program Files\OpenSSH\bin\bash.exe" --login -c "chmod 600 /etc/ssh/ssh_host_*_key && chmod 644 /etc/ssh/ssh_host_*_key.pub"
```

Luego intentar arrancar el servicio nuevamente:

```powershell
Start-Service OpenSSHd
Get-Service OpenSSHd
```

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

Si el proceso que ocupa el puerto 22 no es esencial, detenerlo:

```powershell
Stop-Process -Id <PID> -Force
Start-Service OpenSSHd
```

---

## Causa 5: Recursos insuficientes (RAM)

**Probabilidad: Baja.** Metasploitable3 requiere al menos **4 GB de RAM** asignados en VirtualBox. Con menos memoria, servicios como el SSH pueden fallar al iniciar.

### Cómo verificarlo

En VirtualBox (con la VM apagada): clic derecho sobre la VM → Configuración → Sistema → Memoria base. Debe ser ≥ 4096 MB.

### Solución

1. Apagar la VM
2. En VirtualBox: Configuración → Sistema → aumentar memoria a 4096 MB como mínimo
3. Reiniciar la VM

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

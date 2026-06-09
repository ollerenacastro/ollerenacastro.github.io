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

```powershell
# Ver también el Application log
Get-EventLog -LogName Application -Newest 30 | Where-Object {
    $_.Message -like "*ssh*" -or $_.Source -like "*cyg*"
} | Select-Object TimeGenerated, Source, EntryType, Message | Format-List
```

Anota el mensaje de error — te indicará cuál de las causas aplica.

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

## Causa 2: Claves de host SSH inexistentes o inválidas

**Probabilidad: Alta.** El demonio `sshd` requiere claves de host criptográficas en `C:\cygwin\etc\` para poder arrancar. Si estas claves no existen o tienen permisos incorrectos, el servicio falla silenciosamente.

### Cómo detectarlo

```powershell
# Verificar si las claves de host existen
Test-Path "C:\cygwin\etc\ssh_host_rsa_key"
Test-Path "C:\cygwin\etc\ssh_host_dsa_key"
Test-Path "C:\cygwin\etc\ssh_host_ecdsa_key"
```

Si alguno devuelve `False`, faltan las claves.

### Solución

Abrir una terminal de **Cygwin** (no PowerShell) y regenerar las claves:

```bash
# Desde la terminal de Cygwin (C:\cygwin\bin\bash.exe)
ssh-keygen -A
```

Si no hay un ícono de Cygwin Terminal, puedes abrirlo desde PowerShell:

```powershell
Start-Process "C:\cygwin\bin\bash.exe" -ArgumentList "--login", "-i"
```

Luego desde esa terminal bash:

```bash
ssh-keygen -A
# Deberías ver:
# Generating public/private rsa key pair...
# Generating public/private dsa key pair...
# etc.
```

Después vuelve a PowerShell y arranca el servicio:

```powershell
Start-Service OpenSSHd
Get-Service OpenSSHd
```

---

## Causa 3: Ruta del binario incorrecta (servicio apunta a un archivo que no existe)

**Probabilidad: Media.** El registro de Windows guarda la ruta del ejecutable del servicio. Si Cygwin no está instalado en `C:\cygwin\` o si el OVA fue importado con problemas de filesystem, la ruta puede estar rota.

### Cómo detectarlo

```powershell
# Ver la ruta registrada del servicio
(Get-WmiObject win32_service -filter "name='OpenSSHd'").PathName
```

Anota esa ruta (por ejemplo: `C:\cygwin\bin\sshd.exe -D`) y verifica que el archivo existe:

```powershell
Test-Path "C:\cygwin\bin\sshd.exe"
```

Si devuelve `False`, el binario no está donde el servicio lo busca.

### Solución

Verificar en qué unidad/ruta está realmente instalado Cygwin:

```powershell
# Buscar sshd.exe en el disco
Get-ChildItem -Path C:\ -Filter "sshd.exe" -Recurse -ErrorAction SilentlyContinue
```

Si se encuentra en otra ruta (ej. `C:\Program Files\OpenSSH\`), hay que re-registrar el servicio:

```powershell
# Detener el servicio actual
sc.exe delete OpenSSHd

# Re-registrar con la ruta correcta (ajustar según lo que encontraste arriba)
sc.exe create OpenSSHd binPath= "C:\ruta\correcta\sshd.exe -D" start= auto DisplayName= "OpenSSH Server"
Start-Service OpenSSHd
```

---

## Causa 4: Permisos incorrectos en directorios de Cygwin

**Probabilidad: Media.** Cygwin emula permisos POSIX sobre NTFS. El directorio `/var/empty` (requerido por sshd como `ChrootDirectory`) y `/etc/ssh/` necesitan permisos específicos. Una importación problemática del OVA puede romper estas ACLs.

### Cómo detectarlo

Abrir una terminal de Cygwin y ejecutar el diagnóstico de sshd:

```bash
# Desde Cygwin bash
/usr/sbin/sshd -t
```

Si hay errores de permisos, verás algo como:
```
/var/empty must be owned by root and not group or world-writable.
```

### Solución

Desde una terminal de Cygwin con privilegios de Administrador:

```bash
# Corregir permisos del directorio requerido por sshd
chown root /var/empty
chmod 755 /var/empty
chmod 600 /etc/ssh/ssh_host_*_key
chmod 644 /etc/ssh/ssh_host_*_key.pub
```

---

## Causa 5: Puerto 22 ya en uso

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

## Causa 6: Recursos insuficientes (RAM)

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
3. Verificar claves de host → ¿existen ssh_host_*_key?
      ↓ no → Causa 2 (ssh-keygen -A)
      ↓ sí → continuar
4. Verificar ruta del binario → ¿existe C:\cygwin\bin\sshd.exe?
      ↓ no → Causa 3 (re-registrar servicio)
      ↓ sí → continuar
5. Ejecutar sshd -t desde Cygwin → ¿hay errores de permisos?
      ↓ sí → Causa 4 (corregir permisos)
      ↓ no → continuar
6. Verificar puerto 22 → ¿hay otro proceso usando :22?
      ↓ sí → Causa 5 (liberar el puerto)
      ↓ no → Causa 6 (verificar RAM asignada)
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

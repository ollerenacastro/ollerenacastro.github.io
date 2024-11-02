---
title: Examen Parcial
date: 2024-10-31 06:20:00 -0500
categories: [Cybersecurity,Ethical Hacking,Pentesting,Exploitation Techniques,Buffer Overflows]
tags: [buffer overflow,exploit development,pentesting,reverse shell,msfvenom,shellcode,eip control,bad characters,fuzzing,xvulnerabilities]     # TAG names should always be lowercase
---

<hr style="border: none; height: 10px; background-color: #003b00;" />

# <font color="#87CEEB">Examen Parcial.</font>

<hr style="border: none; height: 10px; background-color: #003b00;" />

## 1. Preliminares

### 1.1. En la máquina Metasploitable 3

- Asegúrese de tener la máquina virtual en modo Nat Network.

- Asegúrese de tener el Firewall de Windows desactivado.

- Realizar los siguientes pasos ya que por defecto, la máquina virtual Metasploitable 3 no tiene el servicio SMBv1 habilitado.

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

- Comprobar que tiene el servicio `SMBv1` funcionando.

  ```powershell
  PS C:\Users\vagrant> netstat -an | Select-String "139|445"

    TCP    0.0.0.0:445            0.0.0.0:0              LISTENING
    TCP    10.0.2.15:139          0.0.0.0:0              LISTENING
    TCP    [::]:445               [::]:0                 LISTENING
  ```

### 1.2. En la máquina Kali Linux

- Asegúrese de tener la máquina virtual en modo Nat Network.

### 1.3. Setup del laboratorio


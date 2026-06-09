---
status: complete
date: 2026-06-09
commit: 339138e
---

# Summary: Troubleshooting OpenSSH Stopped en Metasploitable3

## Lo que se hizo
- Creado `_posts/2026-06-09-ts-openssh-metasploitable3.md` con análisis offline de 6 causas del síntoma "OpenSSHd Stopped + StartServiceFailed"
- Causas cubiertas en orden de probabilidad: StartMode Manual, host keys SSH faltantes, ruta del binario rota, permisos Cygwin incorrectos, puerto 22 ocupado, RAM insuficiente
- Incluye flujo de diagnóstico secuencial y comandos de verificación final
- Agregada nota al final de `_posts/2026-05-31-tema03.md` con link al troubleshooting

## Commit
339138e — content: agregar guia troubleshooting OpenSSH Stopped en Metasploitable3

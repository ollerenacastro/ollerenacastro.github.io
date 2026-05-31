---
quick_id: 260531-fyy
type: quick
phase: 01-revisar-tema03-plantilla
autonomous: true
files_modified:
  - _posts/2026-05-31-tema03.md
---

<objective>
Enriquecer la sección ## Kill Chain 2: Jenkins Script Console RCE en tema03 expandiendo
el ### Fundamento Técnico con: narrativa histórica del movimiento DevOps (2009-2012),
explicación de Groovy/JVM, el escenario de auditoría interna, impacto real post-RCE, y
contexto preciso de T1190.

Purpose: El fundamento técnico actual es correcto pero escueto — los estudiantes no entienden
POR QUÉ Jenkins estaba sin contraseña ni QUÉ significa realmente comprometer un servidor CI/CD.
Output: Sección ### Fundamento Técnico ampliada con subsección #### Impacto real: más allá del whoami.
</objective>

<context>
@_posts/2026-05-31-tema03.md
</context>

<tasks>

<task type="auto">
  <name>Tarea 1: Expandir ### Fundamento Técnico en Kill Chain 2 (líneas 1246-1253)</name>
  <files>_posts/2026-05-31-tema03.md</files>
  <action>
Reemplazar el bloque existente ### Fundamento Técnico: Misconfiguration vs. CVE
(actualmente líneas 1246-1253, dos párrafos cortos) con el contenido expandido siguiente.
NO modificar ninguna línea antes de ## Kill Chain 2 (línea 1244) ni después de
### Lab: Ejecución de código Groovy (línea 1255 en adelante).

El bloque antiguo a reemplazar es exactamente:

```
### Fundamento Técnico: Misconfiguration vs. CVE

Jenkins es una plataforma de integración continua que expone una consola de scripts Groovy
accesible para administradores. En Metasploitable3, esta consola está expuesta en el puerto
8484 **sin autenticación**: cualquier usuario que acceda a la URL puede ejecutar código
arbitrario en el servidor.

Este ataque corresponde a **T1190 — Exploit Public-Facing Application** (Initial Access) y **T1059 — Command and Scripting Interpreter** (Execution). A diferencia de EternalBlue, no se explota un CVE — se abusa de una configuración insegura por diseño del entorno de lab.
```

Reemplazar por el siguiente bloque (tono educativo, español, markdown estándar Jekyll/Chirpy):

---

### Fundamento Técnico: Misconfiguration vs. CVE

#### Contexto histórico: DevOps y el perímetro disuelto

Alrededor de 2009-2012, el movimiento **DevOps** irrumpió con la promesa de pasar de
desplegar código una vez al mes a varias veces al día. **Jenkins**, lanzado en 2011 como
fork de Hudson (creado por Kohsuke Kawaguchi en Sun Microsystems), se convirtió en la
herramienta central de esa revolución.

En ese contexto la velocidad lo era todo: los equipos querían que Jenkins "simplemente
funcionara" — build, tests, deploy sin fricción. La seguridad era consideración secundaria.
La mentalidad dominante era: *"Jenkins vive detrás del firewall corporativo, solo el equipo
de dev puede llegar. ¿Para qué necesita contraseña?"*

Este razonamiento era comprensible en 2012 con redes de perímetro claro. Pero cuando Jenkins
migró a AWS/Azure/GCP y los equipos empezaron a trabajar desde cafeterías con VPNs, el
perímetro se disolvió. Para 2019, Shodan registraba más de **50,000 instancias Jenkins
accesibles desde internet sin credenciales** — el FBI y CISA emitieron alertas específicas
sobre Jenkins mal configurado como vector de ataque activo.

#### Qué es Groovy y por qué da acceso total

**Groovy** es un lenguaje de scripting que corre sobre la JVM (la misma máquina virtual de
Java). La Script Console de Jenkins es un intérprete Groovy con acceso completo a la JVM y
al sistema operativo del servidor — **no es un sandbox limitado**. Tiene exactamente los
mismos permisos que el proceso Jenkins.

#### El escenario de auditoría

Como auditores, estamos dentro de la red. El `nmap -A` de la Etapa 1 ya nos reveló el
puerto 8484 con `http-title: Dashboard [Jenkins]`. Accedemos a `http://10.0.2.15:8484/`
— el dashboard carga sin credenciales. Navegamos a `/script` y encontramos el intérprete
Groovy, sin restricciones de acceso.

#### Impacto real: más allá del whoami

El `whoami` demuestra ejecución remota de código, pero el riesgo real es otro: **Jenkins
almacena credenciales para todos los servidores a los que despliega** — SSH keys a
producción, tokens de cloud, contraseñas de bases de datos. Comprometer Jenkins equivale
a acceso potencial a toda la infraestructura que ese Jenkins administra.

En ataques de supply chain, inyectar código malicioso en el pipeline significa que ese
código llega a todos los artefactos que Jenkins construye y distribuye.

Ejemplos de lo que un atacante puede extraer directamente desde la Script Console:

```groovy
// Leer variables de entorno (pueden contener tokens, API keys, passwords)
println System.getenv()

// Leer archivos del sistema — claves SSH, configuraciones sensibles
println new File("C:/Users/Administrator/.ssh/id_rsa").text

// Extraer TODAS las credenciales almacenadas en Jenkins
import com.cloudbees.plugins.credentials.*
CredentialsProvider.lookupCredentials(
  StandardCredentials.class, Jenkins.instance, null, null
).each { println it.id + ": " + it }
```

#### T1190 en este contexto

**T1190 — Exploit Public-Facing Application** es la técnica MITRE para explotar debilidades
en aplicaciones expuestas a internet (o a la red interna). El "exploit" aquí no es un CVE
clásico — es un fallo de **control de acceso**: la aplicación ejecuta lo que se le pide
porque no verifica quién está pidiendo. MITRE lo clasifica en **Initial Access** porque es
el punto de entrada al sistema. A diferencia de EternalBlue (T1210), no hay shellcode: la
aplicación misma es el intérprete.

---

Después del bloque reemplazado, la sección ### Lab: Ejecución de código Groovy debe
continuar exactamente como está, sin ninguna modificación.
  </action>
  <verify>
    <automated>grep -n "Contexto histórico" _posts/2026-05-31-tema03.md | head -3</automated>
    <automated>grep -n "Impacto real" _posts/2026-05-31-tema03.md | head -3</automated>
    <automated>grep -n "## Kill Chain 3" _posts/2026-05-31-tema03.md | head -3</automated>
  </verify>
  <done>
    - La subsección "#### Contexto histórico: DevOps y el perímetro disuelto" existe en el post
    - La subsección "#### Impacto real: más allá del whoami" existe con los tres bloques Groovy
    - La subsección "#### T1190 en este contexto" existe
    - ## Kill Chain 3 sigue siendo la sección inmediatamente posterior a Kill Chain 2 (sin contenido intercalado)
    - bundle exec jekyll build no reporta errores de front matter ni liquid
  </done>
</task>

</tasks>

<verification>
```bash
# Confirmar las cuatro subsecciones nuevas presentes
grep -n "#### Contexto histórico\|#### Qué es Groovy\|#### El escenario\|#### Impacto real\|#### T1190" _posts/2026-05-31-tema03.md

# Confirmar que Kill Chain 3 no fue afectada
grep -n "## Kill Chain 3" _posts/2026-05-31-tema03.md

# Confirmar que Lab: Ejecución sigue intacto
grep -n "### Lab: Ejecución de código Groovy" _posts/2026-05-31-tema03.md

# Build limpio
bundle exec jekyll build 2>&1 | grep -i error
```
</verification>

<success_criteria>
- Cinco nuevas subsecciones `####` bajo ### Fundamento Técnico en Kill Chain 2
- Tres bloques de código Groovy de impacto real presentes y correctamente delimitados
- ### Lab: Ejecución de código Groovy (verificación de servicio + pasos 1-5) sin cambios
- ## Kill Chain 3 comienza inmediatamente después del cierre de Kill Chain 2
- Jekyll build sin errores
</success_criteria>

<output>
Commit con mensaje: `content(tema03): expand Kill Chain 2 technical foundation — DevOps history, Groovy/JVM, real impact`
Actualizar STATE.md quick tasks con entrada 260531-fyy.
</output>

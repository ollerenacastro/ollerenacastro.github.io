---
quick_id: 260531-fyy
type: quick
status: complete
date: "2026-05-31"
commit: 618a517
files_modified:
  - _posts/2026-05-31-tema03.md
duration_minutes: 5
---

# Quick Task 260531-fyy Summary

**One-liner:** Expanded Kill Chain 2 Fundamento Tecnico with DevOps history (2009-2012), Groovy/JVM explanation, internal audit scenario, real supply-chain impact with three Groovy exploit examples, and precise T1190 MITRE context.

## What Was Done

Replaced the two-paragraph `### Fundamento Tecnico: Misconfiguration vs. CVE` block in `## Kill Chain 2: Jenkins Script Console RCE` with five new `####` subsections:

1. **Contexto historico: DevOps y el perimetro disuelto** - Jenkins origin (2011, fork of Hudson), DevOps movement 2009-2012, "no password needed" mindset, perimeter dissolution into cloud, Shodan 50k stat, FBI/CISA alerts.
2. **Que es Groovy y por que da acceso total** - Groovy as JVM scripting language, Script Console as non-sandboxed interpreter with full OS permissions.
3. **El escenario de auditoria** - Frames the student as internal auditor, connecting nmap discovery from Stage 1 to accessing the exposed console.
4. **Impacto real: mas alla del whoami** - Three Groovy code examples: System.getenv(), reading SSH id_rsa, and extracting all Jenkins stored credentials via CredentialsProvider API. Supply chain attack framing.
5. **T1190 en este contexto** - Precise MITRE classification: access-control failure not CVE, Initial Access entry point, contrast with EternalBlue T1210.

## Verification

```
#### Contexto historico: DevOps y el perimetro disuelto  -> line 1248
#### Que es Groovy y por que da acceso total             -> line 1266
#### El escenario de auditoria                           -> line 1273
#### Impacto real: mas alla del whoami                   -> line 1280
#### T1190 en este contexto                              -> line 1306
### Lab: Ejecucion de codigo Groovy                      -> line 1315 (unchanged)
## Kill Chain 3: EternalBlue / MS17-010                  -> line 1340 (unchanged)
```

All done criteria met. No content before `## Kill Chain 2` (line 1244) was modified. No content after the new block through `## Kill Chain 3` was modified.

## Deviations

None - plan executed exactly as written.

## Commit

- `618a517` - `content(tema03): expand Kill Chain 2 technical foundation - DevOps history, Groovy/JVM, real impact`

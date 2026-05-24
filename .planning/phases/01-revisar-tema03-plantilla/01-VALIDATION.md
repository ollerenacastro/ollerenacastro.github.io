---
phase: 1
slug: revisar-tema03-plantilla
status: draft
nyquist_compliant: false
wave_0_complete: false
created: 2026-05-24
---

# Phase 1 — Validation Strategy

> Per-phase validation contract for feedback sampling during execution.

---

## Test Infrastructure

| Property | Value |
|----------|-------|
| **Framework** | Jekyll build (bundle exec) + grep assertions |
| **Config file** | `_config.yml` |
| **Quick run command** | `bundle exec jekyll build --quiet 2>&1 \| tail -5` |
| **Full suite command** | `bundle exec jekyll build 2>&1 && echo "BUILD OK"` |
| **Estimated runtime** | ~15 seconds |

---

## Sampling Rate

- **After every task commit:** Run `bundle exec jekyll build --quiet`
- **After every plan wave:** Run full build + grep structural assertions
- **Before `/gsd:verify-work`:** Full suite must be green, post visible in `_site/`
- **Max feedback latency:** ~20 seconds

---

## Per-Task Verification Map

| Task ID | Plan | Wave | Requirement | Threat Ref | Secure Behavior | Test Type | Automated Command | File Exists | Status |
|---------|------|------|-------------|------------|-----------------|-----------|-------------------|-------------|--------|
| 01-01-01 | 01 | 1 | CONT-01 | — | N/A | build | `bundle exec jekyll build --quiet` | ✅ | ⬜ pending |
| 01-01-02 | 01 | 1 | STR-01 | — | N/A | grep | `grep -c "## Objetivos" _posts/2026-*-tema03.md` | ✅ | ⬜ pending |
| 01-01-03 | 01 | 1 | STR-02 | — | N/A | grep | `grep -c "Jenkins\|EternalBlue\|MS17-010" _posts/2026-*-tema03.md` | ✅ | ⬜ pending |
| 01-01-04 | 01 | 1 | STR-03 | — | N/A | grep | `grep -c "MITRE\|ATT&CK\|T1" _posts/2026-*-tema03.md` | ✅ | ⬜ pending |

*Status: ⬜ pending · ✅ green · ❌ red · ⚠️ flaky*

---

## Wave 0 Requirements

- Existing infrastructure covers all phase requirements.
  - Jekyll build system is already configured and functional
  - No test framework installation needed — verification is via `bundle exec jekyll build` and `grep`

---

## Manual-Only Verifications

| Behavior | Requirement | Why Manual | Test Instructions |
|----------|-------------|------------|-------------------|
| Post visible in GitHub Pages | CONT-01 | Requires git push + CI/CD pipeline | Push to main, wait for GitHub Actions, verify at site URL |
| Jenkins Groovy output matches OVAs | STR-02 | Requires running VMs | Boot Kali + Metasploitable3, run `http://10.0.2.15:8484/jenkins/script`, verify Groovy output |
| EternalBlue shell SYSTEM confirmed | STR-02 | Requires running VMs | Run ms17_010_eternalblue module, verify `getuid` returns `NT AUTHORITY\SYSTEM` |
| Kill chain commands reproducible end-to-end | STR-01 | Requires lab environment | Follow post from start to finish on the OVAs |

---

## Validation Sign-Off

- [ ] All tasks have `<automated>` verify or Wave 0 dependencies
- [ ] Sampling continuity: no 3 consecutive tasks without automated verify
- [ ] Wave 0 covers all MISSING references
- [ ] No watch-mode flags
- [ ] Feedback latency < 30s
- [ ] `nyquist_compliant: true` set in frontmatter

**Approval:** pending

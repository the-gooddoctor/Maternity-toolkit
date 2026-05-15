# Research artefacts — Seed PRD

Multi-agent research reports preserved for traceability. Every change in PRD v2.1 and v2.2 can be traced back to a specific finding in one of these reports.

## Pass 1 (v2.0 → v2.1): five agents

| File | Agent | Focus |
|---|---|---|
| `01_regulatory_clinical_validation.md` | Regulatory and clinical | UK MDR / MHRA, NICE guideline IDs, ICO Children's Code, charity helplines, Apple/Google health-app policies |
| `02_competitive_market_analysis.md` | UK competitive market | Top 10 UK pregnancy apps 2026, pricing, September 2025 Flo settlement, UK birth statistics, ASO keywords |
| `03_technical_stack_validation.md` | Technical stack | Expo SDK 55 / RN 0.83 / MMKV v4, critical code bugs in v2.0, New Architecture |
| `04_ux_accessibility_sensitivity.md` | UX, accessibility, sensitivity | WCAG 2.2 AA gaps, colour contrast, inclusive language, loss-charity expansion, postnatal scope mismatch |
| `05_gap_analysis_completeness.md` | Gap analysis | Support model, ToS, QA, CI/CD, release management, GDPR Article 20, OTA policy, edge cases, roadmap |

See `../PRD_v2_0_to_v2_1_CHANGELOG.md` for the synthesised delta from v2.0 to v2.1.

## Pass 2 (v2.1 → v2.2): six agents

| File | Agent | Focus |
|---|---|---|
| `06_v2_1_internal_consistency.md` | Internal consistency | SED-* ID inventory (287 unique), broken refs, stale fields, doc hygiene, pricing arithmetic error |
| `07_deeper_regulatory.md` | Deeper regulatory | DPIA-lite, vulnerability disclosure, MHRA Innovation Office, store-suspension contingency, NICE ESF tier, charity URL/name corrections |
| `08_deeper_technical.md` | Deeper technical | Verification of v2.1 fixes; critical Proxy + async-bootstrap boot-crash; Buffer import; expo-print tagged-PDF infeasibility; PBKDF2 iterations |
| `09_content_audit.md` | Content audit | Prohibited-term violations in v2.1 (recommends ×4, normal ×3, harmless, you should), reading-age failures in banners/briefs, UK spelling (Fetal→Foetal), clinical-jargon glosses |
| `10_operational_maturity.md` | Operational maturity | Deputy CSO, vulnerability disclosure (`SECURITY.md`/`security.txt`), incident response, OSS licences, insurance, DSAR SOP, status page, 15 runbook documents |
| `11_implementation_readiness.md` | Implementation readiness | 60-day timeline reality (81–111 day envelope); v1.0/v1.0.x/v1.1 cut list; critical-path dependencies; spec ambiguities; content authoring workflow; brand-rename SOP |

See `../PRD_v2_1_to_v2_2_CHANGELOG.md` for the synthesised delta from v2.1 to v2.2.

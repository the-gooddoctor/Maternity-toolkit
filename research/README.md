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

## Pass 3 (v2.2 → v2.3): twelve agents — feature optimisation

| File | Agent | Focus |
|---|---|---|
| `12_voice_of_uk_users.md` | Voice of UK users | Top 20 most-requested features, top 15 complaints, underserved populations, daily-ritual map, 20 prioritised feature recommendations — drawn from Mumsnet, Reddit r/PregnancyUK, App Store / Play Store reviews of Pregnancy+/Flo/BabyCentre/Baby Buddy, Mozilla Privacy Not Included, UK academic studies |
| `13_f01_f02_dashboard.md` | F01 + F02 dashboard / week tracker | 26 features incl. profile-aware rendering (SED-F02-008), per-week diary, mood note, extended term window (37–42+), extra-care self-declaration |
| `14_f03_contraction_timer.md` | F03 contraction timer + Labour Mode | 25 features incl. confirm-on-end, enriched summary, "Read to your midwife" card, partner-operated UI, battery banner, "I'm in labour" home entry, SED-F03-ANTI |
| `15_f04_kick_counter_movement_diary.md` | F04 movement diary | Conceptual rename; signposting-before-counting; **cold-drink prompt REMOVED** (Tommy's contraindicates); session-start screen; sensitive mode; twin mode; SED-F04-ANTI |
| `16_f05_weight_tracker.md` | F05 weight tracker | Opt-in onboarding gate; reference band default-OFF; write-only mode; BEAT signposting; hyperemesis mode; recommendation to move to Free tier |
| `17_f06_f08_f09_admin_trio.md` | F06 + F08 + F09 practical admin trio | 30 features incl. week-34 bag prompt, Labour-Mode grab-now subset, UK maternity rights 10-item subsection, F08↔F09 auto-tick, calendar export, recurring series, twin schedules |
| `18_f07_birth_plan.md` | F07 birth plan builder | 13 features incl. auto-save version history, "If things change" 5 sub-scenarios with defaults, trauma-informed section, VBAC/twin branches, must-haves max-5, multi-page PDF + partner companion PDF + content fix (A.3 "recommends" → "lists") |
| `19_f10_baby_names_delight.md` | F10 baby names | v1.0 1-hour stand-in + full v1.1 spec (24 features) incl. curated lists, meaning search, pronunciation, couple veto/discussed/private tiers, cultural collections curated by named contributors, reflection ritual, mask mode |
| `20_partner_coparent_surrogacy.md` | Partner / co-parent / surrogacy IP / solo / same-sex modes | Partner-mode Home dashboard; pregnant-person name/pronouns fields; `partnerRole` for same-sex co-parents; full IP path with parental-order timeline; solo path; partner education library (9 sections); long-distance and older-children-in-household modes |
| `21_twin_multiples_mode.md` | Twin / multiples mode | Chorionicity capture in onboarding (no longer v1.1-deferred); twin welcome; week-specific twin callouts (14 weeks); multiples educational library (12 cards); DC/DA + MC/DC schedules; twin birth-plan branch; Twins Trust partnership levels |
| `22_loss_pathway_mental_wellbeing.md` | Loss pathway + mental wellbeing | Memorial state lifecycle; "Save a record" memorial page; anniversary acknowledgement; pregnancy-after-loss lite mode (v1.0); soft pause "while I find out"; gestation-aware charity highlighting; mental wellbeing library expanded 4→18 cards |
| `23_daily_ritual_delight_retention.md` | Daily ritual + delight + retention without telemetry | **SED-UX-027 anti-pattern catalogue (AP-01..30)** — engagement-farming patterns explicitly prohibited at any version; SED-UX-028 Pro upsell 5-touchpoint discipline; SED-UX-026 60-second value-confirmation beat; Memories PDF; subsequent-pregnancy welcome flow; post-birth sign-off screen |

Plus F11 postnatal v1.1 full spec embedded in PRD §3.11 (sources/effort estimate in `23_*` and PRD itself).

See `../PRD_v2_2_to_v2_3_CHANGELOG.md` for the synthesised delta from v2.2 to v2.3.

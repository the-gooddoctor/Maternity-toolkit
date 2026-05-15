# Maternity-toolkit

## Seed — Privacy-first UK pregnancy toolkit

This repo contains the Product Requirements Document and scaffold for **Seed**, a privacy-first, offline-only UK pregnancy app targeted for May 2026 launch.

### Files

| File | Purpose |
|---|---|
| `Seed_Maternity_Toolkit_PRD_v2_3.md` | **Current source of truth.** PRD v2.3 (May 2026 — feature-optimisation pass). |
| `Seed_Maternity_Toolkit_PRD_v2_2.md` | Previous version (May 2026 — second audit pass). |
| `Seed_Maternity_Toolkit_PRD_v2_1.md` | First multi-agent revision (May 2026). |
| `Seed_Maternity_Toolkit_PRD_v2_0.md` | Original version (Feb 2026). |
| `PRD_v2_2_to_v2_3_CHANGELOG.md` | Summary of every change v2.2 → v2.3 (new F11 postnatal spec, new F12 nutrition feature, anti-pattern catalogue, memorial state, pregnancy-after-loss lite mode, partner/IP/solo paths, twin chorionicity, F04 cold-drink-prompt removal, ~120 new requirements). |
| `PRD_v2_1_to_v2_2_CHANGELOG.md` | Summary of every change v2.1 → v2.2. |
| `PRD_v2_0_to_v2_1_CHANGELOG.md` | Summary of every change v2.0 → v2.1. |
| `research/` | 23 multi-agent research reports (5 from pass 1, 6 from pass 2, 12 from pass 3 feature-optimisation). |
| `seed_expo_scaffold.zip` | Initial Expo + TypeScript project scaffold. |
| `maternity_toolkit_repo.zip` | Earlier working repo snapshot. |

### What v2.3 adds vs v2.2

v2.3 is the output of a THIRD multi-agent research pass (12 parallel agents covering voice-of-user research, every feature F01–F10 in depth, partner / twin / loss / wellbeing cross-cutting modes, daily-ritual & retention, and two new feature categories F11 postnatal + F12 nutrition/lifestyle). Highlights:

- **F11 postnatal toolkit (v1.1, full spec)** — 40 new requirements covering days-since-birth dashboard, count-only feeding/nappy/sleep logs with inclusive methods, postnatal physical-recovery content, mental wellbeing with postnatal psychosis emergency card, Lullaby Trust safer sleep pinned, memorial state, NICU pathway, per-baby twin logging, MHRA Regulatory Advice meeting pre-GA, dual clinical sign-off (CSO + perinatal-MH-trained reviewer).
- **F12 "What the NHS advises" (v1.0-lite + v1.0.x full)** — verbatim NHS food/drink/lifestyle reference library. 1-hour stand-in ships v1.0 (12 NHS deep-links); full ~106-entry searchable library ships v1.0.x. Free tier. Healthy Start signposting prominent. BUMPS integration. Onboarding flags for eating-disorder sensitivity and hyperemesis.
- **SED-UX-027 anti-pattern catalogue (AP-01 through AP-30)** — 30 engagement-farming patterns explicitly prohibited in any release, CI-enforceable. Encodes the §18 non-roadmap into a code-review check.
- **Memorial state lifecycle** — `lifecycleStage: 'pregnancy' | 'postnatal' | 'memorial'`. "Save a record" memorial page with opt-in anniversary acknowledgement. No UK pregnancy app currently offers this.
- **Pregnancy-after-loss v1.0 lite mode** — `birthSensitivity = 'trauma_aware'` flag suppresses fruit-vegetable size content, reframes milestones as factual not celebratory, surfaces pregnancy-after-loss resources.
- **Partner-mode dashboard** — partner mode is no longer just a content filter; it has its own Home tab layout, partner education library, surrogacy intended-parent path, solo-parent path, same-sex co-parent `partnerRole` field.
- **Twin/multiples mode substantially deepened** — chorionicity captured in onboarding (v1.0, was v1.1-deferred); 14 weeks of substantive twin content; multiples educational library (12 cards); DC/DA + MC/DC pre-populated schedules; twin birth-plan branch; NICU "go bag" sub-section.
- **F04 cold-drink prompt REMOVED** — Tommy's Movements Matter campaign actively contraindicates it. Codified as SED-F04-ANTI / AP-21. F04 conceptually renamed "Movement diary" per RCOG GTG-57 2nd edition.
- **Critical content fix** — Appendix A.3 `data/birth-plan-options.json` "NICE recommends" → "The NHS lists" (the only surviving SED-SAF-005 violation; would have failed `scripts/audit-terms.js` CI).
- **F05 weight tracker sensitive defaults** — opt-in onboarding gate; reference band default-OFF (was on); write-only mode with no echo on save; BEAT signposting tied to UI state. Recommendation to move F05 to Free tier.
- **Mental wellbeing library** expanded from 4 organisations to ~18 cards (perinatal OCD, postnatal psychosis emergency, tokophobia, Black maternal MH, hyperemesis distress, etc.).
- **Universal "Call your maternity unit" footer** (SED-CC-030) — voice-of-user research identified as highest-leverage missing surface.
- **F02 profile-aware rendering** (SED-F02-008) — single highest-leverage F02 change. Per-week diary, mood note (CSO sign-off required), milestone log, daily prompt rotation, alternative size representations.
- **F07 birth plan** — auto-save with version history; "If things change" 5 sub-scenarios with pre-written defaults; trauma-informed section; cultural/disability sections; VBAC and twin branches; must-haves max-5 tagging; multi-page PDF; partner companion PDF.
- **F08 UK maternity rights subsection** — 10 gov.uk / Acas / NHSBSA / HSE items pinned in T1/T2.
- **F09 outcomes capture, calendar export, multi-cadence reminders, travel-time offset, appointment letter paste.**
- **Memories PDF export** at week 37+ / lifecycle-stage transition.

Net: ~120 new SED-* requirements. v1.0 envelope tight at 70–95 dev-days against 60-day target — cut list at §3.0a / §3.0b is the governance document.

### What v2.2 fixes vs v2.1

v2.2 is the output of a SECOND parallel multi-agent research pass. Highlights:

- **Boot-crash bug:** v2.1's encryption-key bootstrap (async `initMMKV` + Proxy pattern) would throw at module-load time because Zustand v5 `persist` hydrates synchronously at `create()` time. v2.2 fixes with synchronous `SecureStore.getItem` + `Crypto.getRandomBytes` (both available since Expo SDK 51). New SED-PRI-008.
- **Pricing arithmetic:** v2.1 claimed £4.16 net "after VAT and commission" — actually VAT-net before commission. Real net after 15% commission and 20% VAT = **£3.53**. Target scenario in v2.1 (15k × 5% = £3,120) was actually £2,648, below the £3,000 success metric. v2.2 corrects throughout.
- **SettingsState interface** missed 6 fields that v2.1 prose mandated (`userMode` 4-value enum, `showPartnerContent`, `trackingStatus: 'hidden'`, `disclaimerV`, `weightChartHidden`, `hapticsDisabled`). v2.2 reconciles.
- **Cut list (§3.0a):** v2.1 full scope estimated 81–111 dev-days, not 60. v2.2 introduces explicit v1.0 / v1.0.x / v1.1 ship list saving ~15–23 dev-days for v1.0.
- **Prohibited terms** in v2.1 evidence summaries and "What to expect" briefs ("recommends" ×4, "normal" ×3, "harmless", "you should") — all rewritten; SED-SAF-005 exception list formalised.
- **Reading-age failures** in v2.1 banners (F03-014 27-word sentence, F09 booking brief 24-word comma-stack, F09 dating-scan 26-word, 36-week ECV unglossed) — all rewritten for grade 6.
- **Tagged-PDF requirement** (SED-F07-009) was physically impossible with `expo-print` — downgraded to semantic HTML + plain-text alternative; tagged PDF deferred to v1.0.x.
- **`Buffer` import** in v2.1 won't bundle under SDK 55 — replaced with `globalThis.btoa`.
- **Keychain accessibility** tightened from `AFTER_FIRST_UNLOCK` to `WHEN_UNLOCKED_THIS_DEVICE_ONLY` (prevents iCloud Keychain sync of encryption key).
- **PBKDF2 iterations** raised from 100k to 200k for backup encryption.
- **Appendix legacy artefacts** fixed (A.1 legacy NHS URL, A.4 `gender` → `nameStyle`, B old Miscarriage Association number, Birth Trauma `.org.uk` → `.org`, Mariposa Trust → Mariposa International).
- **Stale field references** (`isMultiple`, `trackingPaused`, `trackingEnded`) replaced with current enum values.
- **SED-ARCH-004** still mandated dropped `zustand-mmkv-storage` package — corrected.
- **Phase-1 install commands** still included `gluestack-ui` and `npm install zustand-mmkv-storage` — corrected.
- **TOC** missing §18; **§13** had no parent heading; **§11.2/11.3** out of order — fixed.

### What v2.2 adds (new sections)

§3.0a cut list, §5.2a prohibited-terms exception list, §5.6 CSO bandwidth (deputy CSO, NICE ESF, MHRA Innovation Office), §6.8 content authoring workflow, §7.5 privacy expansion (DSAR, RevenueCat controllership, DPIA-lite, breach playbook), §7.6 OSS licence acknowledgements, §7.7 accessibility statement, §7.8 verbatim NHS quotation policy, §11.4 brand-rename SOP, §12.11 incident response, §12.12 service health and observability, §12.13 tooling inventory, §12.14 dependency pinning, §12.15 documentation/bus-factor mitigation, §12b.5 tax/accounting/insurance, §12b.6 user communication channels, §13.6 press kit. ~50 new SED-* requirements.

### Risk register

v2.2 expands to R01–R38. R13–R18 closed (v2.1 bugs fixed). R21–R34 new (operational, security, legal, timeline). R35–R38 retroactively close v2.1 issues.

### Multi-agent research methodology

Each PRD revision was produced by parallel agents:
- Pass 1 (v2.0 → v2.1): 5 agents (regulatory, competitive, technical, UX/a11y, gap analysis)
- Pass 2 (v2.1 → v2.2): 6 agents (internal consistency, deeper regulatory, deeper technical, content audit, operational maturity, implementation readiness)
- Pass 3 (v2.2 → v2.3): 12 agents (voice-of-user, F01+F02 dashboard, F03 contraction timer, F04 movement diary, F05 weight tracker, F06+F08+F09 admin trio, F07 birth plan, F10 baby names, partner/IP/surrogate/solo/same-sex modes, twin/multiples mode, loss + mental wellbeing, daily ritual/delight/retention, plus F11 postnatal full spec and F12 nutrition feature)

23 reports total preserved under `research/`. See `research/README.md` for the full index.

### Working branch

Active development: `claude/multi-agent-prd-research-AoneB`.

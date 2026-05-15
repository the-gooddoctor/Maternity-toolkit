# Maternity-toolkit

## Seed — Privacy-first UK pregnancy toolkit

This repo contains the Product Requirements Document and scaffold for **Seed**, a privacy-first, offline-only UK pregnancy app targeted for May 2026 launch.

### Files

| File | Purpose |
|---|---|
| `Seed_Maternity_Toolkit_PRD_v2_2.md` | **Current source of truth.** PRD v2.2 (May 2026). |
| `Seed_Maternity_Toolkit_PRD_v2_1.md` | Previous version (May 2026, first multi-agent revision). |
| `Seed_Maternity_Toolkit_PRD_v2_0.md` | Original version (Feb 2026). |
| `PRD_v2_1_to_v2_2_CHANGELOG.md` | Summary of every change v2.1 → v2.2 (boot-crash fix, pricing arithmetic, SettingsState reconciliation, cut list, ~50 new requirements). |
| `PRD_v2_0_to_v2_1_CHANGELOG.md` | Summary of every change v2.0 → v2.1. |
| `research/` | 11 multi-agent research reports (5 from pass 1, 6 from pass 2). |
| `seed_expo_scaffold.zip` | Initial Expo + TypeScript project scaffold. |
| `maternity_toolkit_repo.zip` | Earlier working repo snapshot. |

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

11 reports total preserved under `research/`. See `research/README.md` for the full index.

### Working branch

Active development: `claude/multi-agent-prd-research-AoneB`.

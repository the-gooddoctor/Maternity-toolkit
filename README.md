# Maternity-toolkit

## Seed — Privacy-first UK pregnancy toolkit

This repo contains the Product Requirements Document and scaffold for **Seed**, a privacy-first, offline-only UK pregnancy app targeted for May 2026 launch.

### Files

| File | Purpose |
|---|---|
| `Seed_Maternity_Toolkit_PRD_v2_1.md` | **Current source of truth.** PRD v2.1 (May 2026). |
| `Seed_Maternity_Toolkit_PRD_v2_0.md` | Previous version (Feb 2026). Preserved for reference. |
| `PRD_v2_0_to_v2_1_CHANGELOG.md` | Summary of every material change from v2.0 to v2.1. |
| `research/` | Five multi-agent research reports that informed v2.1: regulatory, competitive, technical, UX/a11y, gap analysis. |
| `seed_expo_scaffold.zip` | Initial Expo + TypeScript project scaffold. |
| `maternity_toolkit_repo.zip` | Earlier working repo snapshot with data JSONs and Codex docs. |

### What changed in v2.1

v2.1 is the output of a parallel multi-agent research pass against v2.0. Highlights:

- **Critical fixes:** hard-coded MMKV encryption key (privacy regression), timer hook writing to unencrypted instance, missing `createJSONStorage` wrapper for Zustand persist.
- **Regulatory corrections:** NICE NG232 → NG192 (Caesarean birth); NG229 relabelled as "Fetal monitoring in labour"; NHS URLs migrated to `/best-start-in-life/`; Miscarriage Association helpline updated to freephone 0303 003 6464; MHRA intended-purpose statement added (SED-SAF-001a); Article 9(2)(a) framing corrected.
- **Tech stack refresh:** Expo SDK 52 → 55; React Native 0.76 → 0.83; MMKV v3 → v4; RevenueCat v8 → v10; 16 KB Android page-size compliance; Xcode 26 / iOS 26 SDK requirement.
- **Accessibility:** WCAG 2.1 AA → 2.2 AA; colour palette corrected for contrast failures; SED-A11Y-001 through 011 (screen reader, dynamic type, reduced motion, voice control).
- **Inclusivity:** partner-mode rewritten to include same-sex couples, solo parents, and surrogacy intended parents; non-binary name-style filter; expanded feeding-intentions options; gender-neutral partner-content copy.
- **Pregnancy-loss pathway:** added Ectopic Pregnancy Trust, ARC (TFMR), Petals, Aching Arms, Twins Trust Bereavement, Birth Trauma Association, Mariposa/Saying Goodbye; "Hide pregnancy content" softer option; total notification suppression enumerated.
- **Operational additions:** customer support model, Terms of Service, refund/restore lifecycle, encrypted backup/restore, GDPR Article 20 data export, QA strategy with device matrix, CI/CD pipeline, release management with phased rollout and analytics-free bad-release detection, edge-case taxonomy, asset specs, trademark plan, monthly metrics review.
- **Postnatal scope:** v2.0 implied "postnatal toolkit"; v2.1 scopes v1.0 to pregnancy-through-labour and defers postnatal to v1.1 (F11). Roadmap §18 added.
- **Competitive positioning:** September 2025 Flo + Google $56M class-action settlement folded into the privacy narrative as a strategic asset.

See `PRD_v2_0_to_v2_1_CHANGELOG.md` for the full delta and `research/` for the source justifications.

### Working branch

Active development: `claude/multi-agent-prd-research-AoneB`.

# Seed Maternity Toolkit PRD v2.0 — Gap Analysis

**Verdict:** Strong on clinical safety, regulatory boundary, feature scope, and offline architecture. Materially incomplete on operational, legal, QA, release, and user-rights surfaces. ~24 categories require addition or substantial expansion before this can serve as the single source of truth for a store-submission build.

---

## Category-by-category gaps (24 categories)

### 1. Customer support strategy without telemetry

**Missing:** No support model. No support email, no contact route, no triage owner, no SLA, no purchase-verification workflow.

**Add — new Section 12.7:** SED-SUP-001 through 008 covering support@seed.health shared inbox, support web page with FAQ, triage SLAs, manual Pro verification, response templates, special handler for pregnancy-loss correspondence, in-app "Help & support" entry point.

### 2. Refund and purchase policy

**Missing:** Refund stance, restore UX, Family Sharing, cross-device reinstall, payment-success-but-sync-fail edge case.

**Add — new Section 2.5:** SED-REV-006 through 015 covering refund policy text, "Restore purchases" button on Pro screen AND Settings, iOS Family Sharing config, cross-device restore journey, payment-sync-fail recovery, RevenueCat appUserId visible in About, "I already bought Pro" CTA equal prominence.

### 3. Terms of Service

**Missing:** PRD mentions privacy policy but no ToS. Both stores effectively require one for paid apps.

**Add — new Section 7.3:** SED-LEG-001/002/003 — Terms hosted at public URL; required clauses (acceptance, age 16+, licence, medical disclaimer, no clinical relationship, Pro purchase terms, restore rights, acceptable use, IP, limitation of liability, indemnity, governing law England & Wales, changes-to-terms, contact, severability); Apple standard EULA on iOS.

### 4. Quality assurance and testing strategy

**Missing:** 12-line testing checklist. No unit-test framework, coverage targets, device matrix, clinical-content QA, accessibility audit method, performance budgets, beta programme.

**Add — new Section 12.8:** SED-QA-001 through 009 — Jest + RTL with coverage targets, JSON schema validation via ajv, device matrix (Pixel 6a, Galaxy A14, S22, iPhone SE 2, iPhone 12, iPhone 15), manual test plan template, TalkBack/VoiceOver passes, UK English audit, performance budgets, TestFlight + Play Internal Testing beta.

### 5. CI/CD beyond EAS Build

**Missing:** Only `eas-build.yml`. No lint, type-check, test gate, schema validation, bundle-size guard, security scanning, pre-commit hooks.

**Add — new Section 12.9:** SED-CI-001 through 007 — ESLint with prohibited-term rule, Prettier + husky + lint-staged, GitHub Actions `ci.yml`, npm audit, bundle-size budget, secret scanning, on-device crash diagnostics with voluntary share.

### 6. Release, versioning, rollback

**Missing:** No semver policy, no release notes template, no phased rollout, no rollback. **Critically: with no analytics, how is a bad release detected?**

**Add — new Section 11.3:** SED-REL-001 through 006 — semver MAJOR.MINOR.PATCH, Keep-a-Changelog, phased rollout 5%→20%→50%→100% on Play, iOS phased release, bad-release detection model (daily store-review scan, support inbox monitoring, RevenueCat dashboard, manual smoke test), rollback procedure with `v-stable-YYYYMMDD` tag.

### 7. Clinical Sign-off SOP

**Missing:** CLINICAL_REVIEW_LOG.md referenced but no SOP, no template, no change-control, no emergency process.

**Add — new Section 5.5:** SED-CLIN-001 through 005 — triggering events, sign-off template with sources/edition verified, post-launch correction timeline, emergency clinical correction (24h CSO sign-off + expedited Apple review).

### 8. OTA updates (Expo Updates)

**Missing:** SED-ARCH-005 bans server content but PRD never clarifies EAS Update.

**Add — SED-ARCH-013 through 015:** EAS Update **disabled for v1.0**; `expo-updates` not installed; every content/JS change goes through store review. v1.2 may reconsider on JS-bugfix-only channel.

### 9. Notification permission handling

**Missing:** No UX for permission requests on iOS / Android 13+.

**Add:** SED-F09-010 through 015 — request only on first relevant action (creating appointment with reminders); pre-permission priming screen; denied-state UX with `Linking.openSettings()`; no iOS provisional auth; Android channels created before scheduling.

### 10. Pro restore on reinstall / new phone

**Missing:** No explanation of how Pro returns without an account.

**Add:** SED-REV-012/013/014/015 — same device fresh install auto-restores from store receipt; new device same store account uses `restorePurchases()`; new device different store account = no cross-account restore (intentional); "I already bought Pro" CTA mandatory.

### 11. App icon and asset specifications

**Missing:** PRD lists files but no full spec.

**Add — new Section 12b.2:** SED-ASSET-001 through 007 — 1024×1024 icon, Android adaptive 432×432 in 1024 canvas with 264 safe zone, splash 1284×2778, Play feature graphic 1024×500 + screenshots, App Store Connect 1290×2796 6.7" minimum 3 screenshots, screenshot copy CSO-reviewed.

### 12. Localisation / i18n readiness

**Missing:** PRD assumes English-only without string externalisation.

**Add:** SED-CONTENT-002 (in Section 9.2b) + SED-I18N concepts — `data/copy.en-GB.json` locale file; `Intl.DateTimeFormat('en-GB')`; architecture permits adding `cy` and other locales without refactor.

### 13. Performance and resource budgets

**Add — under SED-QA-007:** Cold start <2.0s on Pixel 6a; warm start <500ms; MMKV read <2ms; chart render <300ms; bundle <30MB APK / <50MB IPA; memory <150MB on Galaxy A14; Labour Mode <8% battery/hour on Pixel 6a.

### 14. Error handling, empty states, edge cases

**Add — new Section 12b.3:** SED-EDGE-001 through 010:
- 001: Future LMP rejected inline
- 002: LMP >42 weeks ago — soft handling
- 003: Contraction-timer crash recovery via `isActive` flag
- 004: Zero-movement kick session saved without colour coding
- 005: Weight >10kg delta from previous — non-alarming inline note
- 006: PDF out-of-storage error → offer Share
- 007: Past-scheduled notifications refused; foreground recovery banner
- 008: MMKV corruption → preserve `.bak` + fresh start + modal
- 009: Payment success + sync fail (SED-REV-010)
- 010: Disclaimer versioning (`disclaimer_v`)

### 15. Analytics-free metric collection

**Missing:** Section 14 lists targets without measurement source.

**Add:** Section 14 table extended with "Measurement source" column — Play Console, App Store Connect, RevenueCat dashboard, manual review scan, audit script. SED-MET-001 — monthly metrics review meeting first Monday.

### 16. Data export user right (UK GDPR Article 20)

**Missing:** SED-PRIV-004 covers deletion but not export.

**Add:** SED-PRIV-010/011/012 — "Export all my data" → JSON file via OS share sheet; PDF export option; privacy policy mentions export right.

### 17. In-app contact / feedback link

**Add:** SED-SUP-008 — Settings → "Help and support" with mailto link including version/platform/appUserId pre-filled.

### 18. Tracking pause / end / delete — confirmation friction

**Missing:** Single confirmation on irreversible deletion.

**Add:** SED-CC-021 — Delete requires 3 steps: initial screen + type "DELETE" + 3-second hold. Pause/End/Hide are single-tap.

### 19. Backups (changing phones)

**Missing:** No encrypted backup/restore.

**Add — new Section 7.4:** SED-BAK-001 through 004 — encrypted JSON archive `.seedbk`, AES-256-GCM via PBKDF2 from user passphrase; restore via document picker; backup excludes RevenueCat appUserId; privacy policy disclosure.

### 20. Onboarding edge cases (mid-pregnancy, postnatal)

**Add:** SED-UX-007 — dashboard adapts to <8 wk / 8–36 / 37+ / past EDD. SED-UX-019 — "Has your baby already been born?" path: "Postnatal features not yet available."

### 21. Legal entity, publisher, trademark

**Missing:** PRD has "subject to trademark clearance" caveat but no action plan.

**Add — new Section 12b.4:** SED-LEG-004 through 008 — IPO TM search Class 9 + 44 by day 30; domain seed.health; publisher = registered legal entity; ICO registration matches publisher; Companies House confirmed.

### 22. Crisis content handling in free-text fields

**Add:** SED-CC-020 — universal "Need urgent help?" footer on every free-text input: "999... Samaritans 116 123." App does NOT attempt to detect crisis content.

### 23. Twin pathway granularity (chorionicity)

**Add:** SED-CC-022 — twin pathway in v1.0 follows NICE NG137 baseline without distinguishing MC/DC vs DC/DA; one-line note in F09 to ask consultant; granular schedules deferred to v1.1.

### 24. Future roadmap (v1.1, v1.2)

**Add — new Section 18:**
- **v1.1:** F11 postnatal module (0–6 weeks), Welsh content, system dark mode, commissioned illustration set, chorionicity-specific twin schedules.
- **v1.2:** Pregnancy-after-loss pathway, IVF integration, partner standalone variant, EAS Update reconsidered.
- **v2.0:** Optional iCloud/Drive encrypted sync, Apple Health/Google Fit read-only handoff, additional UK community languages.
- **Explicit non-roadmap:** No social, no community, no AI chat, no symptom checker, no thresholds, no ads — ever.

---

## Pre-launch checklist (gating items)

See Section 12.10 in v2.1. Includes Legal/policy, Clinical, Support, Purchases, QA, CI/CD, Edge cases, Release, Assets, Compliance categories.

---

## Top 10 critical gaps (ranked)

1. **No customer support model at all** — both stores require a working developer contact.
2. **No Terms of Service** — required for a paid app with health-adjacent content.
3. **OTA / EAS Update policy ambiguity** — must be resolved before architecture finalised.
4. **Cross-device Pro restore journey unspecified** — most common support ticket pregnancy apps receive.
5. **Bad-release detection without analytics** — broken release could sit live for weeks.
6. **Clinical sign-off SOP undefined** — CLINICAL_REVIEW_LOG.md is a filename, not a process.
7. **Data export (UK GDPR Article 20) not implemented** — legal gap.
8. **Notification permission UX unspecified** — iOS 16+ / Android 13+ reject vague flows.
9. **Backup / change-of-phone unaddressed** — single most predictable 1-star review trigger.
10. **QA strategy effectively missing** — 1-paragraph checklist insufficient for clinically-adjacent app.

---

**Summary.** PRD is unusually strong on regulatory bright line, content sourcing, technical architecture, feature definition. Weaknesses concentrated in **operational surface** (support, releases, QA, post-launch process), **legal surface** (ToS, trademark, entity), **user-rights surface** (export, backup, restore), and small architectural ambiguities (OTA policy, notification UX). All gaps remediable within the 60-day window; most require text additions rather than significant engineering. The single most consequential addition is the **Customer support and Pro-restore operating model**, intersecting refunds, GDPR rights, clinical correction, and store-review survival.

# PRD v2.0 → v2.1 Changelog

**Date:** 15 May 2026
**Source:** Multi-agent research pass (5 parallel agents covering regulatory/clinical, UK competitive market, technical stack, UX/accessibility/sensitivity, and operational/gap completeness).

This changelog summarises every material change between Seed Maternity Toolkit PRD v2.0 (Feb 2026) and v2.1 (May 2026). Where text wording is changed, before/after diffs are listed. New requirement IDs are introduced where existing IDs were not extensible.

The full agent research reports are preserved under `research/`.

---

## Critical fixes (must-do, blocking)

### 1. Hard-coded MMKV encryption key — privacy regression in v2.0

**v2.0** (Section 6.5):
```javascript
const mmkv = new MMKV({ id: 'seed-storage', encryptionKey: 'seed-enc-key' });
```

Shipping a literal key in JS means anyone who unpacks the .apk/.ipa can decrypt every user's MMKV file. Combined with SED-PRIV-001 / SED-PRIV-003 / 100% privacy-first claims, this is a regulatory and marketing problem.

**v2.1 fix:** new SED-PRI-006 / SED-PRI-007 — per-install CSPRNG key (`expo-crypto.getRandomBytesAsync(32)`) stored in iOS Keychain / Android Keystore via `expo-secure-store` (`AFTER_FIRST_UNLOCK`). Full implementation in Section 6.5.

### 2. Timer hook wrote to the wrong MMKV instance — encryption bypass in v2.0

**v2.0** (Section 6.4): `useMMKVString(...)` called without the named instance argument, silently using the default unnamed (unencrypted) MMKV. Timer timestamps were not encrypted; not wiped by `resetAll()`.

**v2.1 fix:** new SED-ARCH-016 — timer hooks MUST pass the named encrypted instance as second argument. Section 6.4 code updated.

### 3. `persist` middleware not wrapped in `createJSONStorage` — silent persistence failure

**v2.0** (Section 6.5): `persist({ storage: mmkvStorage })` where `mmkvStorage` is a raw `StateStorage`. Zustand v5 requires `createJSONStorage(() => stateStorage)` wrapping for JSON serialization.

**v2.1 fix:** Section 6.5 updated to use `createJSONStorage`.

### 4. NICE NG232 doesn't exist — should be NG192 (Caesarean birth)

v2.0 cited "NICE NG232 (Caesarean birth)" four places. Correct ID is **NG192**, published 31 March 2021, with notable updates June/July 2025. Fixed throughout v2.1 (Sections 3.7, 4, 8, Appendix B).

### 5. NICE NG229 mislabelled as "Intrapartum care"

NG229's title is **"Fetal monitoring in labour"**; it replaced the monitoring section of CG190 (the wider intrapartum guideline). Fixed in Appendix B; CG190 added as the broader intrapartum reference.

### 6. NHS week-by-week URLs migrated

v2.0 used `https://www.nhs.uk/pregnancy/week-by-week/...`; NHS migrated to **"Best Start in Life"** IA in 2024–2025. Legacy URLs unreliable.

**v2.1 fix:** all URLs updated to `https://www.nhs.uk/best-start-in-life/pregnancy/week-by-week-guide-to-pregnancy/{trimester}/week-{N}/`. SED-F02-003 mandates a build-time URL liveness check.

### 7. Miscarriage Association helpline out of date

v2.0 listed **01924 200 799**. Charity has moved to freephone **0303 003 6464**. Fixed in SED-CC-010 and Appendix B.

### 8. Colour palette fails WCAG contrast in 4 places

v2.0 §6.7:
- `primary: '#7B9E87'` sage as button label/text — 2.81:1 (fails 4.5:1)
- `textTertiary: '#9B9590'` — 2.81:1
- `secondary: '#C4A882'` tan — 2.15:1
- `bannerBorder: '#E6A817'` non-text UI on `#FFF3E0` — 1.92:1 (fails 3:1)

**v2.1 fix:** palette corrected (`primary` → `#4A6B53`, `textTertiary` → `#767670`/`#595550`, `secondary` → `#9C8050`, `bannerBorder` → `#B07A00`), with documented ratios. SED-UX-017 mandates programmatic contrast tests.

### 9. "Postnatal toolkit" claimed but no postnatal features

v2.0 §1.1 and §1.3 call Seed a "pregnancy and postnatal toolkit"; v2.0 §3 has zero postnatal features (F01–F10 are all antenatal/intrapartum). Store-listing mismatch risk.

**v2.1 fix:** §1.1 rewritten to scope v1.0 as pregnancy-to-labour only. F11 postnatal module specified in Roadmap §18 for v1.1. Risk Register R11 added.

### 10. `NSUserTrackingUsageDescription` contradicts privacy stance

v2.0 `app.json` included `NSUserTrackingUsageDescription: "This app does not track you. This permission is not used."` But if `requestTrackingAuthorization` is never called, the key is contradictory and Apple Review flags inconsistency.

**v2.1 fix:** key removed (SED-STORE-010). Plugin entries also cleaned up (`react-native-mmkv` plugin removed — no-op; `expo-secure-store` and `expo-build-properties` added).

---

## Updated technology stack (Section 6.1)

v2.0 targeted Expo SDK 52 / RN 0.76 — by May 2026 these are three majors behind.

| Package | v2.0 target | v2.1 target (May 2026) |
|---|---|---|
| React Native | 0.76+ | **0.83.x** (via Expo SDK 55) |
| Expo SDK | 52+ | **55** (SDK 56 in beta — not for launch) |
| React | (implicit 18.3) | **19.2** (required by SDK 55) |
| expo-router | v4+ | **v7** (SDK 55 bundled) |
| zustand | v5+ | **5.0.10+** |
| react-native-mmkv | v3+ | **v4.x** (Nitro Modules) |
| Persistence adapter | zustand-mmkv-storage | **Inline** (Section 6.5; dropped the third-party package — single-maintainer supply-chain risk) |
| nativewind | v4+ | **v4.2** (v5 has Tailwind 4.1 config rewrite — defer) |
| UI components | @gluestack-ui/themed v2+ | **Hand-rolled primitives + NativeWind** (Gluestack v3 is shadcn-style copy-paste; overkill) |
| react-native-purchases | latest | **v10.x** (Play Billing 8) |
| **New in v2.1** | — | `expo-secure-store`, `expo-crypto`, `expo-document-picker`, `expo-file-system`, `expo-sharing`, `expo-build-properties` |

**Platform updates:**
- Android: targetSdk 35 + **16 KB page-size compatibility** (Google Play deadline 31 May 2026)
- iOS: deployment target 16.0; **Xcode 26 / iOS 26 SDK** required for new submissions from 28 April 2026

---

## New sections in v2.1

| Section | Purpose | Triggered by |
|---|---|---|
| §5.1 SED-SAF-001a | MHRA "Crafting an intended purpose" statement | Regulatory agent |
| §5.5 Clinical sign-off SOP | SED-CLIN-001 through 005 | Gap agent |
| §6.3a EAS Update / OTA policy | SED-ARCH-013/014/015 — `expo-updates` disabled | Gap agent |
| §7.3 Terms of Service | SED-LEG-001/002/003 | Gap agent |
| §7.4 Backup and restore | SED-BAK-001 through 004 | Gap agent |
| §9.2a Screen reader / motion | SED-A11Y-001 through 011 | UX agent |
| §9.2b Reading age / locale | SED-CONTENT-001/002 | UX agent |
| §11.3 Release management | SED-REL-001 through 006 | Gap agent |
| §12.7 Support operating model | SED-SUP-001 through 008 | Gap agent |
| §12.8 QA strategy | SED-QA-001 through 009 | Gap agent |
| §12.9 Continuous integration | SED-CI-001 through 007 | Gap agent |
| §12.10 Pre-submission audit checklist | — | Gap agent |
| §12b.1 Notification permission UX | SED-F09-010 through 015 | Gap agent |
| §12b.2 Asset specifications | SED-ASSET-001 through 007 | Gap agent |
| §12b.3 Edge case taxonomy | SED-EDGE-001 through 010 | Gap agent |
| §12b.4 Legal entity / trademark | SED-LEG-004 through 008 | Gap agent |
| §18 Roadmap | v1.1 / v1.2 / v2.0 / non-roadmap | UX + Gap agents |

---

## Inclusive language and sensitivity edits

| Location | v2.0 | v2.1 |
|---|---|---|
| §1.5 (target users) | "Pregnant women in the UK..." | "People who are pregnant in the UK... The majority are women; the app uses 'pregnant person' or 'you' throughout..." |
| §1.5 | "First-time mothers..." | "First-time parents (most often mothers)..." |
| §3.1 SED-F01-009 (new) | — | Onboarding wording "When is your pregnancy due?" with third option "I'd rather not enter this now" |
| §3.7 SED-F07-001 step 8 | "Feeding intentions (breast, bottle, combination, undecided)" | Expanded: breastfeeding/chestfeeding, expressing, formula/bottle, combination, donor milk, tube feeding, undecided |
| §3.7 SED-F07-013/014 (new) | — | Feeding-options expansion; special-considerations placeholder examples |
| §3.10 SED-F10-002 | "gender (boy/girl/unisex)" filter | "name style: traditionally given to boys / girls / used for any gender" — internal type renamed to `nameStyle` |
| §4.1 SED-CC-001 | Binary "pregnant person / partner or birth companion" | Four options: pregnant / partner-companion / surrogacy-intended-parent / shared-device, plus independent partner-content toggle for solo parents |
| §4.1 SED-CC-004 | (no inclusive-language detail) | Gender-neutral content required; no "dad/father/husband/wife" anywhere |
| §4.1 SED-CC-004a (new) | — | "How would you like to be addressed?" optional field |
| §4.3 SED-CC-010 | 3 charities (Miscarriage Association, Tommy's, Sands) | Added: Ectopic Pregnancy Trust, ARC (TFMR), Petals, Aching Arms, Twins Trust Bereavement, Birth Trauma Association, Child Bereavement UK, Mariposa/Saying Goodbye. Plus "Hide pregnancy content" softer option (SED-CC-019). Miscarriage Association number corrected. |
| §4.3 SED-CC-018/019/020/021 (new) | — | Total notification suppression while paused; soft "Hide" option; universal crisis footer on free-text screens; three-step delete confirmation |
| §5.3 SED-SAF-007 disclaimer | "Using this app does not create a clinical relationship between you and the developers." | "Using this app doesn't make us your healthcare team." (Plain English, grade 6 reading age per SED-CONTENT-001.) |
| §5.3 SED-SAF-007a/b (new) | — | Disclaimer versioning; screen-reader accommodation |
| §9.1 Onboarding screen 3 | "When is your baby due?" | "When is your pregnancy due?" with third option |
| §F05-008 (new) | — | "Hide the chart" option for eating-disorder sensitivity |
| §F04-013 (new) | — | Daily pattern view: no comparative or interpretive visual cues |

---

## Prohibited-terms list expanded (SED-SAF-005)

v2.0 listed: safe, unsafe, normal, abnormal, diagnosis, diagnose, treatment, treat, prescribe, recommend, risk, you should.

**v2.1 adds**: monitor, monitoring, detect, detection, predict, prediction, screen, screening, assess, assessment, healthy, warning sign, red flag, reassure, reassuring, accurate, medically accurate. Plus an exception-list mechanism for legitimate occurrences (e.g. "screening" in NHS dating-scan brief), with each exception CSO-signed-off.

---

## Article 9(2)(a) consent framing corrected (SED-PRIV-007)

v2.0 simultaneously claimed (a) no controller relationship exists for on-device health data (§7.1) and (b) the disclaimer satisfies Article 9(2)(a) explicit consent (§7.2) — these positions are in tension. ICO and EDPB guidance is hostile to bundling Article 9 consent into a general modal.

**v2.1:** the cleaner position — no Article 9 processing occurs at the controller level for on-device data. Disclaimer is a safety acknowledgement only. If any future feature transmits health data off-device, a separate granular Article 9(2)(a) consent screen is mandated.

---

## Pricing and revenue projections updated (§2)

| | v2.0 | v2.1 |
|---|---|---|
| Net per sale | £4.24 | **£4.16** (commission **and** VAT — both stores remit VAT on developer's behalf and report net) |
| Pessimistic scenario | 5,000 / 250 conversions / £1,060 | 3,000 / 60 / £250 |
| New "Realistic-conservative" tier | — | 10,000 / 3% / 300 / £1,250 |
| Realistic → "Target" | 15,000 / 750 / £3,180 | 15,000 / 750 / £3,120 (reframed as **target**; median freemium conversion is 2.18%, not 5%) |
| Optimistic | 40,000 / 2,000 / £8,480 | 30,000 / 1,500 / £6,240 |
| Strong | 80,000 / 4,000 / £16,960 | 80,000 / 4,000 / £16,640 |
| UK births context | "approximately 650,000" | "approximately 655,000 in 2024 (ONS + NRS + NISRA)" |

Plus SED-REV-001a — mandates enrolment in Apple Small Business Program and Google Play 15% small-business tier.

---

## New SED-* requirement IDs (v2.1)

Total new IDs: ~80. Highlights:

- **SED-SAF-001a, 007a, 007b** — intended purpose statement; disclaimer versioning; screen-reader accommodation
- **SED-CLIN-001 to 005** — clinical sign-off SOP
- **SED-PRI-006, 007** — encryption key in OS keystore; bootstrap
- **SED-PRIV-010 to 012** — GDPR Article 20 data export
- **SED-LEG-001 to 008** — Terms of Service, trademark, legal entity
- **SED-BAK-001 to 004** — encrypted backup/restore
- **SED-UX-007 to 019** — onboarding edge cases, WCAG 2.2 SC coverage, postnatal disclosure, system dark mode
- **SED-A11Y-001 to 011** — screen reader, dynamic type, reduced motion, voice control
- **SED-CONTENT-001, 002** — reading age, UK English locale file
- **SED-CC-018 to 022** — notification suppression, hide option, crisis footer, delete friction, twin chorionicity note
- **SED-F01-009, F05-008/009/010, F04-013, F07-009 to 015, F10-008** — feature-level UX and sensitivity additions
- **SED-F09-010 to 015** — notification permission UX
- **SED-REL-001 to 006** — release management
- **SED-SUP-001 to 008** — support operating model
- **SED-QA-001 to 009** — QA strategy
- **SED-CI-001 to 007** — CI/CD
- **SED-ASSET-001 to 007** — asset specs
- **SED-EDGE-001 to 010** — edge cases
- **SED-REV-001a, 006 to 015** — pricing rationale fix; refund/restore lifecycle
- **SED-ARCH-013 to 017** — OTA policy; encryption-key bootstrap; timer-hook fixes
- **SED-STORE-009, 010** — Apple regulated-medical-device declaration; remove tracking-permission key
- **SED-MET-001** — monthly metrics review

---

## Risk register additions

R11 through R20 added — postnatal mismatch, accessibility audit, encryption key regression (now fixed), timer hook bug (now fixed), OTA ambiguity (now resolved), cross-device Pro restore, trademark, ICO Children's Code refresh, source URL drift, competitor pivot.

---

## What did NOT change

- Core architecture: 100% offline, MMKV-encrypted, no analytics, no remote config, no accounts.
- Regulatory bright line: notepad-not-device principle.
- Feature scope F01–F10 — features are unchanged in scope; many are extended in inclusivity, sensitivity, and accessibility.
- £4.99 price point (one-time).
- Partner mode, twin mode, pregnancy-loss pathway as cross-cutting differentiators.
- Free vs Pro tier split.
- 60-day build phase structure.
- Document conventions (MUST/SHOULD/MAY; SED-* IDs).

---

*v2.1 was produced through a multi-agent research process: five specialised research agents executed in parallel against the v2.0 PRD, producing detailed reports preserved under `research/`. Findings were then synthesised into this PRD revision. The agent reports are recommended reading for anyone needing the source justifications behind v2.1 changes.*

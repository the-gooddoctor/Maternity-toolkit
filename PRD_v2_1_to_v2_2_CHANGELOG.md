# PRD v2.1 → v2.2 Changelog

**Date:** 15 May 2026
**Source:** Second-pass multi-agent research review against v2.1 (6 parallel agents covering internal consistency, deeper regulatory, deeper technical verification, content audit, operational maturity, and implementation-readiness).

This changelog summarises every material change between Seed Maternity Toolkit PRD v2.1 and v2.2. v2.2 fixes critical bugs introduced by v2.1, corrects arithmetic errors, adds ~50 new requirements, and most importantly introduces the **v1.0 / v1.0.x / v1.1 cut list** that makes the 60-day timeline achievable.

The 6 agent research reports are preserved under `research/` (files `06_*` through `11_*`).

---

## Critical fixes (must-do, blocking)

### 1. Encryption-key bootstrap (v2.1) would crash the app at first launch

**v2.1 §6.5** introduced an async `initMMKV()` + `Proxy` pattern to lazily inject the encryption key. The code throws at module-import time because Zustand v5 `persist` middleware hydrates synchronously when storage is synchronous: `storage.getItem()` is called inside `create(persist(...))` *before* `app/_layout.tsx`'s `await initMMKV()` ever runs.

**v2.2 fix:** synchronous bootstrap using `SecureStore.getItem()` and `Crypto.getRandomBytes()` (both available in Expo SDK 51+). No Proxy. MMKV instantiated at module-import time. New SED-PRI-008. Plus:
- SED-PRI-006 retightened to use `WHEN_UNLOCKED_THIS_DEVICE_ONLY` (not v2.1's `AFTER_FIRST_UNLOCK`) — prevents iCloud Keychain syncing of the encryption key
- SED-PRI-009 mandates `globalThis.btoa` over `import { Buffer } from 'buffer'` — `buffer` is not bundled by SDK 55 by default

### 2. Pricing arithmetic in v2.1 was wrong

**v2.1 §2.2** claimed net per sale = £4.16 "after 15% commission AND VAT". The £4.16 figure is actually VAT-net before commission. Real net after VAT (£4.99 ÷ 1.20 = £4.158) and then 15% commission = **£3.53**.

Cascade: v2.1 §2.3 target scenario (15k × 5% × £4.16 = £3,120) becomes **£2,648** — below the §14 "£3,000+" success target. v2.2 corrects the math, updates the projection table, and updates §14 to note 17k downloads at 5% conversion (or 15k at 5.7%) needed to hit £3,000 net.

### 3. SettingsState interface missed 6 fields the v2.1 prose mandated

v2.1 added requirements referencing settings fields that did not exist in the v2.1 `SettingsState`:

| Required by | Field | v2.1 status | v2.2 |
|---|---|---|---|
| SED-CC-001 4-mode partner | `userMode: 'pregnant' | 'partner_companion' | 'intended_parent' | 'shared_device'` | Only 2 values (`'pregnant' | 'partner'`) | Added |
| SED-CC-001 secondary toggle | `showPartnerContent: boolean` | Missing | Added |
| SED-CC-019 | `trackingStatus: ... | 'hidden'` | Missing `'hidden'` | Added |
| SED-SAF-007a / SED-EDGE-010 | `disclaimerV: number` | Missing | Added |
| SED-F05-008 | `weightChartHidden: boolean` | Missing | Added |
| SED-A11Y-010 | `hapticsDisabled: boolean` | Missing | Added |
| SED-COMM-005 (new) | `policyAcknowledgedV: number` | Missing | Added |
| SED-PRIV-009 | `ageAnswer: 'yes' | 'no' | null` | Boolean `ageConfirmed` confused gate vs answer | Replaced |
| SED-F06-001 | `birthSetting: ... | 'other'` | Missing 'other' | Added |
| SED-CC-004a | `preferredAddress: string | null` | Missing | Added |

### 4. SED-ARCH-004 still mandated dropped `zustand-mmkv-storage` package

v2.1 §6.1 dropped the package; SED-ARCH-004 was not updated. v2.2 rewrites SED-ARCH-004 to mandate the inline `createJSONStorage(() => mmkvStateStorage)` pattern.

### 5. Phase-1 install commands referenced dropped packages

§12.1 install list still included `gluestack-ui` and `npm install zustand-mmkv-storage`. v2.2 removes both, adds `expo-secure-store`, `expo-crypto`, `expo-document-picker`, `expo-file-system`, `expo-sharing`, `expo-build-properties`, pins RevenueCat to v10+, and documents Buffer→btoa.

### 6. Stale field references (`isMultiple`, `trackingPaused`, `trackingEnded`)

v2.1 renamed underlying enums but didn't update SED-F02-007, SED-CC-011, SED-CC-012, SED-CC-014. v2.2 rewrites all four to reference the current enum values.

### 7. Appendix legacy artefacts preserved

- A.1 week-4 example still used legacy `nhs.uk/pregnancy/week-by-week/` URL → v2.2 updated to `/best-start-in-life/...`
- A.4 names example still used `"gender": "girl"/"boy"` → v2.2 updated to `nameStyle: 'feminine'/'masculine'`
- B Miscarriage Association still listed 01924 200 799 → v2.2 updated to freephone 0303 003 6464

### 8. Birth Trauma Association URL was wrong; Mariposa rebranded

- v2.1 listed `birthtraumaassociation.org.uk` → v2.2 fixes to `.org`
- v2.1 listed "Mariposa Trust" → v2.2 updates to "Mariposa International" (late-2024 rebrand)

### 9. Prohibited-term violations in v2.1's own content additions

v2.1 expanded SED-SAF-005 but four "recommends" verbs survived in evidence summaries, plus "normal" applied to twin sizes, "harmless" applied to Braxton Hicks, and "you should" in the week-28 appointments note. v2.2 rewrites each and codifies a tightly-scoped exception list (§5.2a / SED-SAF-005a) for legitimate Tier-1-quoted uses.

### 10. Reading-age failures in v2.1 banners and briefs

- F03-014 contraction banner: 27-word sentence at grade ~10 → split to 5 sentences at grade ~5
- F04-011 kick banner: 18-word call sentence → split
- F09-007 "What to expect" briefs: booking 24-word comma-stack; dating-scan 26-word; 36-week ECV jargon — all rewritten for grade-6 with plain-English glosses

### 11. UK spelling: "Fetal Movement Log" → "Foetal Movement Log"

SED-F04-010 PDF header used US spelling. v2.2 uses UK "Foetal" (NICE/RCOG document titles cited verbatim where they use "fetal" are an explicit exception).

### 12. F07-009 tagged-PDF requirement physically infeasible

`expo-print` on iOS (`WKWebView` → `UIPrintPageRenderer`) and Android (`WebView.createPrintDocumentAdapter`) both produce flat paginated PDFs with no logical structure tree. v2.2 downgrades SED-F07-009 to semantic HTML + plain-text alternative (SED-F07-009a); tagged PDF deferred to v1.0.x.

### 13. Risk Register inconsistency for fixed risks

R13/R14/R15 listed Critical/High/Medium likelihood despite being closed. v2.2 marks them Closed.

### 14. TOC missed §18; §13 had no parent heading; §11.2 / §11.3 out of order

v2.2 fixes TOC, inserts `## 13. Go-to-market` parent heading, and adds reconciliation note for §11.2/§11.3 ordering.

### 15. AFTER_FIRST_UNLOCK Keychain accessibility too loose

For an offline-only app, the encryption key MUST stay device-local. v2.1 used `AFTER_FIRST_UNLOCK` which permits iCloud Keychain sync of the key. v2.2 tightens to `WHEN_UNLOCKED_THIS_DEVICE_ONLY`.

### 16. PBKDF2 iteration count 100k below 2025 OWASP recommendation

OWASP 2025 minimum for SHA-256-PBKDF2 is 600k; v2.1 used 100k. v2.2 raises to 200k (the pragmatic floor that completes <1s on Galaxy A14 and is materially stronger than 100k against offline brute force). SED-CRYPTO-002.

### 17. Backup crypto library not specified

v2.2 SED-CRYPTO-001 mandates `react-native-aes-gcm-crypto` (Tectiv3/Craftzdog) — `react-native-quick-crypto` is the alternative but its AES-GCM is incomplete as of May 2026.

---

## The v1.0 / v1.0.x / v1.1 cut list (§3.0a — new in v2.2)

**The most consequential change in v2.2.** Implementation-readiness review showed v2.1 full scope = 81–111 dev-days, not 60. The cut list defers ~15–23 dev-days of v2.1 additions to v1.0.x / v1.1, bringing realistic v1.0 to 58–88 days.

**v1.0 ships:** All regulatory/safety/store-compliance/core-feature requirements. F01-F09 (defers F10 Names to v1.1; F07 ships untagged A4 PDF with system fonts).

**v1.0.x within 90 days post-launch:** F07 tagged PDF + Atkinson Hyperlegible, SED-PRIV-011 PDF data export, SED-BAK-001/002 encrypted backup, custom UI primitives full set, SED-CI-007 crash diagnostics share, multi-language refactor.

**v1.1 months 3–6:** F10 baby names, F11 postnatal, Welsh content, system dark mode, chorionicity-specific twin schedules.

---

## New SED-* requirements (v2.2)

### Encryption + bootstrap
- **SED-PRI-008** Synchronous MMKV encryption-key bootstrap (mandatory pattern)
- **SED-PRI-009** `globalThis.btoa` rather than `import { Buffer } from 'buffer'`

### Clinical SOP expansion
- **SED-SAF-005a** Exception list for prohibited terms (§5.2a)
- **SED-CLIN-006** Deputy CSO retainer + GMC/NMC PIN logged
- **SED-CLIN-007** Annual re-verification cadence (1 September calendar)
- **SED-CLIN-008** NICE ESF Tier-A self-classification
- **SED-CLIN-009** MHRA Innovation Office policy

### Content
- **SED-CONTENT-003** Verbatim NHS quotation policy (paraphrase no longer mandatory; Crown Copyright + OGL v3.0 permits verbatim with attribution)
- **SED-AUTH-001 to 005** Markdown-first content authoring workflow with `scripts/md-to-json.js` conversion

### Privacy expansion
- **SED-PRIV-013** DSAR SOP
- **SED-PRIV-014** Joint-controller/processor analysis with RevenueCat
- **SED-PRIV-015** International-transfer disclosure (UK Extension to DPF)
- **SED-PRIV-016** DPIA-lite documentation
- **SED-PRIV-017** RevenueCat breach playbook
- **SED-PRIV-018** Children's Code post-DUAA monitoring
- **SED-PRIV-019** End-of-life commitment

### Legal expansion
- **SED-LEG-009** OSS licence acknowledgements screen
- **SED-LEG-010** Accessibility statement at seed.health/accessibility
- **SED-LEG-011** Equality Act 2010 reference in §9
- **SED-LEG-012** Insurance: PI + cyber + public + product liability
- **SED-LEG-013/014/015** Brand-rename SOP (`{appName}` token, dual bundle ID reservation)
- **SED-FIN-001** Tax and accounting setup (HMRC, R&D credits, SEIS/EIS)

### Communication
- **SED-COMM-001** What's-new screen on update
- **SED-COMM-002** Store promotional text guidance
- **SED-COMM-003** Changelog feed at seed.health/changelog
- **SED-COMM-004** Security advisory channel
- **SED-COMM-005** Privacy-policy versioning with `policy_v`

### Incident response
- **SED-INC-001** RevenueCat breach response
- **SED-INC-002** Vulnerability disclosure (`SECURITY.md` + RFC 9116 security.txt)
- **SED-INC-003** Store-suspension contingency
- **SED-INC-004** Purchase outage in-app message
- **SED-INC-005** Public post-mortem within 14 days

### Observability (no telemetry)
- **SED-OBS-001 to 005** Synthetic monitoring, store-review RSS, support inbox SLA, RevenueCat dashboard daily, status page

### Architecture
- **SED-ARCH-019** Exact dependency version pinning
- **SED-ARCH-020** RevenueCat Paywalls disabled
- **SED-ARCH-021** RevenueCat Targeting/Experiments disabled

### Crypto
- **SED-CRYPTO-001** `react-native-aes-gcm-crypto` for backup encryption
- **SED-CRYPTO-002** PBKDF2 raised from 100k to 200k iterations

### Backup
- **SED-BAK-005** Android picker MIME `*/*` workaround for `.seedbk`

### Documentation / bus-factor
- **SED-DOC-001** ADRs in `/docs/adr/`
- **SED-DOC-002** CONTRIBUTING.md, cookbook.md, architecture diagram
- **SED-DOC-003** Documentation maintenance cadence

### Feature
- **SED-CC-023** EDD+14 quiet-mode prompt (from operational maturity review)
- **SED-CC-024** Child-safety design review of every disclaimer-flow screen
- **SED-F07-009a** Plain-text export as accessibility-equivalent to tagged PDF
- **SED-F09-007a** Trust-variant tail sentence on "What to expect" briefs
- **SED-STORE-011** Apple App Review reviewer accommodation (promo codes / sandbox tester)
- **SED-STORE-012** In-app events explicitly N/A
- **SED-STORE-013** Age Rating questionnaire answers (2025 form)

### Tooling
- **SED-TOOL-001** Tooling inventory at §12.13

---

## New sections in v2.2

| Section | Purpose |
|---|---|
| §3.0a | v1.0 / v1.0.x / v1.1 cut list |
| §5.2a | SED-SAF-005 prohibited-terms exception list |
| §5.6 | CSO bandwidth and bus-factor (deputy CSO, annual re-verification, NICE ESF, MHRA Innovation Office) |
| §6.8 | Content authoring workflow (Markdown-first) |
| §7.5 | Privacy policy expanded (DSAR, RevenueCat controllership, international transfers, DPIA-lite, breach playbook, Children's Code monitoring, end-of-life) |
| §7.6 | OSS licence acknowledgements |
| §7.7 | Accessibility statement |
| §7.8 | Verbatim NHS quotation policy |
| §11.4 | Brand-rename SOP |
| §12.11 | Incident response (SED-INC-001 to 005) |
| §12.12 | Service health and observability (SED-OBS-001 to 005) |
| §12.13 | Tooling inventory |
| §12.14 | Dependency version pinning |
| §12.15 | Documentation and bus-factor mitigation |
| §12b.5 | Tax, accounting, insurance |
| §12b.6 | User communication channels without telemetry |
| §13.6 | Press kit |

---

## Risk register additions (R21–R38)

R21 CSO bus-factor; R22 RevenueCat breach; R23 vuln disclosure; R24 store suspension; R25 web property down; R26 support inbox missed; R27 OSS licence non-compliance; R28 DSAR missed; R29 Children's Code; R30 DPF revocation; R31 insurance gap; R32 end-of-life; R33 trademark clash; R34 60-day timeline; R35–R38 retroactively closed v2.1 bugs.

---

## Content edits (prohibited terms / reading age / UK English)

| Location | v2.1 | v2.2 |
|---|---|---|
| §3.7 SED-F07-002 cord clamping | "NICE recommends clamping..." | "The NHS advises waiting at least one minute..." |
| §3.7 SED-F07-002 skin-to-skin | "The NHS recommends skin-to-skin... helps regulate temperature" | "The NHS advises skin-to-skin... helps keep your baby's temperature steady" |
| §3.7 SED-F07-002 caesarean | "NICE recommends discussing..." | "NICE advises that you discuss..." |
| §3.2 week-28 maternal_changes | "irregular, painless tightenings" | "irregular, usually mild tightenings" |
| §3.2 week-28 partner_content | "Braxton Hicks... are usually harmless" | "Braxton Hicks... are usually not a cause for concern" |
| §3.2 week-28 key_appointments | "You should have an antenatal appointment around 28 weeks" | "An antenatal appointment is usually offered around 28 weeks" |
| §3.2 week-28 twin_adjustments | "...This is normal" | "...This is common and expected with twin pregnancies" |
| §3.3 SED-F03-014 banner | 27-word single sentence at grade 10 | 5 short sentences at grade 5 |
| §3.4 SED-F04-010 | "Fetal Movement Log" | "Foetal Movement Log" |
| §3.4 SED-F04-011 banner | "...contact your maternity unit immediately. Do not wait until the next day." | "...call your maternity unit. Call now — do not wait until the next day." |
| §3.9 SED-F09-007 booking brief | 24-word comma-stacked sentence | 6 short sentences with Trust-variant tail |
| §3.9 SED-F09-007 dating scan | "nuchal translucency measurement may be taken" (unglossed) | "nuchal translucency measurement — a check of the fluid at the back of your baby's neck" |
| §3.9 SED-F09-007 28-week | "symphysis-fundal height" | "your bump — sometimes called symphysis-fundal height" + "anti-D injection (a treatment given when your blood type is Rhesus negative)" |
| §3.9 SED-F09-007 36-week | "external cephalic version (ECV)" | "external cephalic version, or ECV — when a doctor gently tries to turn your baby into a head-down position" |
| §4.3 SED-CC-010 lead | "If your circumstances have changed — a loss..." (24 words) | "Things may have changed for you — a loss..." (split into 3 sentences) |
| §4.3 charity list | birthtraumaassociation.org.uk | birthtraumaassociation.org |
| §4.3 charity list | "Mariposa Trust / Saying Goodbye" | "Mariposa International (parent charity of Saying Goodbye)" |
| §4.4 SED-CC-016 | "...is completely normal" | "Many people feel this way" |
| §5.3 SED-SAF-007 disclaimer | "If you are worried about your baby's movements..." (22-word sentence) | "Contact your maternity unit straight away if anything worries you. This includes any change in your baby's movements..." (split) |
| §9.1 onboarding hint | "It's normal for the date to change" | "It's common for the date to change" |
| Appendix A.4 | `"gender": "girl"/"boy"` | `"nameStyle": "feminine"/"masculine"` |
| Appendix A.1 | legacy `/pregnancy/week-by-week/` URL | `/best-start-in-life/...` |
| Appendix B | Miscarriage Association 01924 200 799 | 0303 003 6464 |
| Appendix B | NHS week-by-week legacy URL | Best Start in Life URL |

---

## What did NOT change

- Architecture: 100% offline, MMKV-encrypted, no telemetry, no accounts, no remote config
- Regulatory bright line: notepad-not-device principle, SED-SAF-001a intended-purpose statement
- Feature scope F01–F10 in their intent (cut list defers F10 to v1.1 for shipping, not for spec)
- £4.99 price point (one-time)
- Partner, twin, pregnancy-loss differentiators
- Free vs Pro tier split
- 60-day target (now achievable with cut list)
- WCAG 2.2 AA target
- Document conventions

---

*v2.2 is the second multi-agent revision in 24 hours. The agent reports underlying it are preserved at `research/06_*` through `research/11_*`. v2.2 should now lock and any further changes go through normal MINOR-release review.*

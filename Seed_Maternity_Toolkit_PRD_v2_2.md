# Seed: Maternity Toolkit

## Product Requirements Document and Technical Specification

**Version 2.2 | May 2026**

**Prepared for:** Jonathan Watchorn (CSO) and Christian Baverstock (CTO)

**Confidential**

*Working title: "Seed". Final brand name subject to trademark clearance (see Section 13.4). The product is developed by the same entity as the company's clinical-grade maternity information agent, and shares its commitment to clinical quality, privacy, and UK-focused content. Seed is a distinct, standalone product.*

*Revision history: v2.0 (Feb 2026) → v2.1 (May 2026) → v2.2 (May 2026). v2.2 is the output of a second-pass multi-agent research review against v2.1 (internal consistency, deeper regulatory, deeper technical verification, content audit against the new SED-CONTENT-001 / SED-SAF-005 standards, operational maturity, and implementation-readiness). v2.2 fixes one **boot-crashing code defect** in v2.1's encryption-key bootstrap, corrects the £4.16 net-revenue arithmetic (it was VAT-net only, not VAT-and-commission as v2.1 claimed; corrected to ≈£3.53), adds the v1.0/v1.0.x/v1.1 cut list that makes the 60-day timeline achievable, adds ~50 new requirements covering vulnerability disclosure, incident response, DSAR SOP, deputy CSO, insurance, and OSS-licence acknowledgements, and reconciles the SettingsState interface with the v2.1 prose. See `PRD_v2_1_to_v2_2_CHANGELOG.md` for the full delta.*

---

## Document conventions

| Convention | Meaning |
|---|---|
| **MUST / MUST NOT** | Non-negotiable requirement |
| **SHOULD / SHOULD NOT** | Strong recommendation; deviation requires documented justification |
| **MAY** | Optional; include if time permits within 60-day timeline |
| Requirement IDs (e.g. SED-SAF-001) | Stable identifiers. Never reuse or renumber. Reference by ID, not section number. |
| [Jon] | Clinical Safety Officer ownership |
| [Christian] | CTO / developer ownership |
| `code blocks` | Exact implementation patterns for Claude Code |

---

## Table of contents

1. Executive summary
2. Revenue model
3. Feature specification (F01–F10) — including 3.0a v1.0 / v1.0.x / v1.1 cut list
4. Cross-cutting enhancements
5. Safety, clinical standards, and regulatory position — including 5.5 Clinical sign-off SOP and 5.6 CSO bandwidth
6. Technical specification — including 6.3a OTA policy and 6.8 Content authoring workflow
7. Privacy and data protection — including 7.3 Terms of Service, 7.4 Backup and restore, 7.5 Privacy policy expansion (controller/processor/transfers), 7.6 OSS licence acknowledgements, 7.7 Accessibility statement
8. Content strategy
9. User experience — including 9.2a Screen reader / motion and 9.2b Reading age / locale
10. App store compliance
11. Build plan and phased delivery — including 11.3 Release management and 11.4 Brand-rename SOP
12. Claude Code implementation guide — including 12.7 Support, 12.8 QA, 12.9 CI, 12.10 Pre-submission checklist, 12.11 Incident response, 12.12 Service health and observability, 12.13 Tooling inventory, 12b Notification UX / Assets / Edge cases / Trademark / Tax / User-comms
13. Go-to-market — including 13.4 Baby Buddy reference and 13.6 Press kit
14. Success metrics
15. Risk register (R01–R32)
16. Appendix A: Static data schemas — including A.5 NHS appointment pathway and A.6 copy.en-GB.json structure
17. Appendix B: Key external references
18. Roadmap (v1.1 / v1.2 / v2.0 and explicit non-roadmap)

---

## 1. Executive summary

### 1.1 Purpose

Seed is a privacy-first, offline-only pregnancy toolkit for UK users. It provides high-utility tracking tools (contraction timer, kick counter, weight tracker), NHS-aligned weekly developmental information, and practical checklists, with zero data collection, zero ads, and zero subscriptions.

**Scope note for v1.0:** v1.0 covers pregnancy through to labour. A postnatal module (F11) is planned for v1.1 and is explicitly out of scope here (see Section 18 Roadmap). Marketing and store copy MUST NOT claim postnatal functionality in v1.0. v2.0 of this PRD previously implied "and postnatal" — that wording has been corrected throughout v2.1.

### 1.2 Strategic context

Seed serves two strategic purposes:

1. **Revenue generation.** Generate early income (target: £3,000-£10,000 in year one) to offset development costs for the company's clinical products, particularly API fees and illustration assets.
2. **Brand credibility.** Establish the company's presence in the UK maternity app market, building trust and name recognition with expectant parents and NHS-aligned audiences.

### 1.3 What Seed is

Seed is a tracking and organisational utility that references NHS public health guidance where appropriate. It records data the user enters and displays it back to them. It provides static, publicly available NHS and NICE information to help users engage with their maternity care.

### 1.4 What Seed is not (via negativa)

Seed is explicitly **not**:

- A medical device under UK MDR 2002 or EU MDR 2017/745
- A diagnostic tool or symptom checker
- A tool that interprets, analyses, or makes clinical judgements about user-entered data
- A replacement for midwife, GP, or obstetric care
- A community forum or social platform
- A content platform with articles, videos, or editorial opinion
- An AI chatbot or conversational agent
- A tool that sets thresholds, triggers clinical alerts, or tells users when to seek care based on their data

### 1.5 Target users

**Primary:** People who are pregnant in the UK, aged 16–45, seeking practical tracking tools with NHS-aligned terminology and pathways. The majority are women; the app uses "pregnant person" or "you" throughout to be inclusive of trans men, non-binary and intersex users. First-time parents (most often mothers) are the highest-value segment (highest information anxiety, highest engagement).

**Secondary:** Partners, co-parents and birth companions who want to understand and support the pregnancy. Seed includes a dedicated partner mode (see Section 4.1), with inclusive options for same-sex couples, solo parents by choice, and intended parents in surrogacy arrangements.

**Tertiary:** People expecting twins or multiples, who are chronically underserved by existing apps (see Section 4.2).

### 1.6 Competitive positioning

The pregnancy app market is saturated with free, ad-supported apps and freemium subscription products. Seed does not compete on features (everything it offers is available free somewhere). It competes on **experience**:

| Competitor weakness | Seed's response |
|---|---|
| Aggressive ads and paywall nagging | Zero ads. Single, transparent one-time Pro unlock. |
| Privacy violations and data brokerage | 100% offline. No accounts. No analytics. No data leaves the device. |
| US-centric content and terminology | UK-only. NHS pathways, midwife-first language, stones and pounds. |
| Insensitive pregnancy loss handling | Compassionate "pause or end tracking" pathway with charity signposting. |
| Feature bloat and social media clutter | Focused toolkit. Ten features, each done well. No forums, no feeds. |
| Partner as afterthought | Integrated partner mode with weekly companion guidance. |
| No twin/multiple support | Twin pathway with adjusted content and appointment schedule. |

---

## 2. Revenue model

### 2.1 Model: freemium with one-time Pro unlock

**Free tier (drives downloads, reviews, and organic visibility):**

- F01: Due date calculator and countdown (with term window display)
- F02: Week-by-week developmental tracker (with "Questions for your midwife" and partner content)
- F06: Hospital bag checklist (personalised by birth setting)
- F08: Pregnancy to-do checklist (with employer/practical UK tasks)
- F10: Baby name favourites (with ONS popularity trends)

**Pro tier (£4.99 one-time purchase, unlocks permanently):**

- F03: Contraction timer with Labour Mode (dark theme, session history, call button)
- F04: Kick counter with daily pattern view and midwife export
- F05: Weight tracker with chart and NICE reference band
- F07: Birth plan builder with evidence context and PDF export
- F09: Appointment reminders with NHS pathway pre-population and "What to expect" briefs

### 2.2 Pricing rationale

**SED-REV-001 (v2.2 — pricing arithmetic corrected):** Price: £4.99 one-time at UK store tier 5. This signals quality, compensates for volume constraints of a paid tier competing against free alternatives, and is substantially cheaper than subscription competitors (Pregnancy+ Premium ~£3.99/month, Flo Premium ~£39.99–£49.99/year depending on region and cohort, Peanut+ ~£7.99/month, Glow ~£7.99/month).

**Correct net-revenue math (v2.2 fix to v2.1 error):**
1. £4.99 inclusive of UK 20% VAT → VAT-exclusive = £4.99 ÷ 1.20 = **£4.158**
2. Apple/Google Small Business commission (15% on the VAT-exclusive price) = £4.158 × 0.15 = £0.624
3. Net to developer = £4.158 − £0.624 = **≈£3.53**

v2.0's £4.24 figure omitted VAT. v2.1's £4.16 figure removed VAT but then applied the 15% commission only conceptually, not arithmetically — it is in fact the *VAT-net before commission* number, not the developer's net receipt. The corrected developer net of **£3.53** flows through to §2.3 and §14.

**SED-REV-001a:** The company MUST enrol in the Apple Small Business Program AND the Google Play 15% small-business tier before submitting v1.0. Failure to enrol increases commission to 30% (net per sale drops to **≈£2.91**), materially affecting projections in §2.3.

### 2.3 Revenue projections (UK market, year one)

Net revenue per sale: **≈£3.53** (after 20% VAT remittance and 15% small-business commission — see corrected math in SED-REV-001). The realistic conversion case has been split from "target" because the median freemium conversion in this category is ~2.18% (Business of Apps / Mirava 2025); 5% is an aggressive, evidence-supported target for high-intent verticals but is not the median.

| Scenario | Free downloads | Pro conversion | Pro purchases | Net annual revenue @ £3.53 |
|---|---|---|---|---|
| Pessimistic | 3,000 | 2% | 60 | £212 |
| Realistic-conservative | 10,000 | 3% | 300 | £1,059 |
| Target | 15,000 | 5% | 750 | £2,648 |
| Optimistic | 30,000 | 5% | 1,500 | £5,295 |
| Strong performer | 80,000 | 5% | 4,000 | £14,120 |

**v2.2 note on success-metric implications:** the v2.1 §14 net-revenue target was £3,000+. Under the corrected math, the "Target" scenario (15,000 downloads × 5% conversion) returns **£2,648 — below the £3,000 success target**. To hit £3,000 net at a 5% conversion rate requires **~17,000 downloads** (5% × 17,000 = 850 purchases × £3.53 = £3,001). The §14 success-metric row has been updated accordingly. The strategic implication: organic GTM must deliver ≥17k downloads in year one, OR conversion must exceed 5%, OR price must rise. All three are individually achievable but the original "15k @ 5%" framing was 12% short.

Context: approximately **655,000 UK births in 2024** (ONS Births in England and Wales 2024 = 594,677, plus ~45,000 NRS Scotland and 19,416 NISRA Northern Ireland — the first increase in England & Wales since 2021). Over 50% of pregnant people download at least one pregnancy app; the average is 3 apps per pregnancy (PMC systematic review; Oxford 2024 tracking-app study).

### 2.4 Purchase implementation

**SED-REV-002:** Use RevenueCat (`react-native-purchases` v10+) for in-app purchase management across both platforms. RevenueCat provides: unified API for Google Play Billing 8 and Apple StoreKit 2, receipt validation without a custom backend, dashboard for conversion tracking, and offline entitlement caching (now including cached Offerings, so paywalls render during full backend outage).

**SED-REV-003:** The app MUST function fully offline after initial purchase validation. RevenueCat's offline entitlement caching ensures Pro features remain accessible without network connectivity.

**SED-REV-004:** The free tier MUST be fully functional without any account creation, email entry, or network connectivity.

**SED-REV-005:** Pro features MUST be gated with a single, clearly labelled unlock screen accessible from any locked feature. The screen MUST show: what is included, the price (£4.99, one-time), and a clear "no subscription, no recurring charges" statement. No dark patterns, no misleading trial flows.

### 2.5 Purchase lifecycle and policy (new in v2.1)

v2.0 did not specify how Pro is restored after reinstall, how Family Sharing works, refund policy, or the edge case of a payment that succeeded but didn't sync. v2.1 closes these gaps.

**SED-REV-006:** Refund policy text MUST be published in-app under Settings → About → Purchases and in store listings: "Refunds are handled by Apple or Google according to their policies. If you experience a technical issue that prevents Pro features working, email support@seed.health and we will assist with verification."

**SED-REV-007:** A "Restore purchases" button MUST be present on the Pro unlock screen AND in Settings → About → Purchases. It calls `Purchases.restorePurchases()`. Outcome states: restored / nothing to restore / error (offline) — each with clear, calm copy.

**SED-REV-008:** Family Sharing (iOS): the IAP product MUST be configured as "Family Shareable" in App Store Connect so one purchase covers up to six family members. Google Play does not currently extend Family Library to most managed products; this asymmetry is documented in support FAQ and in the in-app About → Purchases screen.

**SED-REV-009:** Cross-device restore: on a new device after fresh install, Pro is restored automatically if the user signs in with the same Apple ID / Google Account used for the original purchase. Onboarding MUST include an opt-in "I already bought Pro" link (calls `restorePurchases()` once after disclaimer acceptance) so users are not stranded.

**SED-REV-010:** Edge case — payment succeeded, RevenueCat entitlement sync failed: show "Payment received but unlock failed. Tap Restore, or contact support with this ID: &lt;appUserId&gt;." The `appUserId` MUST be retrievable in Settings → About.

**SED-REV-011:** "Delete everything" (SED-CC-013) MUST NOT delete the RevenueCat anonymous user ID by default. A secondary "Also forget my purchase identifier" toggle inside the delete flow MUST carry an explicit warning: "This means you'll need to restore your purchase via the store if you ever reinstall."

**SED-REV-012:** Same device, fresh install — Apple/Google receipts on the device restore the entitlement automatically when `Purchases.configure()` runs and the store account matches. No user action needed. Documented in support FAQ.

**SED-REV-013:** New device, same store account: `restorePurchases()` retrieves entitlement from the store-account purchase history. Documented user journey: install Seed → complete onboarding → tap "I already bought Pro" on the Pro unlock screen → restore runs → entitlement granted.

**SED-REV-014:** New device, *different* store account: no cross-account restore exists (intentional — no accounts). Documented in support FAQ. Support playbook includes a goodwill-grant SOP for legitimate cases.

**SED-REV-015:** "I already bought Pro" CTA MUST appear on the Pro unlock screen with equal prominence to "Unlock now".

---

## 3. Feature specification

### 3.0 Feature summary

| ID | Feature | Tier | Priority | Est. dev time |
|---|---|---|---|---|
| F01 | Due date calculator and countdown | Free | P1 | 4-6 hours |
| F02 | Week-by-week developmental tracker | Free | P1 | 3-4 days |
| F03 | Contraction timer with Labour Mode | Pro | P1 | 4-5 days |
| F04 | Kick counter with pattern view | Pro | P1 | 3-4 days |
| F05 | Weight tracker with chart | Pro | P1 | 3-4 days |
| F06 | Hospital bag checklist | Free | P1 | 2-3 days |
| F07 | Birth plan builder with PDF export | Pro | P2 | 4-5 days |
| F08 | Pregnancy to-do checklist | Free | P2 | 2-3 days |
| F09 | Appointment reminders | Pro | P2 | 3-4 days |
| F10 | Baby name favourites | Free | P3 | 3-5 days |

Cross-cutting enhancements (Partner Mode, Twin Mode, Pregnancy Loss Pathway, Mental Wellbeing Signposting) are specified in Section 4.

**Total estimated feature development: 30-40 days.** Remaining time within 60-day build: app shell, navigation, and foundation (5-7 days), content writing and clinical review (parallel with dev, 5-7 days), design polish (3-5 days), testing (3-5 days), store submission (2-3 days).

> **v2.2 effort re-baseline:** the v2.1 additions (encrypted backup, data export, accessibility pass, custom UI primitives, edge cases, tagged PDF, 16 KB compliance, encryption-key bootstrap, crash diagnostics, multi-language architecture, three-step delete, universal crisis footer, term-audit ESLint rule, beta cohort management) added an estimated **+33 to +44 dev-days** to v2.0's 48–67-day envelope. Realistic full v2.1 scope = 81–111 dev-days for one engineer. To preserve the 60-day target, v2.2 introduces the cut list in §3.0a below. **The cut list is the most consequential decision in v2.2.**

### 3.0a v1.0 / v1.0.x / v1.1 cut list (new in v2.2)

This list governs what ships in the 60-day v1.0, what is patched in within 90 days post-launch as v1.0.x, and what is deferred to v1.1 (months 3–6). The implementation-readiness review identified that without this list, Christian will spend 30–40% of the 60-day window on items that should have been deferred.

**v1.0 (day 60) — MUST ship (regulatory safety, store approval, core promise):**

- All SED-SAF-* and SED-PRI-* (regulatory + encryption-key bootstrap)
- All SED-ARCH-* core architecture invariants
- F01 Due date, F02 Week tracker, F03 Contraction timer + Labour Mode, F04 Kick counter, F05 Weight tracker, F06 Hospital bag, F08 To-do, F09 Appointments (8 of 10 features)
- All SED-CC-001–022 cross-cutting (partner mode, twin mode, loss pathway, crisis footer, three-step delete, EDD+14 quiet prompt)
- All SED-EDGE-001–010 edge cases
- All SED-PRIV-001–009 core privacy
- **SED-PRIV-010 JSON data export only** (defer 011 PDF format to v1.0.x)
- All SED-STORE-* store compliance and SED-REV-* purchase basics
- SED-A11Y core: TalkBack/VoiceOver labels, Dynamic Type, palette contrast — but **with Gluestack retained and selective overrides**, not the full custom primitives set
- All SED-SUP-001–008 support model
- All SED-LEG-001–012 legal (entity, trademark, ICO, ToS, OSS acknowledgements, accessibility statement)
- All SED-CLIN-* clinical sign-off SOP including the new SED-CLIN-006 deputy CSO
- All SED-INC-* (incident response) and SED-SEC-* (vulnerability disclosure) — non-engineering but launch-blocking
- F07 Birth plan — **ship untagged A4 PDF with system fonts** (defer tagged PDF + Atkinson Hyperlegible to v1.0.x)

**v1.0.x (within 90 days post-launch) — patch:**

- F07 birth plan: tagged PDF, large-print variant, Atkinson Hyperlegible (SED-F07-009/010/011)
- SED-PRIV-011 PDF data export format
- SED-BAK-001/002 encrypted backup/restore (deferred from v1.0 — 4–6 dev-days saved)
- Custom UI primitives full set (replace Gluestack overrides)
- Crash diagnostics share button (SED-CI-007) — rely on Help-screen typed reports until then
- Multi-language-ready architecture refactor (i18next wiring; en-GB strings remain hard-coded in v1.0)

**v1.1 (months 3–6 post-launch):**

- F10 Baby names (entire feature deferred from v1.0 — 3–5 dev-days saved). Risk: F10 is in the v2.0 free-tier ship list as a download-driver. Mitigation: an in-app "Baby names — coming in v1.1" tile that links to the ONS public page is a 2-hour build and preserves the free-tier breadth without the 3-5 days of database work.
- F11 Postnatal module (per existing §18 roadmap)
- Welsh content
- System dark mode
- Chorionicity-specific twin schedules

**Cut list quantification:** total v1.0 savings ≈ **15–23 dev-days**, bringing realistic v1.0 envelope to **~58–88 days**. Still slightly over 60 in worst case, but achievable with a 70-day stretch target and a hard 60-day cut at day 35 if behind.

**Cut-list governance:** the cut list is the source of truth for "what ships at v1.0". Every SED-* requirement carries an implicit v1.0 status unless this section moves it to v1.0.x or v1.1. Cuts after v2.2 are PRD changes requiring re-issue, not in-flight decisions.

---

### 3.1 F01: Due date calculator and countdown

**Purpose:** Calculate and display the user's estimated due date and current gestational age.

**Requirements:**

**SED-F01-001:** Accept input as either Last Menstrual Period (LMP) date or Expected Due Date (EDD) provided by the user's maternity team. Default prompt: "When is your baby due?" with a secondary option: "I do not have a due date yet — calculate from my last period."

**SED-F01-002:** If LMP is entered, calculate EDD using Naegele's rule (LMP + 280 days). Display: "Your estimated due date is [date]. Your midwife may give you a different date based on your dating scan. You can update your due date at any time."

**SED-F01-003:** Display current gestational age in weeks and days (e.g. "28 weeks and 3 days").

**SED-F01-004:** Display countdown to EDD in days.

**SED-F01-005:** Display a "term window" bar showing 37+0 to 42+0 with the current position marked. Label: "Your baby is considered 'term' from 37 weeks. Only about 4% of babies arrive on their exact due date."

**SED-F01-006:** Allow the user to update their EDD at any time (e.g. after dating scan adjustment). When updated, recalculate all gestational-age-dependent content throughout the app.

**SED-F01-007:** All date calculations MUST use pure JavaScript date arithmetic. No server calls. No timezone-dependent logic beyond the device's local timezone.

**SED-F01-008:** For twin/multiple pregnancies (see Section 4.2), display an additional note: "Twins are usually born earlier than singletons — your consultant will discuss timing with you, often between 36 and 37 weeks (NICE NG137)."

**SED-F01-009:** Onboarding wording for the EDD question MUST be "When is your pregnancy due?" rather than "When is your baby due?" — the latter wording can land hard for users who have recently experienced loss and are pregnant again. A third option ("I'd rather not enter this now") MUST be offered; selecting it lets the user use the rest of the app without a week display, and the EDD can be added later from Settings.

**Implementation detail:**

```javascript
// Naegele's rule
const calculateEDD = (lmpDate) => {
  const edd = new Date(lmpDate);
  edd.setDate(edd.getDate() + 280);
  return edd;
};

// Gestational age
const getGestationalAge = (edd) => {
  const now = new Date();
  const conceptionEstimate = new Date(edd);
  conceptionEstimate.setDate(conceptionEstimate.getDate() - 280);
  const diffMs = now - conceptionEstimate;
  const totalDays = Math.floor(diffMs / (1000 * 60 * 60 * 24));
  const weeks = Math.floor(totalDays / 7);
  const days = totalDays % 7;
  return { weeks, days, totalDays };
};
```

---

### 3.2 F02: Week-by-week developmental tracker

**Purpose:** Provide weekly information about fetal development and common maternal changes, sourced from NHS and public domain content. Help users engage with their maternity care through prompted questions.

**Requirements:**

**SED-F02-001:** Display a weekly "card" based on current gestational age (weeks 4-42).

**SED-F02-002:** Each card MUST contain all of the following sections:

1. **Baby's size:** Fruit/vegetable comparison illustration, approximate length (mm/cm) and weight (g/kg).
2. **Development this week:** Key developmental milestones (2-4 bullet points).
3. **Your body this week:** Common maternal symptoms and changes (2-4 bullet points).
4. **Questions for your midwife:** 2-3 evidence-based prompts relevant to the gestational week (see examples below).
5. **Key appointments:** If a routine NHS appointment falls in this week window, note it (e.g. "Your dating scan is usually offered between 11 and 14 weeks").
6. **For your birth partner** (visible when partner mode is active, see Section 4.1): What the pregnant person may be experiencing this week, 1-2 practical suggestions.
7. **NHS link:** Direct link to the corresponding NHS week-by-week page.
8. **Attribution:** "Based on NHS pregnancy guidance."

**SED-F02-003:** Content MUST be sourced from NHS "Week-by-week guide to pregnancy" (current URL pattern `https://www.nhs.uk/best-start-in-life/pregnancy/week-by-week-guide-to-pregnancy/{trimester}/week-{N}/` — NHS migrated to "Best Start in Life" IA in 2024–2025; legacy `/pregnancy/week-by-week/` URLs are unreliable) and/or other Tier 1 UK sources (NICE, RCOG, Tommy's). Content MUST be rewritten to avoid copyright infringement while preserving clinical accuracy. All content requires CSO sign-off [Jon]. A build-time check (`scripts/validate-urls.js`) MUST resolve every `nhs_link` to HTTP 200 before submission.

**SED-F02-004:** Data MUST be stored as a static JSON file bundled within the app. No network requests.

**SED-F02-005:** The user MUST be able to browse any week (past or future), not only the current week. Swipe left/right or tap navigation.

**SED-F02-006:** Use high-quality 2D illustrations or vector graphics for baby size comparisons. Do NOT attempt 3D fetal models.

**SED-F02-007 (v2.2 — field reference corrected):** For twin/multiple pregnancies, display adjusted content where it diverges from singleton pregnancy (primarily from week 24 onwards). Twin content is **additive** to singleton content — it renders as a visually distinct "If you're expecting more than one baby" callout panel appended to the relevant card section (development, maternal changes, appointments), not as a replacement. This content is conditional on `settingsStore.pregnancyType !== 'singleton'` (v2.1 changed `isMultiple` boolean to the three-value enum `pregnancyType: 'singleton' | 'twins' | 'triplets_plus'` — v2.1 prose still referenced the old `isMultiple` field; v2.2 corrects this).

**Example "Questions for your midwife" content:**

| Week | Prompts |
|---|---|
| 8-10 | "Ask about which screening tests are available to you." / "Ask what support is available if you are feeling very sick." |
| 12 | "Ask about your dating scan results and what they mean." / "Ask about the combined screening test if you have not discussed it yet." |
| 16 | "Ask about your blood results from your booking appointment." |
| 20 | "Ask about your anomaly scan results." / "Ask whether your placenta position has any implications." |
| 25 | "Ask about your whooping cough vaccination." / "Ask when you need to tell your employer about your maternity leave." |
| 28 | "Ask about your blood group antibodies and whether you need anti-D." / "Ask about your glucose tolerance test results if you had one." |
| 34 | "Start discussing your birth preferences with your midwife." |
| 36 | "Ask about your baby's position and your options if baby is breech." / "Ask about membrane sweeps and what to expect." |

**JSON schema per week:**

```json
{
  "week": 28,
  "baby_size_comparison": "Aubergine",
  "baby_length_cm": 37.6,
  "baby_weight_g": 1005,
  "development_points": [
    "Your baby can open and close their eyes and may turn towards light.",
    "The brain is developing rapidly with new neural connections forming.",
    "Lungs are maturing but are not yet ready to breathe independently."
  ],
  "maternal_changes": [
    "You may notice Braxton Hicks contractions — irregular, usually mild tightenings.",
    "Heartburn and indigestion are common as your uterus presses on your stomach.",
    "You may feel more tired as your body works harder."
  ],
  "midwife_questions": [
    "Ask about your blood group antibodies and whether you need anti-D.",
    "Ask about your glucose tolerance test results if you had one.",
    "Discuss your birth preferences and where you would like to give birth."
  ],
  "key_appointments": "An antenatal appointment is usually offered around 28 weeks. Your midwife will check your blood pressure, test your urine, and measure your bump. You may be offered a blood test for anaemia.",
  "partner_content": {
    "this_week": "Your partner may be experiencing more tiredness, heartburn, and backache. Braxton Hicks contractions can feel alarming. They are usually not a cause for concern — speak to your midwife if you are unsure.",
    "how_to_help": "Offer to take on more household tasks. Ask about their birth preferences and attend antenatal classes together if you can."
  },
  "twin_adjustments": {
    "development_note": "With twins, your babies may be slightly smaller than the singleton sizes shown. This is common and expected with twin pregnancies.",
    "appointment_note": "You will have more frequent scans and appointments. Your next scan may be around 28 weeks.",
    "additional_question": "Ask your consultant about your delivery plan and when they expect your babies to arrive."
  },
  "nhs_link": "https://www.nhs.uk/best-start-in-life/pregnancy/week-by-week-guide-to-pregnancy/2nd-trimester/week-28/",
  "illustration_key": "week_28_aubergine"
}
```

---

### 3.3 F03: Contraction timer with Labour Mode

**Purpose:** Record contraction duration and frequency during labour. Provide a focused, low-distraction interface optimised for use during active labour.

**Requirements:**

**SED-F03-001:** Provide a single prominent button to start/stop timing each contraction. Button MUST be minimum 80x80 points with high contrast.

**SED-F03-002:** Record for each contraction: start timestamp, end timestamp, duration (seconds), and interval since previous contraction start (seconds).

**SED-F03-003:** Display a running list of contractions in the current session with duration and interval in a clear, scannable format (e.g. "Duration: 45s | Gap: 6m 12s").

**SED-F03-004:** Display rolling averages for the last 3 and last 5 contractions (duration and interval).

**SED-F03-005:** The timer MUST use the timestamp delta method (see Section 6.4). It MUST survive app backgrounding, screen lock, and device sleep without losing accuracy.

**SED-F03-006:** Session data MUST persist to local storage via MMKV. The user MUST be able to review previous sessions from a session history screen.

**SED-F03-007:** The contraction timer MUST NOT interpret data, suggest the user is in any stage of labour, or recommend any action based on the recorded data.

**Labour Mode (SED-F03-008 to SED-F03-013):**

**SED-F03-008:** Provide a "Labour Mode" toggle that activates a dedicated dark-theme interface. Dark background (#1A1A2E or similar deep navy), large white/cream text, minimal UI elements. Designed for use in a dimly lit room during labour.

**SED-F03-009:** In Labour Mode, the start/stop button MUST be the dominant screen element (minimum 120x120 points). Haptic feedback (medium impact) on tap.

**SED-F03-010:** In Labour Mode, provide a "Keep screen on" toggle that prevents the device screen from locking. Implement using `expo-keep-awake`.

**SED-F03-011:** In Labour Mode, display only: current contraction status (timing / resting), current duration or interval, rolling averages. Hide the full session history list to reduce visual clutter. The user can exit Labour Mode to see the full list.

**SED-F03-012:** After each session (when the user taps "End session"), display a summary: "Your last [N] contractions averaged [X] minutes apart, each lasting about [Y] seconds. Here is what to tell your maternity unit when you call." This is data formatting for communication purposes. It presents the user's own data back to them in a sentence they can read to a midwife. It does not interpret or advise.

**SED-F03-013:** Include a prominently placed "Call maternity unit" button that opens the device dialler with the user's pre-stored maternity unit number (entered during feature setup or in settings). If no number is stored, prompt the user to add it. Label: "Call your maternity unit."

**Clinical signposting (static, non-personalised):**

**SED-F03-014 (v2.2 — reading-age rewrite):** Display a persistent, non-dismissible banner at the top of the contraction timer screen: "This timer logs your contractions. It does not provide medical advice. The NHS advises calling your maternity unit when contractions are regular. That usually means every 5 minutes, each lasting about 60 seconds. If you are unsure or worried, call your maternity unit." *(v2.1 version was 49 words across 3 sentences with one 27-word sentence at grade ~10; v2.2 splits to 5 sentences averaging 9 words at grade ~5.)*

**SED-F03-015:** Include a "For your birth partner" expandable section (collapsed by default, always visible regardless of partner mode): "You can help by keeping a calm environment, offering water between contractions, timing contractions so your partner does not have to think about it, and noting the pattern to report to the maternity unit when you call."

**Regulatory boundary:** The banner reproduces standard NHS public health guidance available at https://www.nhs.uk/pregnancy/labour-and-birth/signs-of-labour/signs-that-labour-has-begun/. The post-session summary presents the user's own data in a readable format. Neither interprets the user's data against clinical criteria. The app never states "you are in active labour", "you should go to hospital", or "your contractions indicate X."

---

### 3.4 F04: Kick counter with daily pattern view

**Purpose:** Help users track fetal movements as recommended by their maternity team. Provide a 7-day overview that helps users recognise their own baby's pattern, and a clean export for midwife appointments.

**Requirements:**

**SED-F04-001:** Provide a single prominent button to record each perceived movement. Button MUST be minimum 80x80 points. Haptic feedback (light impact) on each tap.

**SED-F04-002:** Record the timestamp of each tap. Display a running count for the current session.

**SED-F04-003:** Include a session timer showing elapsed time since the first movement was recorded in the current session (timestamp delta method).

**SED-F04-004:** Allow the user to end a session and save it with: date, total movements, total duration, and individual movement timestamps.

**SED-F04-005:** Display a history of previous sessions in reverse chronological order.

**Daily pattern view (SED-F04-006 to SED-F04-008):**

**SED-F04-006:** Provide a "Last 7 days" overview screen showing a summary card for each day: date, number of sessions, total movements across all sessions that day, and time of day the sessions were recorded (morning/afternoon/evening as simple labels).

**SED-F04-007:** This view helps the user see their own baby's pattern over time, consistent with Tommy's and RCOG guidance: "Get to know your baby's movements. What matters is a change from what is normal for you." The app MUST NOT define "normal", set thresholds, highlight "low" days, use colour coding to suggest concern, or in any way interpret the data.

**SED-F04-008:** The 7-day view is a neutral data display. It shows the same information a paper diary would show. The user draws their own conclusions.

**SED-F04-013 (new in v2.1):** The daily pattern view MUST NOT include any of: mean line, median line, trend arrows, comparative percentages, or colour-coded cells. All days render in the same visual treatment regardless of count. This explicitly preserves the SED-SAF-001 bright line — no interpretation.

**Export (SED-F04-009 to SED-F04-010):**

**SED-F04-009:** Provide an export function that generates a one-page PDF or shareable text summary of movement history (last 7 or 14 days, user-selectable). Format: date, session start time, number of movements, session duration. Clean, legible, professional layout suitable for showing a midwife at an appointment.

**SED-F04-010 (v2.2 — UK spelling):** PDF header: "Foetal Movement Log — generated by Seed. This is a record of movements logged by the user. It has not been clinically reviewed." NICE/RCOG document titles cited within the app (e.g. NG229 "Fetal monitoring in labour") are quoted verbatim and remain unchanged — only the app-authored "Foetal Movement Log" PDF title uses UK spelling.

**Clinical signposting (static, non-personalised):**

**SED-F04-011 (v2.2 — reading-age rewrite):** Display a persistent, non-dismissible banner: "This tool helps you log your baby's movements. It does not analyse them. There is no set number of movements that is 'normal'. What matters is your baby's usual pattern. If you think your baby is moving less than usual, call your maternity unit. Call now — do not wait until the next day."

**SED-F04-012:** Below the banner, include a tappable link: "More about baby movements — Tommy's" linking to https://www.tommys.org/pregnancy-information/symptoms-and-complications/baby-movements-in-pregnancy.

**Regulatory boundary:** The app logs raw data only. The 7-day view displays historical data in a neutral format. It never compares data to any threshold, baseline, or expected range. The signposting reproduces publicly available Tommy's/RCOG guidance without personalising it.

---

### 3.5 F05: Weight tracker with chart

**Purpose:** Allow users to log their weight and visualise changes over the course of pregnancy.

**Requirements:**

**SED-F05-001:** Accept weight input in kilograms or stones and pounds (user preference, stored in settings). The stones/pounds input MUST use two separate fields (stones: integer, pounds: decimal to one place) as UK users expect. Do not use pounds-only input.

**SED-F05-002:** Record weight with date and gestational week (auto-calculated from EDD).

**SED-F05-003:** Display a time-series line chart using `react-native-gifted-charts`. X-axis: gestational week. Y-axis: weight in the user's chosen unit.

**SED-F05-004:** The chart SHOULD overlay a shaded reference band showing the general range of expected weight gain during pregnancy, sourced from NICE guidance. This band is a single, static range representing the broad population norm. It is NOT personalised to the user's pre-pregnancy weight or BMI. Label: "General range based on NICE guidance. Your midwife will advise you individually."

**SED-F05-005:** The weight tracker MUST NOT calculate BMI, classify the user's weight, provide any advisory text about weight being "too high" or "too low", or change the visual presentation (e.g. colour of the data line) based on the user's position relative to the reference band.

**SED-F05-006:** Allow deletion of individual entries (long-press to delete with confirmation).

**Clinical signposting:**

**SED-F05-007:** Display a note below the chart: "Weight gain in pregnancy varies from person to person. Speak to your midwife if you have concerns about your weight."

**SED-F05-008 (new in v2.1 — eating disorder sensitivity):** Settings → Weight tracker MUST offer a "Hide the chart" toggle. When on, the chart and the reference band are suppressed and only the numeric history list is shown. Onboarding hint at first entry: "If you'd rather not see your weight on a chart, you can switch the chart off in Settings."

**SED-F05-009 (new in v2.1 — sanity range):** Accept any value the user enters; do NOT impose clinical bounds. If an entered weight differs from the previous entry by >10 kg, show a non-coloured, non-alarming inline note: "This is much larger than your last entry. Tap to edit if you meant something different." Tap dismisses; no clinical interpretation.

**SED-F05-010 (new in v2.1 — drag alternative, SC 2.5.7):** Long-press to delete MUST be supplemented with a swipe-to-delete-with-confirm action AND a tap-then-delete button accessible from an entry detail view.

---

### 3.6 F06: Hospital bag checklist (personalised by birth setting)

**Purpose:** Reduce anxiety and cognitive load in the final weeks of pregnancy with a checklist tailored to the user's planned birth setting.

**Requirements:**

**SED-F06-001 (v2.1 — expanded):** On first access, ask: "Where are you planning to give birth?" Options:
- Hospital labour ward
- Birth centre / midwife-led unit
- Home birth
- Planned caesarean section
- Birth in a setting not listed (free-text — covers MGP, specialist units, etc.)
- Not yet decided (uses the default hospital list)

Store the selection in settings. Allow changing at any time.

**SED-F06-002:** Pre-populated checklist of essential items, grouped by category: "For you during labour", "For you after birth", "For your baby", "For your birth partner", "Practical items".

**SED-F06-003:** The item list adapts based on birth setting:

- **Planned caesarean section** adds: loose high-waisted underwear and clothing, pillow for the car journey home, peppermint tea (for wind pain), long phone charger cable (may be in bed for longer).
- **Home birth** removes hospital-specific items and adds: old towels and sheets, plastic sheeting or shower curtain for floor protection, desk lamp or torch (midwife may need directed light), snacks and drinks for your midwives, warm room and space prepared.
- **Birth centre** adjusts: may add birthing pool essentials (bikini top, hair tie), and notes that some items may not be needed if the centre provides them.

**SED-F06-004:** Base items sourced from NHS hospital bag guidance (https://www.nhs.uk/pregnancy/labour-and-birth/preparing-for-the-birth/pack-your-bag-for-labour/).

**SED-F06-005:** Users MUST be able to add custom items to any category.

**SED-F06-006:** Users MUST be able to delete or hide default items they do not need.

**SED-F06-007:** Completion state (checked/unchecked) persisted locally via MMKV.

**SED-F06-008:** Display a progress indicator: "[X] of [Y] items packed."

---

### 3.7 F07: Birth plan builder with evidence context and PDF export

**Purpose:** Help users structure their birth preferences with brief evidence context for each option, generating a clean document for discussion with their maternity team.

**Requirements:**

**SED-F07-001:** Multi-step form (one screen per section) covering:
1. Where I would like to give birth (hospital, birth centre, home, planned caesarean)
2. Who I would like with me
3. My environment preferences (lighting, music, movement, water)
4. Pain relief preferences (breathing/relaxation, water, TENS, gas and air, pethidine/diamorphine, epidural, remifentanil PCA where available)
5. Labour positions and mobility
6. Monitoring preferences (intermittent auscultation vs continuous CTG)
7. Delivery preferences (skin-to-skin, delayed cord clamping, cord blood collection, physiological vs managed third stage)
8. Feeding intentions (breastfeeding/chestfeeding, expressing, formula/bottle, combination, donor milk, tube feeding if recommended, undecided) — see SED-F07-013
9. If things change (preferences for assisted delivery, emergency caesarean section, neonatal care if needed)
10. Special considerations (free text: cultural, religious, disability, previous birth experience, anxiety) — placeholder per SED-F07-014

**SED-F07-002:** Each option within a section MUST include a one-line evidence summary in a subtle, expandable "Why this matters" area. Examples:

- **Delayed cord clamping:** "The NHS advises waiting at least one minute after birth before clamping the cord. The wait may be shorter if your baby needs urgent care (NICE CG190 / NG229)."
- **Epidural:** "An epidural is the most effective form of pain relief in labour. It is available in hospital and some birth centres. Your midwife or anaesthetist can discuss the benefits and risks with you."
- **Skin-to-skin contact:** "The NHS advises skin-to-skin contact as soon as possible after birth. It helps keep your baby's temperature steady and supports bonding and breastfeeding."
- **Planned caesarean section:** "A planned caesarean is a valid birth choice. NICE advises that you discuss the reasons, the procedure, and recovery with your maternity team (NICE NG192)."

> *v2.2 fix: v2.1 evidence summaries used "recommends" (banned by SED-SAF-005 in clinical contexts) and "regulate" (Latinate jargon flagged by SED-CONTENT-001). All four lines rewritten to "advises" / plain English. The SED-SAF-005 exception list (§5.2a) now also formally permits "screening" when naming an NHS-offered screening test.*

**SED-F07-003:** Each section MUST present options as selectable choices (radio buttons for single-select, checkboxes for multi-select) to speed completion. A free text "Additional notes" field SHOULD be available per section.

**SED-F07-004:** The "If things change" section (step 9) MUST be included as a first-class section, not hidden or treated as an afterthought. Introduction text: "Birth does not always go to plan, and that is okay. It can help to think about your preferences in advance, so your birth partner and maternity team know your wishes."

**SED-F07-005:** Generate a clean, printable A4 PDF from completed preferences using `expo-print`. The PDF MUST include:
- A header: "My Birth Preferences — [User's name, optional] — Due date: [EDD]"
- All selected preferences organised by section
- The note: "This is a record of your preferences to share with your midwife and birth partner. Birth is unpredictable, and your maternity team will talk through your options with you."
- Date generated

**SED-F07-006:** The PDF MUST NOT include the evidence summaries (these are for the user's education during completion, not for the printout).

**SED-F07-007:** Allow editing after initial completion. Allow regenerating the PDF after changes.

**SED-F07-008:** Planned caesarean section MUST be treated as a first-class pathway throughout the birth plan, not as a deviation or fallback. If the user selects "Planned caesarean section" in step 1, subsequent sections adapt: skip labour positions and monitoring, add sections for theatre preferences (music, screen lowered to see baby born, who cuts the cord, skin-to-skin in theatre, delayed cord clamping during caesarean).

**SED-F07-009 (v2.2 — downgraded; tagged PDF deferred to v1.0.x):** PDF source HTML MUST use semantic structure (`<h1>`, `<h2>`, `<ul>` rather than styled `<div>`s). However, `expo-print` (the on-device renderer used by Seed) does NOT produce tagged (PDF/UA) PDFs on either iOS (`WKWebView` → `UIPrintPageRenderer`) or Android (`WebView.createPrintDocumentAdapter`) — both paths emit a flat paginated PDF with no logical structure tree. Putting `<h1>` in the HTML does not propagate to PDF accessibility metadata. **True tagged-PDF output is deferred to v1.0.x or v1.1** when a native module wrapping iOS PDFKit / Android PdfDocument can be assessed.

**SED-F07-009a (new in v2.2 — accessibility-equivalent alternative):** Where tagged PDF is not produced, the app MUST offer an accessibility-equivalent alternative: a screen-reader-navigable **in-app birth-plan view** of the same content (already exists from the build step), AND a **plain-text export** via `expo-sharing` ("Share as text"). The plain-text export is the WCAG 2.2 "Alternative Version" the user can paste into an email to their midwife.

**SED-F07-010 (new in v2.1 — large print toggle, retained in v2.2):** The export screen MUST offer a "Large print" toggle that re-renders content at 14 pt base font (up from default 11 pt) with increased line height. Two-page A4 output is acceptable.

**SED-F07-011 (new in v2.1 — accessible font):** Use a humanist sans-serif (Atkinson Hyperlegible preferred, Inter or system fallback) at minimum 11 pt body, 14 pt headings, 1.4 line-height. Avoid condensed faces.

**SED-F07-012 (new in v2.1 — predictable filename):** Filename pattern `birth-preferences-{edd-YYYYMMDD}-v{n}.pdf` for predictable, dated output.

**SED-F07-013 (new in v2.1 — feeding options inclusivity):** The "Feeding intentions" section MUST offer: breastfeeding/chestfeeding, expressing, formula/bottle, combination, donor milk, tube feeding if recommended, undecided. v2.0's binary "breast/bottle/combination/undecided" excludes important options.

**SED-F07-014 (new in v2.1 — special considerations prompts):** Step 10 "Special considerations" free-text MUST include placeholder examples: "e.g. interpreter required; religious requirements around modesty, prayer, dietary needs; disability-related access or communication needs; prior loss; previous traumatic birth; fear of needles."

**SED-F07-015 (new in v2.1 — birth plan PDF footer):** Every page of the exported PDF MUST include the footer: "This PDF is a record of preferences entered by the user. It has not been clinically reviewed and is not medical advice." This mirrors SED-F04-010 and pre-empts a regulatory argument that the personalised PDF constitutes a clinical document.

---

### 3.8 F08: Pregnancy to-do checklist (with employer and practical UK tasks)

**Purpose:** Guide users through key tasks and milestones by trimester, including UK-specific practical and employment tasks that no competitor covers well.

**Requirements:**

**SED-F08-001:** Pre-populated checklist grouped by trimester, with two categories per trimester: "Health and appointments" and "Work and practical".

**SED-F08-002:** Health and appointment items sourced from NHS antenatal timeline. Work and practical items sourced from gov.uk and NHS guidance.

**SED-F08-003:** Example items:

**First trimester (weeks 1-12) — Health:**
- Book your booking appointment (ideally before 10 weeks)
- Have your dating scan (11-14 weeks)
- Discuss screening tests with your midwife
- Start taking folic acid if you have not already (ideally from before conception)

**First trimester — Work and practical:**
- Check your maternity pay entitlement (gov.uk/maternity-pay-leave)
- Find out your employer's maternity leave policy
- Register with a dentist — NHS dental care is free during pregnancy and for 12 months after your baby is born
- Check whether you are entitled to a free NHS prescription exemption certificate (form FW8 from your midwife)

**Second trimester (weeks 13-27) — Health:**
- Have your anomaly scan (18-21 weeks)
- Start antenatal classes (book early, they fill up)
- Have your whooping cough vaccination (from 16 weeks)
- Have your glucose tolerance test if offered (24-28 weeks)

**Second trimester — Work and practical:**
- Tell your employer you are pregnant (you must notify them by 25 weeks)
- Request a workplace risk assessment from your employer
- Start thinking about childcare options (waiting lists can be long)
- Explore Maternity Allowance if you are self-employed or do not qualify for Statutory Maternity Pay

**Third trimester (weeks 28-40+) — Health:**
- Write your birth preferences
- Pack your hospital bag
- Discuss your birth plan with your midwife
- Know the signs of labour
- Learn about breastfeeding (NHS Start4Life)

**Third trimester — Work and practical:**
- Confirm your maternity leave start date with your employer
- Set up your baby's sleeping area (follow safer sleep guidance: lullabytrust.org.uk)
- Install your car seat (if applicable)
- Register for Child Benefit (gov.uk — you can do this before the baby is born)

**SED-F08-004:** Users MUST be able to add custom items to any trimester and category.

**SED-F08-005:** Completion state persisted locally.

**SED-F08-006:** Display progress per trimester: "[X] of [Y] completed."

**SED-F08-007:** For twin/multiple pregnancies, add twin-specific items: "Discuss your birth plan for twins with your consultant", "Ask about steroid injections if delivery before 36 weeks is planned", "Plan for two of everything in your hospital bag".

---

### 3.9 F09: Appointment reminders with "What to expect" briefs

**Purpose:** Help users track upcoming maternity appointments with context about what will happen at each one, reducing anxiety and improving preparation.

**Requirements:**

**SED-F09-001:** Allow users to add appointments with: title, date, time, location (optional), notes (optional).

**SED-F09-002:** Display appointments in chronological order, with past appointments visually distinct (greyed/muted).

**SED-F09-003:** Send local push notifications as reminders. Default: 1 day before and 1 hour before. User-configurable per appointment.

**SED-F09-004:** Use `expo-notifications` for local notifications only. No server-side push infrastructure.

**SED-F09-005:** On first use, offer to pre-populate a suggested appointment schedule based on the NHS routine antenatal care pathway (NICE NG201, updated from CG62). Prompt: "Would you like us to add the standard NHS appointment schedule to your calendar? You can edit or remove any of these." Adjust dates based on the user's EDD.

**SED-F09-006:** For twin/multiple pregnancies, pre-populate the NICE NG137 multiple pregnancy pathway instead, which includes more frequent scans and consultant appointments.

**SED-F09-007:** Each pre-populated appointment MUST include a "What to expect" brief. This is a short (3-5 sentence) description of what typically happens at that appointment. Examples:

| Appointment | What to expect (v2.2 — rewritten for SED-CONTENT-001 grade-6 reading age with plain-English glosses for clinical jargon) |
|---|---|
| Booking appointment (8-10 weeks) | "Your midwife will take your medical history. They will ask about screening tests. They will check your height, weight, and blood pressure. They will also take blood and urine samples. The appointment usually takes about an hour. You will get your handheld maternity notes to keep. Your local maternity unit may offer something slightly different — your appointment letter will confirm what's planned for you." |
| Dating scan (11-14 weeks) | "An ultrasound scan to check your baby's growth and confirm your due date. If you have chosen the combined screening test, the sonographer may also take a nuchal translucency measurement — a check of the fluid at the back of your baby's neck. This checks for Down's, Edwards', and Patau's syndromes. These tests are optional. Your midwife can talk through what each result might mean for you." |
| Anomaly scan (18-21 weeks) | "A detailed ultrasound scan to check your baby's growth. The sonographer will check your baby's bones, heart, brain, face, and main organs. You may be able to find out your baby's sex if you wish." |
| 28-week appointment | "Your midwife will check your blood pressure and test your urine. They will measure your bump — sometimes called symphysis-fundal height. You may be offered a blood test for anaemia. If you are Rhesus negative, you will be offered an anti-D injection (a treatment given when your blood type is Rhesus negative)." |
| 36-week appointment | "Your midwife will check your baby's position. If your baby is breech (bottom-down), you may be offered a procedure called an external cephalic version, or ECV. This is when a doctor gently tries to turn your baby into a head-down position. You will also talk about your birth plan and where you'd like to give birth." |

**SED-F09-007a (new in v2.2 — Trust-variant tail):** Each "What to expect" brief MUST end with the standard tail: "Your local maternity unit may offer something slightly different — your appointment letter will confirm what's planned for you." The tail is bundled as a constant and appended at render time; do not repeat it per brief in `data/appointment-briefs.json`.

**SED-F09-008:** "What to expect" content is static, NHS-sourced, and requires CSO sign-off [Jon].

**SED-F09-009:** Users MUST be able to edit, delete, or add to the pre-populated schedule. They MUST be able to dismiss the pre-population offer and manage appointments manually.

---

### 3.10 F10: Baby name favourites with ONS popularity trends

**Purpose:** Provide a searchable name database with UK popularity data, personal shortlists, and trend visualisation.

**Requirements:**

**SED-F10-001:** Searchable database of baby names. Target: 10,000+ names. Source: ONS baby name statistics (public domain, published annually). Include names from England and Wales dataset.

**SED-F10-002 (v2.1 — non-binary taxonomy):** Filter by: first letter, name style ("traditionally given to boys" / "traditionally given to girls" / "used for any gender"), popularity ranking (current ONS top 100, top 500, all), origin/cultural background where available. Internal type renamed from `gender: 'boy' | 'girl' | 'unisex'` to `nameStyle: 'masculine' | 'feminine' | 'unisex'` — the name has a style, but the baby it's assigned to need not be gendered for naming purposes.

**SED-F10-003:** Each name entry displays: name, style classification (per SED-F10-002), current ONS ranking (if in top 1000), meaning (where available), origin (where available).

**SED-F10-004:** For names in the ONS dataset, display a small sparkline chart showing popularity trend over the last 10-20 years (rising, falling, stable). This uses historical ONS data bundled as static JSON. Label: "Popularity trend (England and Wales)."

**SED-F10-005:** Filter option: "Currently trending" (names that have risen significantly in ranking over the last 5 years) and "Classic" (names that have remained in the top 200 consistently).

**SED-F10-006:** Save names to "My favourites" shortlist. Separate "Partner's favourites" shortlist stored locally. No account sync.

**SED-F10-007:** Display "Names we both like" — the intersection of both shortlists (simple array comparison).

**SED-F10-008:** Data stored as static JSON, bundled in the app. No network dependency. ONS baby-names dataset is Crown Copyright reproduced under the **Open Government Licence v3.0**, which permits redistribution within the app provided the licence is attributed. Licence acknowledgement MUST appear in Settings → Legal → Acknowledgements: "Baby-names data: Crown Copyright. Source: Office for National Statistics, licensed under the Open Government Licence v3.0."

---

## 4. Cross-cutting enhancements

These enhancements apply across multiple features and represent Seed's key differentiators.

### 4.1 Partner Mode (v2.1 — inclusivity expansion)

**SED-CC-001 (v2.1 — rewritten):** Settings selection: "Who is using this device?" with four options:
1. "I'm pregnant"
2. "I'm a partner, co-parent or birth companion"
3. "I'm an intended parent following a surrogate's pregnancy"
4. "We share this phone — ask each time"

Default: option 1. A **secondary toggle** "Show partner-companion content on weekly cards" is independent of (1)–(4) so a solo parent or surrogacy-intended-parent can opt in/out of partner content separately. Solo parents who want partner content fully hidden set this toggle to off.

**SED-CC-002 (v2.1 — extended):** When the user is set to "partner/companion" OR the toggle is on:
- The week-by-week tracker (F02) displays the "For your birth partner" section on each weekly card.
- The contraction timer (F03) always shows the "For your birth partner" coaching tips section (already independent of mode per SED-F03-015).
- Pronouns throughout the app adjust: "your baby" remains unchanged for partner/companion mode (the baby is still theirs), but body-specific content uses third person ("they may be experiencing..." rather than "you may be experiencing...").
- The hospital bag checklist (F06) highlights the "For your birth partner" category.

For "intended parent" (surrogacy):
- Body/symptom content is suppressed ("you may feel...").
- Weekly development content is shown.
- Contraction timer and kick counter are not surfaced as primary tools (they require physical presence to operate).
- Phrasing uses "the person carrying your baby" rather than "your partner".

**SED-CC-003:** Mode is a UI presentation setting stored in MMKV settings. It does not change the underlying data or create a separate user profile. The same device can switch between modes.

**SED-CC-004 (v2.1 — expanded):** Partner-mode content requirements: all "For your birth partner" content is practical, respectful, and actionable. It avoids being patronising ("she needs you to be strong") and focuses on concrete actions. Content MUST be gender-neutral throughout — no "dad", "father", "husband", "wife"; use "partner", "co-parent". Use the pregnant person's name where possible — if unsure, the app's prompt is "if you're not sure how they want to be referred to, ask."

Examples of good partner content: "Offer water between contractions", "Ask if they want the lights dimmed", "Have the maternity unit number ready on your phone."
Examples of content to avoid: "Remember, this is harder for her than for you", "Your job is to stay calm", any reference to "dad" or "father".

**SED-CC-004a (new in v2.1):** A settings field "How would you like to be addressed?" (free text, optional, stored locally) is used in the birth-plan PDF only — never displayed in the app UI to other users (none exist) and never transmitted.

### 4.2 Twin and multiple pregnancy mode

**SED-CC-005:** Settings toggle: "Are you expecting more than one baby?" Options: "One baby" / "Twins" / "Triplets or more". Default: "One baby".

**SED-CC-006:** When twin/multiple mode is active:
- F01 (Due date): additional note about earlier delivery expectations (see SED-F01-008).
- F02 (Week tracker): twin-specific adjustments shown where content diverges from singleton (see SED-F02-007).
- F08 (To-dos): twin-specific items added (see SED-F08-007).
- F09 (Appointments): NICE NG137 multiple pregnancy pathway used for pre-population instead of singleton pathway (see SED-F09-006).
- F06 (Hospital bag): "Pack two of everything for baby" note and adjusted items.

**SED-CC-007:** Twin content MUST be sourced from NICE NG137 (Twin and triplet pregnancy) and Twins Trust (twinstrust.org). Content requires CSO sign-off [Jon].

**SED-CC-008:** Triplets and higher-order multiples: the app provides twin content as a baseline with a note: "Every multiple pregnancy is different. Your consultant will provide a care plan specific to you." The app does not attempt to provide granular guidance for triplets or more.

**SED-CC-022 (new in v2.1 — chorionicity):** Twin pathway in v1.0 follows NICE NG137 baseline for "uncomplicated multiple pregnancy" without distinguishing chorionicity. MC/DC (monochorionic-diamniotic) twins follow a more intensive surveillance schedule than DC/DA. The app surfaces a one-line note in F09: "Your consultant will tell you whether you have monochorionic or dichorionic twins. This affects how often you are scanned. Adjust the suggested schedule to match what your consultant tells you." Granular chorionicity-specific schedules are deferred to v1.1.

### 4.3 Pregnancy loss pathway

**SED-CC-009:** The settings menu MUST include an option: "Pause or end tracking". This option MUST be accessible at all times.

**SED-CC-010 (v2.1 — expanded):** Tapping "Pause or end tracking" displays a compassionate screen. v2.1 adds a softer fourth option ("Hide pregnancy content") that doesn't require the user to declare a loss, and expands the charity list to cover ectopic pregnancy, TFMR, twin-loss, baby-loss counselling, and birth trauma. The Miscarriage Association number is corrected — they have moved to a freephone line.

> "Things may have changed for you — a loss, complications, a difficult test result, or just needing a break. You can pause or end tracking. We're sorry for what you're going through.
>
> **Hide pregnancy content** — keep your data, hide weekly updates and reminders. Resume any time.
>
> **Pause** — stop notifications and updates. Your data is kept.
>
> **End tracking** — stop all tracking. Your data stays on your device until you delete it.
>
> **Delete everything** — remove all data and reset the app.
>
> You don't need to tell us what's happened. None of this information leaves your phone.
>
> **If you need support:**
> - Miscarriage Association: www.miscarriageassociation.org.uk — Helpline: **0303 003 6464** (Mon, Tue, Thu 9–4; Wed, Fri 9–8)
> - Tommy's: www.tommys.org — Helpline: 0800 014 7800
> - Sands (stillbirth and neonatal death): www.sands.org.uk — Helpline: 0808 164 3332
> - Ectopic Pregnancy Trust: www.ectopic.org.uk — Helpline: 020 7733 2653
> - ARC (Antenatal Results and Choices — termination for medical reasons, screening decisions): www.arc-uk.org — Helpline: 0207 713 7486
> - Petals (specialist counselling after baby loss): www.petalscharity.org
> - Aching Arms: www.achingarms.co.uk
> - Twins Trust Bereavement Support Group: twinstrust.org/our-support/bereavement-support
> - Birth Trauma Association: www.birthtraumaassociation.org
> - Child Bereavement UK: www.childbereavementuk.org
> - Mariposa International (parent charity of Saying Goodbye): www.sayinggoodbye.org"

**SED-CC-011 (v2.2 — field reference corrected):** "Pause" sets `settingsStore.trackingStatus = 'paused'`. While paused: all push notifications are cancelled, the home screen shows a neutral message ("Tracking is paused. You can resume at any time or access your data in settings."), no gestational-age content is displayed.

**SED-CC-012 (v2.2 — field reference corrected):** "End tracking" sets `settingsStore.trackingStatus = 'ended'`. Similar to pause but the home screen messaging changes to: "Take care of yourself. Your data is still here if you need it."

> *v2.1 introduced a `trackingStatus` enum (`'active' | 'paused' | 'ended'`) to replace the v2.0 booleans (`trackingPaused`, `trackingEnded`), and v2.1 SED-CC-019 added `'hidden'` for the soft Hide-pregnancy-content option. v2.2 reconciles SED-CC-011/012/014 to reference the enum. The full enum is `'active' | 'paused' | 'ended' | 'hidden'`.*

**SED-CC-013:** "Delete everything" triggers the full data wipe (SED-PRIV-004) and returns to the onboarding screen.

**SED-CC-014 (v2.2 — scope extended to `hidden`):** The app MUST NOT send any push notifications when `trackingStatus` is `'paused'`, `'ended'`, or `'hidden'`. See SED-CC-018 for the enumerated notification categories.

**SED-CC-015:** The app MUST NOT send notifications that reference gestational milestones in a celebratory tone (e.g. "Congratulations, you are 20 weeks today!"). All notifications MUST be neutral and factual (e.g. "Appointment reminder: dating scan tomorrow"). Notifications MUST NOT anthropomorphise the foetus or reference baby size in fruit/vegetable terms.

**SED-CC-018 (new in v2.1 — total notification suppression):** When `trackingStatus` is `paused`, `ended`, or `hidden` (Hide pregnancy content), the app MUST cancel all scheduled local notifications and MUST NOT schedule new ones, including:
- Appointment reminders (F09)
- Due-date countdown or term-window milestone reminders
- Reminders to update due date after dating scan
- Hospital-bag and to-do nudges
- Weekly card "new week" reminders

While paused/ended/hidden, the home screen MUST NOT display the gestational week, countdown, term window, or any baby-size content. The "Resume tracking" control MUST be clearly available in Settings, never popped up as a modal.

**SED-CC-019 (new in v2.1):** "Hide pregnancy content" sets `trackingStatus: 'hidden'`. It is the softest option in the pause/end/delete spectrum and is the default suggestion in the compassionate-screen wording — many users want to step back temporarily without declaring a loss.

**SED-CC-020 (new in v2.1 — universal crisis footer):** A static "Need urgent help?" footer MUST appear on every free-text input screen in the app (birth-plan notes, appointment notes, custom checklist items): "If you are in crisis or in immediate danger, call 999. For urgent mental health support, call Samaritans on 116 123 (free, 24/7)." Non-dismissible, accessible at the bottom of scroll. The app does NOT attempt to detect crisis content (that would require analysis, breaching SED-SAF-001).

**SED-CC-021 (new in v2.1 — destructive action friction):** "Delete everything" requires three steps:
1. An initial screen explains scope, two buttons "Delete everything" (destructive style) / "Cancel".
2. A confirmation modal: "This will permanently delete all your tracking history, checklists, preferences, and notification schedules. This cannot be undone." The user MUST type "DELETE" to enable the button.
3. Final tap with a 3-second progress hold before action commits.

"Pause", "End tracking", and "Hide" are single-tap with a single explanatory modal (they are reversible / non-destructive).

### 4.4 Mental wellbeing signposting

**SED-CC-016:** The "More" tab MUST include a "Your wellbeing" section. This is a static page, not a mood tracker or questionnaire (which would risk device classification). Content:

> "Pregnancy can bring a mix of emotions. Many people feel this way. If you are feeling anxious, low, overwhelmed, or not like yourself, you are not alone.
>
> Speak to your midwife. They hear this every day and they want to help. You will not be judged.
>
> **Support and information:**
> - NHS: Mental health in pregnancy — www.nhs.uk/pregnancy/keeping-well/mental-health/
> - PANDAS Foundation (pre and postnatal depression support) — Helpline: 0808 196 1776 — www.pandasfoundation.org.uk
> - Samaritans (24/7, any time you need to talk) — Call: 116 123 — www.samaritans.org
> - Maternal Mental Health Alliance — www.maternalmentalhealthalliance.org"

**SED-CC-017:** This page MUST NOT include a mood tracker, questionnaire, PHQ-9, GAD-7, or any screening tool. It provides signposting only.

---

## 5. Safety, clinical standards, and regulatory position

### 5.1 Regulatory classification

**SED-SAF-001:** Seed MUST NOT meet the definition of a medical device under UK MDR 2002 or EU MDR 2017/745. The app is a general wellness and organisational tool.

**The bright line is interpretation.** A tool that records data the user enters and displays it back to them is a digital notepad. A tool that interprets, analyses, or draws clinical conclusions from that data is a medical device. Seed stays firmly on the notepad side. This principle MUST be applied to every feature and every piece of user-facing text. MHRA's published qualification flowchart for standalone software/apps explicitly excludes "apps which are limited to storage or display of manually entered user data" and software that "monitors fitness/health/wellbeing and/or stores medical information without change" from medical-device classification (MHRA SaMD guidance).

**SED-SAF-001a — Intended purpose statement (MHRA template).** The following statement MUST be the canonical regulatory positioning, mirrored verbatim in any communication with MHRA, Apple/Google reviewers, or external counsel. It maps 1:1 to the MHRA "Crafting an intended purpose in the context of software as a medical device" template:

> "Seed's intended purpose is to act as a personal organisational and record-keeping aid for people navigating pregnancy in the UK. It does not diagnose, screen, prevent, monitor, predict, prognose, treat or alleviate any disease, injury or disability, nor does it provide information derived from in vivo data that is intended to provide information for medical decision-making.
>
> **Structure and function:** a static-content reference and user-entered data log.
> **Intended population:** adults aged 16+ in the UK who are pregnant (and their partners, co-parents, intended parents in surrogacy, and birth companions).
> **Intended user:** lay person, self-selecting, with no clinical training assumed.
> **Intended use environment:** personal mobile device, used at the user's own initiative, outside any clinical care pathway. The app is not procured by, embedded in, or advocated by any NHS body."

This statement is referenced from SED-STORE-009 (App Store Connect note) and from SED-CLIN-002 (CSO sign-off template).

### 5.2 Clinical content standards

**SED-SAF-002:** All static clinical content MUST be sourced from Tier 1 UK authorities: NHS, NICE, RCOG, Tommy's. See Section 8 for the complete content strategy.

**SED-SAF-003:** All clinical content MUST be reviewed and signed off by the CSO [Jon] before inclusion in any release.

**SED-SAF-004:** Clinical signposting text MUST reproduce or closely paraphrase publicly available NHS/Tommy's guidance. It MUST NOT be original clinical advice.

**SED-SAF-005:** The app MUST NOT use the following terms in any user-facing context (the v2.1 list expands the v2.0 set with additional MHRA-flagged terms that trigger medical-device classification when used in software UX/marketing):

- "safe", "unsafe"
- "normal" / "abnormal" (when applied to user data, the user's body, or the baby)
- "healthy" (when applied to user, baby, or user data — same risk profile as "normal")
- "diagnosis", "diagnose"
- "treatment", "treat", "prescribe"
- "recommend" (in a clinical sense)
- "risk" (when applied to the user specifically)
- "you should" (in clinical contexts)
- **"monitor", "monitoring"** (implies clinical surveillance)
- **"detect", "detection"**
- **"predict", "prediction"** (when applied to clinical outcomes; "predicted due date" is permissible as a date calculation, but "predict labour" is not)
- **"screen", "screening"** (except when referring to a named NHS screening test the user is offered, e.g. "your dating scan and screening")
- **"assess", "assessment"** (in clinical contexts)
- **"warning sign", "red flag"**
- **"reassure", "reassuring"** (avoid implying clinical reassurance from app output)
- **"accurate", "medically accurate"** (overstates the app's role)

Permitted alternatives: "your midwife can advise", "speak to your maternity team", "the NHS advises", "you may wish to discuss with your midwife", "log", "record", "note".

**Enforcement:** the term audit script (`scripts/audit-terms.js`) MUST scan every `.tsx`, `.ts`, and `data/*.json` file for these terms and fail CI on match. A documented exception list MAY exist for legitimate occurrences (e.g. the word "screening" in the NHS-sourced "What to expect" brief for the dating scan); each exception MUST be justified and CSO-signed-off in `CLINICAL_REVIEW_LOG.md`.

**SED-SAF-006:** Every feature that records health-related data (F03, F04, F05) MUST display a persistent, non-dismissible clinical signposting banner as specified in the relevant feature section.

### 5.3 Medical disclaimer

**SED-SAF-007:** On first launch, the app MUST present a non-dismissible, full-screen modal requiring explicit acknowledgement (tap "I understand") before access to any features. The disclaimer text:

> **Before you start**
>
> Seed is a tracking and organisational tool. It does not give medical advice, diagnosis, or treatment.
>
> Using this app doesn't make us your healthcare team.
>
> Always speak to your midwife, GP, or maternity unit about anything you're worried about — for you or your baby.
>
> **In an emergency, call 999.** Contact your maternity unit straight away if anything worries you. This includes any change in your baby's movements, bleeding, severe pain, or any sudden change. Do not wait.
>
> Information in this app is based on NHS and other UK public health sources. It is general guidance and may not apply to you.
>
> [I understand]

**SED-SAF-007a:** The disclaimer text is versioned (`disclaimer_v` integer in `settingsStore`). On app upgrade, if `disclaimer_v` has changed, the user MUST be re-prompted with the new text before access to features. Trivial typo fixes MAY skip re-prompting at CSO discretion, recorded in `CLINICAL_REVIEW_LOG.md`.

**SED-SAF-007b — Screen-reader accommodation.** The "scroll-to-bottom-required" gate on the disclaimer button can trap VoiceOver/TalkBack users who navigate by element rather than scroll position. When `AccessibilityInfo.isScreenReaderEnabled()` returns true, the "I understand" button MUST be enabled after the disclaimer has been fully announced (detected via the focus reaching the final element) rather than requiring scroll. A "Read the disclaimer to me" button using native TTS (`expo-speech`) MAY additionally be offered for low-literacy users.

**SED-SAF-008:** The full disclaimer and privacy policy MUST be permanently accessible from the app settings.

### 5.4 Prohibited feature patterns

The following patterns MUST NOT be implemented in any feature, at any time, under any circumstances:

- Setting movement count thresholds and alerting when breached
- Classifying contraction patterns as "early labour", "active labour", etc.
- Calculating BMI or classifying weight
- Comparing user data to population norms and drawing conclusions
- Sending notifications based on clinical data thresholds
- Using colour coding (red/amber/green) to indicate clinical status
- Providing dosage information for any medication
- Suggesting when to go to hospital based on recorded data
- Interpreting blood pressure, blood test results, or any clinical measurements
- Any form of symptom checking, decision support, or diagnostic reasoning

### 5.5 Clinical sign-off SOP (new in v2.1)

**SED-CLIN-001 — Triggering events.** Clinical review by [Jon] is REQUIRED for: (a) any new content JSON entry; (b) any change to clinical banner text; (c) any new feature touching health data; (d) any change to disclaimer or signposting; (e) every new app version (re-confirmation that no regression introduced prohibited interpretation); (f) every change to the SED-SAF-005 exception list.

**SED-CLIN-002 — Sign-off template.** Each review MUST be recorded in `CLINICAL_REVIEW_LOG.md` using this template:

```
## [YYYY-MM-DD] vX.Y.Z — Feature/Content ID
Reviewer: Jonathan Watchorn, CSO
Artifacts reviewed: [file paths + commit SHA]
Sources verified: [NHS / NICE / Tommy's / RCOG URLs accessed YYYY-MM-DD; cite edition where applicable, e.g. RCOG GTG-57 2nd ed.]
Prohibited term audit: PASS / FAIL [details + exception list deltas]
Regulatory boundary check: PASS / FAIL [details — confirm no interpretation, no thresholds, no advice]
Decision: APPROVED / APPROVED WITH CHANGES / REJECTED
Signature: [initials + date]
```

**SED-CLIN-003 — Content provenance.** Every entry in `weeks.json` and equivalent content files MUST carry a `sources` array of URLs and a `last_verified` ISO date in a parallel `data/provenance.json`. Re-verification cycle: annually, or sooner if a source URL changes. When a source publishes a new edition (e.g. RCOG GTG-57 2nd edition), content MUST be re-reviewed within 90 days.

**SED-CLIN-004 — Post-launch content correction.** Because Seed ships content in the binary (see SED-ARCH-013), every content correction is a new store submission. Standard timeline: minor correction (typo, link rot) bundled with next release; substantive clinical inaccuracy = patch release within 5 working days.

**SED-CLIN-005 — Emergency clinical correction.** For a dangerously wrong statement (residual risk in birth-plan evidence summaries or "What to expect" briefs), the expedited path is: fix → CSO sign-off within 24h → EAS Build production → expedited Apple review request (App Review Contact form, "expedited review" reason "fixing a critical bug") + Google Play full rollout (skip phased). Document the expedited Apple review process in `/docs/clinical/emergency-correction.md`.

### 5.2a SED-SAF-005 exception list (new in v2.2)

The expanded SED-SAF-005 prohibited-terms list bans words that nonetheless appear in legitimate, NHS-mandated, or directly-quoted Tier-1 source contexts. v2.2 codifies a tightly-scoped exception list. Every exception is CSO-signed-off in `CLINICAL_REVIEW_LOG.md`.

| Banned term | Permitted use (exception) | Where it appears |
|---|---|---|
| **screening** | When naming an NHS-offered screening test by its official name (e.g. "combined screening test", "antenatal screening tests") | SED-F09-007 booking and dating-scan briefs; F08 first-trimester to-do |
| **normal** | Inside the Tommy's-attributed direct quote at SED-F04-007: *"What matters is a change from what is normal for you."* AND the negated/scare-quoted occurrence at SED-F04-011: *"There is no set number of movements that is 'normal'."* | F04 banner + signposting |
| **diagnose / treat / cure / prevent / medical condition** | Inside the Google-Play-mandated disclaimer at SED-STORE-001 (Google's policy text uses these words; ours must match) | Store listing + in-app About |
| **risks** (plural, of a procedure) | When discussing the benefits and risks of a specific medical procedure the user is being offered (epidural, ECV, caesarean — not "risks to you") | SED-F07-002 evidence summaries |

**SED-SAF-005a:** All exceptions MUST be enumerated in this table. The `scripts/audit-terms.js` CI check reads the exception list as an allowlist of (term, file-path-pattern, line-content-pattern) tuples — exceptions are file- and context-specific, not blanket. A term used outside its allowed context still fails CI.

### 5.6 CSO bandwidth and bus-factor (new in v2.2)

**SED-CLIN-006 — Deputy CSO.** All clinical sign-off (SED-CLIN-001/002/005, SED-REL-002, content updates, emergency corrections) currently routes to [Jon]. Bus-factor 1 is unacceptable for a clinical product. Pre-launch the company MUST: (a) name a **deputy CSO** — an NHS-employed obstetrician, midwife consultant, or GP with maternity experience — on a retainer (£200–£400/month or per-incident £150/hour) for emergency cover; (b) document the deputy's GMC/NMC PIN and signed scope-of-engagement in `/docs/clinical/deputy-cso.md`; (c) the deputy's name appears in `CLINICAL_REVIEW_LOG.md`; (d) annual review of the arrangement. The deputy MUST be able to action SED-CLIN-005 emergency corrections within 24 hours when [Jon] is unavailable. Risk register entry: R21.

**SED-CLIN-007 — Annual re-verification cadence.** Calendar reminder on 1 September annually (post-NICE summer guideline updates, pre-RCOG autumn updates): [Jon] (or deputy) re-checks every Tier 1 source URL and content claim. Time budget: 1–2 CSO days per cycle. If a source has changed substantively, the change cascades through dependent content via the dependency map maintained in `CLINICAL_REVIEW_LOG.md` (each content entry lists its source IDs; reverse index by source ID). Logged in `/docs/runbooks/annual-content-reverification.md`.

**SED-CLIN-008 — NICE ESF self-classification.** Seed self-classifies as a NICE Evidence Standards Framework **Tier A** digital health technology ("system service / information": delivers information to users without monitoring, diagnosing, or recommending interventions). The product MUST NOT add functionality that pushes it into Tier B (simple monitoring with interpretation) or Tier C (preventative behaviour change, treatment, diagnosis) without a documented re-classification and an MHRA Innovation Office consultation per SED-CLIN-009.

**SED-CLIN-009 — MHRA Innovation Office policy.** No MHRA consultation will be sought for v1.0 (Seed sits inside the bright line per SED-SAF-001 / SED-SAF-001a). The company will book a paid MHRA Regulatory Advice meeting (currently £987 + VAT, one hour) before shipping any feature that arguably crosses the bright line: specifically (a) any v1.1 postnatal feature that adds interpretation; (b) any future fetal-movement scoring; (c) any future symptom-triage flow; (d) any AI/LLM-driven content. The Innovation Office free service is reserved for genuinely novel regulatory questions and SHOULD NOT be used pre-launch for v1.0 because v1.0 is not novel.

---

## 6. Technical specification

### 6.1 Technology stack

Versions targeted for the May 2026 build. **v2.1 update:** v2.0 targeted Expo SDK 52 / RN 0.76 — by May 2026 Expo SDK 55 (with RN 0.83 + React 19.2 + mandatory New Architecture) is stable. SDK 52 is three majors behind. The targets below are the v2.1-recommended set.

| Component | Package / technology | Target version (May 2026) | Purpose |
|---|---|---|---|
| Runtime | React Native | **0.83.x** (via Expo SDK 55) | Cross-platform mobile framework. Bridgeless / New Architecture mandatory. |
| Toolchain | Expo (managed workflow) | **SDK 55** | Build toolchain, native modules, Hermes. SDK 56 in beta — not for launch. |
| React | react | **19.2** | Required by SDK 55. |
| Navigation | expo-router | **v7** (bundled with SDK 55) | File-based routing. |
| State management | zustand | **5.0.10+** | Minimal-boilerplate reactive state. The Jan 2026 5.0.10 release fixes persist phantom re-renders. |
| Persistence | react-native-mmkv | **v4.x** | Synchronous encrypted local storage via Nitro Modules (JSI). |
| Persistence adapter | **inline** (see Section 6.5) | Hand-rolled `StateStorage` wrapped with `createJSONStorage` | v2.0 referenced `zustand-mmkv-storage`; it is a single-maintainer hobby package and is dropped in v2.1. The inline adapter is 6 lines and removes a supply-chain risk. |
| Encryption key store | expo-secure-store | Latest (SDK 55 bundled) | Per-install MMKV encryption key in iOS Keychain / Android Keystore. **New in v2.1 — see SED-PRI-006.** |
| CSPRNG | expo-crypto | Latest (SDK 55 bundled) | Generate the per-install encryption key. |
| Styling | nativewind | **v4.2** | Tailwind CSS utility classes for React Native. NativeWind v5 is on Tailwind 4.1 with a config rewrite — defer to v1.1. |
| UI components | hand-rolled primitives + NativeWind | n/a | v2.0 specified `@gluestack-ui/themed v2`; Gluestack v3 is now copy-paste shadcn-style and overkill for a 10-screen app. v2.1 replaces this with ~8 hand-rolled primitives (`Button`, `Card`, `Pressable`, `Sheet`, `Modal`, `TextInput`, `Switch`, `Checkbox`) in `components/ui/`. Easier to make accessible; removes a heavy dependency. |
| Charts | react-native-gifted-charts | Latest 1.4.x | Lightweight charting for the weight chart and 7-day kick view. **Alternative:** victory-native-xl (Skia-backed) is now the modern choice; either is acceptable for the two charts in scope. |
| Icons | lucide-react-native | 0.5xx | 1,000+ consistent SVG icons. |
| Notifications | expo-notifications | SDK-bundled | Local-only scheduled notifications. Android 13+ POST_NOTIFICATIONS + channels handled per Section 9.4. |
| PDF export | expo-print | SDK-bundled | HTML-to-PDF generation. **Note:** iOS WKWebView cannot resolve local asset URLs in HTML; logos/illustrations MUST be inlined as base64. |
| Screen wake | expo-keep-awake | SDK-bundled | Prevent screen lock during Labour Mode. |
| Haptics | expo-haptics | SDK-bundled | Tactile feedback on timer/counter taps. Respect SED-A11Y-010 "disable haptics" toggle. |
| In-app purchases | react-native-purchases (RevenueCat) | **v10.x** | Cross-platform billing. v9+ adds Play Billing 8 support. Offline entitlement caching is unchanged. |
| Linking | expo-linking | SDK-bundled | Phone dialler and external URL links. |
| Splash screen | expo-splash-screen | SDK-bundled | Controlled splash; gates render on encryption key bootstrap (see Section 6.5). |
| Document picker | expo-document-picker | SDK-bundled | Backup file restore (SED-BAK-002). **New in v2.1.** |
| File system | expo-file-system | SDK-bundled | Backup file write; data export. **New in v2.1.** |
| Sharing | expo-sharing | SDK-bundled | OS share sheet for PDFs, JSON exports, backups, crash logs. **New in v2.1.** |
| Speech (optional) | expo-speech | SDK-bundled | TTS for disclaimer accessibility (SED-SAF-007b). MAY include. |
| Build properties | expo-build-properties | SDK-bundled | NDK r27 / 16 KB page-size support; ProGuard rules; deployment targets. **New in v2.1 — see Section 12.6.** |

**SED-ARCH-EXPO-001:** `expo-updates` MUST NOT be installed in v1.0 — EAS Update is disabled. Every JS or content change goes through store review. See SED-ARCH-013 / SED-ARCH-014.

### 6.2 Architecture principles

**SED-ARCH-001:** Zero network dependency. The app MUST operate with zero network connectivity after initial download and (if Pro) purchase validation. No analytics, no telemetry, no crash reporting, no remote config, no feature flags fetched from a server.

**SED-ARCH-002:** On-device only. All user data MUST be stored exclusively on the user's device using MMKV with encryption enabled. No data leaves the device under any circumstances.

**SED-ARCH-003:** No tracking SDKs. The app MUST NOT include any third-party SDK that transmits data off-device. Explicitly excluded: Firebase Analytics, Google Analytics, Facebook SDK, Sentry, Crashlytics, Amplitude, Mixpanel, and any advertising SDK.

**SED-ARCH-004 (v2.2 — corrected):** Zustand + MMKV persistence. State management MUST use Zustand stores with the **inline `StateStorage` adapter wrapped by `createJSONStorage`** (see §6.5) — NOT `zustand-mmkv-storage`, which v2.1 dropped as a single-maintainer supply-chain risk. Every store mutation is automatically and synchronously persisted to encrypted MMKV storage. No manual save/load logic. No async loading states for persisted data once the per-install encryption key is bootstrapped (see SED-PRI-006/008).

**SED-ARCH-005:** Static content bundling. All clinical content (weekly data, checklists, name database, birth plan options, appointment descriptions) MUST be bundled as static JSON files within the app binary. No network fetches for content.

**SED-ARCH-006:** Single codebase. The same React Native / Expo codebase MUST produce both Android and iOS builds via EAS Build. No platform-specific native code except where Expo modules handle it internally.

### 6.3 Platform targets

**SED-ARCH-007:** Primary: Android (Google Play Store).
**SED-ARCH-008:** Secondary: iOS (Apple App Store). Target submission 2–4 weeks after Android launch.
**SED-ARCH-009:** Minimum Android API: 26 (Android 8.0). Target SDK: API 35 (Android 15). **Builds MUST be 16 KB page-size compatible** (NDK r27+, configured via `expo-build-properties`). Google Play required 16 KB support from 1 Nov 2025 with a grace period to 31 May 2026 — Seed launches into the post-grace period.
**SED-ARCH-010:** Minimum iOS deployment target: 16.0. **Builds MUST use Xcode 26 / iOS 26 SDK** (Apple requirement effective 28 April 2026). EAS Build under SDK 55 does this by default.

### 6.3a EAS Update / OTA policy (new in v2.1)

**SED-ARCH-013:** EAS Update is **disabled for v1.0**. The `expo-updates` module MUST NOT be installed. Rationale: (a) preserves the property that every change reaching users has been clinically and store-reviewed; (b) avoids any perception of dynamic content delivery that could complicate the medical-device boundary; (c) every JS or content change goes through store review.

**SED-ARCH-014:** All content JSON ships in the binary. Any content change = new store version (aligned with SED-CLIN-004).

**SED-ARCH-015 (future):** v1.2 MAY reconsider EAS Update on a JS-bugfix-only channel with a written policy excluding content JSON. Out of scope for v1.0 and v1.1.

### 6.4 Timestamp delta method

All timer features (F03 contraction timer, F04 kick counter session timer) MUST use this pattern. This is critical for accuracy during app backgrounding.

```typescript
// === TIMESTAMP DELTA TIMER HOOK (v2.1) ===
// This hook provides a timer that survives app backgrounding.
// It does NOT use react-native-background-timer or any native background module.
//
// v2.1 changes vs v2.0:
//   1. Pass the named, encrypted `mmkv` instance to useMMKVString (v2.0 silently wrote
//      timestamps to the default unnamed instance — SED-ARCH-002 violation).
//   2. Call recalculate() synchronously on mount, not only on AppState 'active'.
//   3. Re-read AppState.currentState on every screen mount in case 'active' fires
//      were missed on iOS lockscreen-resume.

import { useRef, useState, useEffect, useCallback } from 'react';
import { AppState } from 'react-native';
import { useMMKVString } from 'react-native-mmkv';
import { mmkv } from '@/stores/mmkvStorage';

const useTimestampDeltaTimer = (storageKey: string) => {
  // v2.1 fix: pass the named encrypted MMKV instance as the second argument.
  const [startTimestamp, setStartTimestamp] =
    useMMKVString(`${storageKey}_start`, mmkv);
  const [elapsedMs, setElapsedMs] = useState(0);
  const [isRunning, setIsRunning] = useState(false);
  const intervalRef = useRef<ReturnType<typeof setInterval> | null>(null);

  const recalculate = useCallback(() => {
    if (startTimestamp) {
      const start = parseInt(startTimestamp, 10);
      setElapsedMs(Date.now() - start);
      setIsRunning(true);
    } else {
      setIsRunning(false);
    }
  }, [startTimestamp]);

  // v2.1: recalculate on mount AND on every AppState 'active' transition.
  useEffect(() => {
    recalculate();
    const subscription = AppState.addEventListener('change', (nextState) => {
      if (nextState === 'active') recalculate();
    });
    return () => subscription.remove();
  }, [recalculate]);

  useEffect(() => {
    if (isRunning) {
      // v2.1: force an immediate update before the first 1s tick.
      if (startTimestamp) setElapsedMs(Date.now() - parseInt(startTimestamp, 10));
      intervalRef.current = setInterval(() => {
        if (startTimestamp) {
          setElapsedMs(Date.now() - parseInt(startTimestamp, 10));
        }
      }, 1000);
    }
    return () => {
      if (intervalRef.current) clearInterval(intervalRef.current);
    };
  }, [isRunning, startTimestamp]);

  const start = useCallback(() => {
    setStartTimestamp(Date.now().toString());
    setElapsedMs(0);
    setIsRunning(true);
  }, [setStartTimestamp]);

  const stop = useCallback(() => {
    const duration = startTimestamp ? Date.now() - parseInt(startTimestamp, 10) : 0;
    setStartTimestamp(undefined);
    setIsRunning(false);
    if (intervalRef.current) clearInterval(intervalRef.current);
    return duration;
  }, [startTimestamp, setStartTimestamp]);

  const reset = useCallback(() => {
    setStartTimestamp(undefined);
    setElapsedMs(0);
    setIsRunning(false);
    if (intervalRef.current) clearInterval(intervalRef.current);
  }, [setStartTimestamp]);

  return { elapsedMs, isRunning, start, stop, reset };
};

export default useTimestampDeltaTimer;
```

**SED-ARCH-011:** Timer features MUST NOT use `react-native-background-timer` or any native background task module.

**SED-ARCH-012:** Timer features MUST register an AppState listener that recalculates elapsed time on every foreground event AND on initial mount.

**SED-ARCH-016 (new in v2.1):** Timer hooks MUST pass the named MMKV instance (`mmkv` from `stores/mmkvStorage.ts`) as the second argument to `useMMKVString`. Without this, timestamps are written to the default unnamed (unencrypted) MMKV instance — violating SED-ARCH-002 and SED-PRIV-003. This was a latent bug in the v2.0 code sample.

**SED-ARCH-017 (new in v2.1):** Timer effects MUST force a synchronous display update on mount before relying on the 1 s interval, and MUST re-read elapsed time immediately on every `AppState` "active" transition. The 1 s tick is a UI refresh, never the source of truth.

### 6.5 Zustand store definitions

Each feature has its own Zustand store file. All stores use MMKV persistence middleware.

```typescript
// stores/settingsStore.ts
import { create } from 'zustand';
import { persist } from 'zustand/middleware';
import { mmkvStorage } from './mmkvStorage';

// v2.2 fix: SettingsState now contains every field the v2.1 prose mandates.
// v2.1 added six requirements that referenced settings fields that did not exist
// in the v2.1 interface: userMode 4-value enum (SED-CC-001), showPartnerContent
// toggle, trackingStatus 'hidden' state (SED-CC-019), disclaimer_v versioning
// (SED-SAF-007a), weightChartHidden (SED-F05-008), hapticsDisabled (SED-A11Y-010),
// policyAcknowledgedV (SED-COMM-005). All added in v2.2.

interface SettingsState {
  edd: string | null;                  // ISO date string
  lmp: string | null;                  // ISO date string
  userMode: 'pregnant' | 'partner_companion' | 'intended_parent' | 'shared_device';
  showPartnerContent: boolean;         // v2.2: independent of userMode for solo parents
  pregnancyType: 'singleton' | 'twins' | 'triplets_plus';
  birthSetting: 'hospital' | 'birth_centre' | 'home' | 'caesarean' | 'other' | 'undecided';
  weightUnit: 'kg' | 'stones';
  weightChartHidden: boolean;          // v2.2 (SED-F05-008): eating-disorder sensitivity
  hapticsDisabled: boolean;            // v2.2 (SED-A11Y-010)
  maternityUnitPhone: string | null;
  trackingStatus: 'active' | 'paused' | 'ended' | 'hidden';  // v2.2: + 'hidden'
  disclaimerAccepted: boolean;
  disclaimerV: number;                 // v2.2 (SED-SAF-007a): disclaimer version
  policyAcknowledgedV: number;         // v2.2 (SED-COMM-005): privacy-policy version
  proUnlocked: boolean;
  ageAnswer: 'yes' | 'no' | null;      // v2.2: explicit answer, not a boolean gate
  userName: string | null;             // Optional, for birth plan PDF only
  preferredAddress: string | null;     // v2.2 (SED-CC-004a)
  // Actions (set/reset omitted from listing — pattern unchanged)
  setEdd: (edd: string) => void;
  setLmp: (lmp: string) => void;
  setUserMode: (mode: SettingsState['userMode']) => void;
  setShowPartnerContent: (show: boolean) => void;
  setPregnancyType: (type: SettingsState['pregnancyType']) => void;
  setBirthSetting: (setting: SettingsState['birthSetting']) => void;
  setWeightUnit: (unit: 'kg' | 'stones') => void;
  setWeightChartHidden: (hidden: boolean) => void;
  setHapticsDisabled: (disabled: boolean) => void;
  setMaternityUnitPhone: (phone: string) => void;
  setTrackingStatus: (status: SettingsState['trackingStatus']) => void;
  acceptDisclaimer: (v: number) => void;
  acknowledgePolicy: (v: number) => void;
  unlockPro: () => void;
  setAgeAnswer: (a: 'yes' | 'no') => void;
  setPreferredAddress: (a: string | null) => void;
  resetAll: () => void;
}

const DEFAULTS: Omit<SettingsState,
  | 'setEdd' | 'setLmp' | 'setUserMode' | 'setShowPartnerContent'
  | 'setPregnancyType' | 'setBirthSetting' | 'setWeightUnit'
  | 'setWeightChartHidden' | 'setHapticsDisabled' | 'setMaternityUnitPhone'
  | 'setTrackingStatus' | 'acceptDisclaimer' | 'acknowledgePolicy'
  | 'unlockPro' | 'setAgeAnswer' | 'setPreferredAddress' | 'resetAll'
> = {
  edd: null, lmp: null,
  userMode: 'pregnant',
  showPartnerContent: false,
  pregnancyType: 'singleton',
  birthSetting: 'undecided',
  weightUnit: 'kg',
  weightChartHidden: false,
  hapticsDisabled: false,
  maternityUnitPhone: null,
  trackingStatus: 'active',
  disclaimerAccepted: false,
  disclaimerV: 0,
  policyAcknowledgedV: 0,
  proUnlocked: false,
  ageAnswer: null,
  userName: null,
  preferredAddress: null,
};

export const useSettingsStore = create<SettingsState>()(
  persist(
    (set) => ({
      ...DEFAULTS,
      setEdd: (edd) => set({ edd }),
      setLmp: (lmp) => set({ lmp }),
      setUserMode: (mode) => set({ userMode: mode }),
      setShowPartnerContent: (show) => set({ showPartnerContent: show }),
      setPregnancyType: (type) => set({ pregnancyType: type }),
      setBirthSetting: (setting) => set({ birthSetting: setting }),
      setWeightUnit: (unit) => set({ weightUnit: unit }),
      setWeightChartHidden: (hidden) => set({ weightChartHidden: hidden }),
      setHapticsDisabled: (disabled) => set({ hapticsDisabled: disabled }),
      setMaternityUnitPhone: (phone) => set({ maternityUnitPhone: phone }),
      setTrackingStatus: (status) => set({ trackingStatus: status }),
      acceptDisclaimer: (v) => set({ disclaimerAccepted: true, disclaimerV: v }),
      acknowledgePolicy: (v) => set({ policyAcknowledgedV: v }),
      unlockPro: () => set({ proUnlocked: true }),
      setAgeAnswer: (a) => set({ ageAnswer: a }),
      setPreferredAddress: (a) => set({ preferredAddress: a }),
      resetAll: () => set({ ...DEFAULTS }),
    }),
    { name: 'seed-settings', storage: mmkvStorage }
  )
);
```

```typescript
// stores/contractionsStore.ts
interface Contraction {
  id: string;
  startTimestamp: number;
  endTimestamp: number;
  durationMs: number;
  intervalFromPreviousMs: number | null;
}

interface ContractionSession {
  id: string;
  startedAt: string; // ISO date
  contractions: Contraction[];
  isActive: boolean;
}

interface ContractionsState {
  sessions: ContractionSession[];
  addSession: (session: ContractionSession) => void;
  addContraction: (sessionId: string, contraction: Contraction) => void;
  endSession: (sessionId: string) => void;
  deleteSession: (sessionId: string) => void;
  resetAll: () => void;
}
```

```typescript
// stores/kicksStore.ts
interface KickSession {
  id: string;
  date: string;         // ISO date
  movements: number[];  // Array of timestamps (Date.now() values)
  startTimestamp: number;
  endTimestamp: number | null;
  totalDurationMs: number | null;
}

interface KicksState {
  sessions: KickSession[];
  addSession: (session: KickSession) => void;
  addMovement: (sessionId: string, timestamp: number) => void;
  endSession: (sessionId: string) => void;
  deleteSession: (sessionId: string) => void;
  resetAll: () => void;
}
```

```typescript
// stores/weightStore.ts
interface WeightEntry {
  id: string;
  date: string;      // ISO date
  week: number;      // Gestational week at time of entry
  valueKg: number;   // Always stored in kg internally
  displayUnit: 'kg' | 'stones';
}

interface WeightState {
  entries: WeightEntry[];
  addEntry: (entry: WeightEntry) => void;
  deleteEntry: (id: string) => void;
  resetAll: () => void;
}
```

```typescript
// stores/checklistStore.ts
interface ChecklistItem {
  id: string;
  category: string;
  label: string;
  checked: boolean;
  isCustom: boolean;
  birthSettingFilter?: string[]; // Which birth settings this item appears for
}

interface ChecklistState {
  hospitalBag: ChecklistItem[];
  todos: ChecklistItem[];
  initHospitalBag: (items: ChecklistItem[]) => void;
  initTodos: (items: ChecklistItem[]) => void;
  toggleItem: (listKey: 'hospitalBag' | 'todos', id: string) => void;
  addCustomItem: (listKey: 'hospitalBag' | 'todos', item: ChecklistItem) => void;
  deleteItem: (listKey: 'hospitalBag' | 'todos', id: string) => void;
  resetAll: () => void;
}
```

```typescript
// stores/birthPlanStore.ts
interface BirthPlanSection {
  id: string;
  title: string;
  selections: string[];  // IDs of selected options
  notes: string;
}

interface BirthPlanState {
  sections: BirthPlanSection[];
  completed: boolean;
  setSections: (sections: BirthPlanSection[]) => void;
  updateSection: (id: string, selections: string[], notes: string) => void;
  setCompleted: (completed: boolean) => void;
  resetAll: () => void;
}
```

```typescript
// stores/appointmentsStore.ts
interface Appointment {
  id: string;
  title: string;
  date: string;        // ISO date
  time: string;        // HH:MM
  location: string;
  notes: string;
  whatToExpect: string; // Pre-populated for NHS appointments, empty for custom
  isNHSPrePopulated: boolean;
  reminderScheduled: boolean;
}

interface AppointmentsState {
  appointments: Appointment[];
  addAppointment: (appt: Appointment) => void;
  updateAppointment: (id: string, updates: Partial<Appointment>) => void;
  deleteAppointment: (id: string) => void;
  resetAll: () => void;
}
```

```typescript
// stores/namesStore.ts (v2.1 — renamed gender → nameStyle, see SED-F10-002)
interface FavouriteName {
  name: string;
  nameStyle: 'masculine' | 'feminine' | 'unisex';
  origin: string;
  meaning: string;
}

interface NamesState {
  myFavourites: FavouriteName[];
  partnerFavourites: FavouriteName[];
  addFavourite: (list: 'my' | 'partner', name: FavouriteName) => void;
  removeFavourite: (list: 'my' | 'partner', name: string) => void;
  resetAll: () => void;
}
```

```typescript
// stores/mmkvStorage.ts (v2.2 — boot-crash fix)
//
// v2.1 fixed two v2.0 bugs (hard-coded key, missing createJSONStorage) but
// introduced a boot-crash regression: it used an async initMMKV() + Proxy
// pattern that throws at module-load time because Zustand v5's persist
// middleware hydrates synchronously when storage is synchronous — it calls
// storage.getItem() inside create(persist(...)), which runs at module-import
// time, before any await can resolve.
//
// v2.2 fix: use the SYNCHRONOUS expo-secure-store API (SDK 51+) and the
// synchronous expo-crypto.getRandomBytes(). No Proxy, no async dance, no
// boot crash. Tighter Keychain accessibility (WHEN_UNLOCKED_THIS_DEVICE_ONLY)
// prevents the encryption key syncing via iCloud Keychain.

import { MMKV } from 'react-native-mmkv';
import { createJSONStorage, StateStorage } from 'zustand/middleware';
import * as SecureStore from 'expo-secure-store';
import * as Crypto from 'expo-crypto';

const KEY_NAME = 'seed.mmkv.encryptionKey';

// v2.2: global btoa via Hermes (no `buffer` polyfill needed under SDK 55).
function bytesToBase64(bytes: Uint8Array): string {
  let bin = '';
  for (let i = 0; i < bytes.length; i++) bin += String.fromCharCode(bytes[i]);
  return globalThis.btoa(bin);
}

function bootstrapKey(): string {
  let key = SecureStore.getItem(KEY_NAME); // synchronous (SDK 51+)
  if (!key) {
    const bytes = Crypto.getRandomBytes(32); // synchronous (SDK 51+)
    key = bytesToBase64(bytes);
    SecureStore.setItem(KEY_NAME, key, {
      keychainAccessible: SecureStore.WHEN_UNLOCKED_THIS_DEVICE_ONLY,
    });
  }
  return key;
}

export const mmkv = new MMKV({ id: 'seed-storage', encryptionKey: bootstrapKey() });

const mmkvStateStorage: StateStorage = {
  getItem: (name) => mmkv.getString(name) ?? null,
  setItem: (name, value) => mmkv.set(name, value),
  removeItem: (name) => mmkv.delete(name),
};

// Wrapped with createJSONStorage so Zustand v5 persist serialises correctly.
export const mmkvStorage = createJSONStorage(() => mmkvStateStorage);
```

**SED-PRI-006 (v2.2 — corrected):** The MMKV encryption key MUST be generated at first launch using a CSPRNG (`expo-crypto.getRandomBytes(32)` — synchronous variant) and stored in iOS Keychain / Android Keystore via `expo-secure-store` using `WHEN_UNLOCKED_THIS_DEVICE_ONLY` (v2.1 used `AFTER_FIRST_UNLOCK` which permits iCloud Keychain backup of the key; for an offline-only health app where data also doesn't sync, the key MUST stay device-local). The key MUST NOT appear in source code, build artefacts, exported logs, or any string visible in the JS bundle.

**SED-PRI-007 (v2.2 — SUPERSEDED by SED-PRI-008):** v2.1's async `initMMKV()` + Proxy pattern is removed. The async pattern fails because Zustand v5 `persist` hydrates synchronously when storage is synchronous — `storage.getItem()` is called inside `create(persist(...))` at module-import time, before any `await` resolves, throwing "MMKV accessed before initMMKV() resolved".

**SED-PRI-008 (new in v2.2):** MMKV encryption key bootstrap MUST be synchronous. Use `SecureStore.getItem()` and `Crypto.getRandomBytes()` (both synchronous, available since Expo SDK 51). The synchronous API permits MMKV to be instantiated at module-import time, which permits Zustand `persist` to hydrate synchronously, which is what every store in §6.5 implicitly requires. No Proxy, no `skipHydration`, no rehydrate ceremony. Bootstrap completes in <40 ms on a Pixel 6a — well inside the cold-start budget (SED-QA-007).

**SED-PRI-009 (new in v2.2):** The `bytesToBase64` helper MUST use `globalThis.btoa` rather than `import { Buffer } from 'buffer'`. The `buffer` npm package is NOT bundled by Expo SDK 55 by default and importing it would fail to bundle. Hermes provides `btoa`/`atob` globally since RN 0.72.

### 6.6 Project file structure

```
seed/
├── .github/
│   └── workflows/
│       └── eas-build.yml             # CI: EAS Build on push to main
├── app/                               # Expo Router file-based routes
│   ├── (tabs)/                        # Bottom tab navigation group
│   │   ├── _layout.tsx                # Tab bar layout (4 tabs)
│   │   ├── index.tsx                  # Tab 1: Home (dashboard)
│   │   ├── tools.tsx                  # Tab 2: Tools hub
│   │   ├── checklists.tsx             # Tab 3: Checklists hub
│   │   └── more.tsx                   # Tab 4: More (settings, about)
│   ├── contraction-timer.tsx          # Full-screen contraction timer
│   ├── labour-mode.tsx                # Labour Mode dark UI
│   ├── kick-counter.tsx               # Full-screen kick counter
│   ├── kick-history.tsx               # 7-day pattern view
│   ├── weight-tracker.tsx             # Weight log and chart
│   ├── hospital-bag.tsx               # Hospital bag checklist
│   ├── birth-plan/
│   │   ├── index.tsx                  # Birth plan hub
│   │   └── [section].tsx              # Individual birth plan section
│   ├── todos.tsx                      # Pregnancy to-do checklist
│   ├── appointments/
│   │   ├── index.tsx                  # Appointment list
│   │   └── [id].tsx                   # Appointment detail/edit
│   ├── names/
│   │   ├── index.tsx                  # Name search
│   │   └── favourites.tsx             # Shortlists
│   ├── wellbeing.tsx                  # Mental wellbeing signposting page
│   ├── settings.tsx                   # Settings screen
│   ├── disclaimer.tsx                 # Full disclaimer text
│   ├── privacy.tsx                    # Privacy policy
│   ├── pro-unlock.tsx                 # Pro purchase screen
│   ├── onboarding/
│   │   ├── welcome.tsx                # Welcome screen
│   │   ├── disclaimer-accept.tsx      # Mandatory disclaimer
│   │   ├── due-date.tsx               # EDD / LMP entry
│   │   └── age-check.tsx              # Age confirmation
│   └── _layout.tsx                    # Root layout (disclaimer gate, splash)
├── components/
│   ├── ui/                            # Shared UI primitives
│   │   ├── ProGate.tsx                # Wrapper: shows unlock prompt if not Pro
│   │   ├── ClinicalBanner.tsx         # Non-dismissible signposting banner
│   │   ├── SignpostingLink.tsx        # Tappable external link with icon
│   │   ├── ProgressBar.tsx            # Checklist progress indicator
│   │   ├── WeekCard.tsx               # Weekly tracker card
│   │   ├── TimerDisplay.tsx           # Large format time display (MM:SS)
│   │   ├── BigButton.tsx              # 80pt+ tap target button
│   │   └── SparklineChart.tsx         # Small inline trend chart (names)
│   ├── contraction/
│   │   ├── ContractionList.tsx        # Session contraction history
│   │   ├── ContractionSummary.tsx     # Post-session summary text
│   │   └── RollingAverages.tsx        # Last 3 / last 5 averages
│   ├── kicks/
│   │   ├── DailyPatternView.tsx       # 7-day overview
│   │   └── SessionCard.tsx            # Individual session display
│   ├── weight/
│   │   ├── WeightChart.tsx            # Line chart with reference band
│   │   └── WeightInput.tsx            # kg or stones/lbs input
│   ├── birthplan/
│   │   ├── SectionForm.tsx            # Individual section with options
│   │   ├── EvidencePopover.tsx        # "Why this matters" expandable
│   │   └── PDFGenerator.tsx           # HTML template for PDF export
│   └── names/
│       ├── NameCard.tsx               # Individual name display
│       └── TrendSparkline.tsx         # ONS popularity sparkline
├── stores/                            # Zustand stores (one per feature)
│   ├── mmkvStorage.ts                 # MMKV instance and Zustand adapter
│   ├── settingsStore.ts
│   ├── contractionsStore.ts
│   ├── kicksStore.ts
│   ├── weightStore.ts
│   ├── checklistStore.ts
│   ├── birthPlanStore.ts
│   ├── appointmentsStore.ts
│   └── namesStore.ts
├── data/                              # Static JSON content (bundled)
│   ├── weeks.json                     # 40+ weekly development entries
│   ├── weeks-twin-adjustments.json    # Twin-specific overrides by week
│   ├── hospital-bag-hospital.json     # Hospital birth checklist
│   ├── hospital-bag-caesarean.json    # Planned caesarean additions
│   ├── hospital-bag-home.json         # Home birth checklist
│   ├── hospital-bag-birthcentre.json  # Birth centre adjustments
│   ├── todos.json                     # Trimester to-do items
│   ├── todos-twins.json               # Twin-specific additional items
│   ├── birth-plan-options.json        # Birth plan sections and choices
│   ├── birth-plan-caesarean.json      # Caesarean-specific birth plan flow
│   ├── birth-plan-evidence.json       # Evidence summaries per option
│   ├── appointments-singleton.json    # NHS singleton pathway schedule
│   ├── appointments-twins.json        # NICE NG137 twin pathway schedule
│   ├── appointment-briefs.json        # "What to expect" descriptions
│   ├── names.json                     # Baby name database (10,000+)
│   ├── names-trends.json             # ONS historical ranking data
│   ├── signposting.json              # All clinical banner text
│   └── disclaimer.json               # Disclaimer and privacy policy text
├── hooks/
│   ├── useTimestampDeltaTimer.ts      # Background-safe timer (Section 6.4)
│   ├── useGestationalAge.ts           # Weeks+days from EDD
│   ├── useCurrentWeekData.ts          # Fetch week data from JSON
│   ├── useProGate.ts                  # Check Pro entitlement
│   └── useTrackingStatus.ts           # Check pause/end status
├── utils/
│   ├── dateCalculations.ts            # Naegele's rule, gestational age, term window
│   ├── weightConversion.ts            # kg <-> stones/lbs
│   ├── contractionAverages.ts         # Rolling average calculations
│   ├── pdfTemplates.ts               # HTML templates for PDF generation
│   ├── notificationScheduler.ts       # Local notification helpers
│   └── idGenerator.ts                 # UUID generation for records
├── constants/
│   ├── colours.ts                     # App colour palette
│   ├── layout.ts                      # Spacing, tap target sizes
│   └── copy.ts                        # UI string constants
├── assets/
│   ├── illustrations/                 # Weekly baby size illustrations
│   ├── icon.png                       # App icon (1024x1024)
│   ├── splash.png                     # Splash screen
│   └── adaptive-icon.png             # Android adaptive icon
├── app.json                           # Expo configuration
├── eas.json                           # EAS Build configuration
├── tailwind.config.js                 # NativeWind / Tailwind config
├── tsconfig.json                      # TypeScript configuration
├── package.json
├── README.md                          # Project overview and setup
├── CLINICAL_REVIEW_LOG.md             # Record of Jon's content sign-offs
└── CHANGELOG.md                       # Version history
```

### 6.7 Colour palette

Define in `constants/colours.ts`. Final values subject to design review, but target warm, calm tones:

```typescript
// constants/colours.ts (v2.1)
//
// v2.1 changes: three v2.0 colours failed WCAG 2.2 SC 1.4.3 (4.5:1) when used as
// text or button label, and one banner border failed SC 1.4.11 (3:1) as a non-text
// UI affordance. v2.1 replaces:
//   primary  #7B9E87 (2.81:1) → #4A6B53 (5.66:1; white-on-primary = 5.97:1)
//   primaryDark #5C7A66 (4.49:1) → #3A5742 (pressed; passes for text)
//   textTertiary #9B9590 (2.81:1) → #595550 (7.03:1; body) or #767670 (4.34:1; disabled/large only)
//   secondary #C4A882 (2.15:1) → #9C8050 (~4.5:1) for text use; #C4A882 retained for decorative only
//   proGate  #8B6F47 (4.46:1) → #7A5A38 (5.97:1)
//   bannerBorder #E6A817 (1.92:1) → #B07A00 (3.0:1 on #FFF3E0)

export const colours = {
  // Core
  background:    '#FDF8F4', // Warm cream
  surface:       '#FFFFFF',
  surfaceSubtle: '#F5F0EB',

  // Text
  textPrimary:   '#2D2A26', // 13.54:1 on background — PASS
  textSecondary: '#595550', // 7.03:1 on background — PASS (v2.1 darkened from #6B6560)
  textTertiary:  '#767670', // 4.34:1 — large text and disabled-state only
  textDisabled:  '#9B9590', // decorative / disabled affordances only — do not use for text

  // Accent (v2.1 corrected)
  primary:       '#4A6B53', // 5.66:1; white-on-primary 5.97:1 — PASS
  primaryDark:   '#3A5742', // pressed state — PASS
  primaryLight:  '#E8F0EB', // decorative backgrounds, badges

  // Secondary
  secondary:     '#9C8050', // 4.5:1 — PASS for text. Use #C4A882 for decorative only.
  secondaryDecorative: '#C4A882',

  // Semantic
  proGate:       '#7A5A38', // 5.97:1 — PASS

  // Clinical signposting
  bannerBackground: '#FFF3E0',
  bannerBorder:     '#B07A00', // 3.0:1 non-text UI — PASS SC 1.4.11
  bannerText:       '#5D4037', // 8.50:1 — PASS

  // Labour Mode (dark theme)
  labourBackground: '#1A1A2E',
  labourSurface:    '#25253E',
  labourText:       '#F0EDE8', // 14.61:1 — PASS
  labourAccent:     '#E8B4B8', // 9.46:1 — PASS
  labourAccentPressed: '#D4979C',

  // Focus visible (new in v2.1, required by WCAG 2.2 SC 2.4.11)
  focusRing:        '#1A4D8F', // ≥3:1 against background and primary

  // System
  error:   '#B0001A', // darkened from #C62828 to 5.5:1 — PASS
  success: '#2E7D32',
  divider: '#E8E2DC',
};
```

**SED-UX-017 (new in v2.1):** All foreground/background colour pairs that carry text MUST meet WCAG 2.2 SC 1.4.3 (4.5:1 body, 3:1 large ≥18 pt or ≥14 pt bold). UI component pairs MUST meet 3:1 per SC 1.4.11. A measured contrast table is checked into `constants/colours.ts` as comments and re-validated whenever a colour is changed. A unit test in `constants/__tests__/colours.test.ts` MUST assert the contrast ratios programmatically.

### 6.8 Content authoring workflow (new in v2.2)

[Jon] is the CSO, not a developer. Hand-writing JSON is error-prone and creates a coordination block. v2.2 specifies the workflow that v2.1 left implicit.

**SED-AUTH-001:** Clinical content (weekly cards, appointment briefs, birth-plan evidence summaries, signposting, disclaimers) MUST be authored by [Jon] in **Markdown** using a per-content-type template at `/docs/authoring-templates/` (one template per content type: `week.md`, `appointment-brief.md`, `birth-plan-evidence.md`, `signposting.md`, `disclaimer.md`). Each template has a fixed heading structure that maps 1:1 to the target JSON schema.

**SED-AUTH-002:** A conversion script `scripts/md-to-json.js` parses the Markdown into the canonical JSON written to `data/`. The conversion is deterministic, lossless, and reversible — i.e. round-trip `md → json → md` yields the same Markdown. The Markdown source is the canonical authored artefact; the JSON is generated.

**SED-AUTH-003:** `CLINICAL_REVIEW_LOG.md` tracks the **Markdown commit SHA**, not the JSON file path, as the canonical reviewed artefact. CI regenerates the JSON from the Markdown on every push and fails if the committed JSON drifts from the regenerated output.

**SED-AUTH-004:** The conversion script is itself in scope for the 60-day build (~0.5–1 dev-day to write). Included in the v1.0 ship list.

**SED-AUTH-005 (Optional v1.1):** An admin UI (`/admin` in a separate web app, not in the mobile app) lets [Jon] preview the rendered weekly card and birth-plan PDF from the Markdown source. Deferred to v1.1; v1.0 uses a Storybook or Expo Web preview against the JSON.

---

## 7. Privacy and data protection

### 7.1 Architecture as compliance

The 100% offline architecture dramatically simplifies UK GDPR compliance. There is no data controller/processor relationship for user health data because no data leaves the device. There is no data breach risk because there is no server to breach.

### 7.2 Requirements

**SED-PRIV-001:** The app MUST NOT transmit any user data off-device. Absolute, non-negotiable.

**SED-PRIV-002:** The app MUST NOT require or offer account creation, email entry, or any form of user identification.

**SED-PRIV-003:** All data MUST be stored using MMKV with encryption enabled (configured in `mmkvStorage.ts`).

**SED-PRIV-004:** The app MUST provide a "Delete all my data" option in settings that irreversibly clears all MMKV stores, cancels all scheduled notifications, and resets the app to first-launch state. Require confirmation: "This will permanently delete all your data, including your tracking history, checklists, and preferences. This cannot be undone."

**SED-PRIV-005:** The privacy policy MUST be hosted at a publicly accessible URL and accessible within the app settings.

**SED-PRIV-006:** The privacy policy MUST state: what data is stored (only what the user enters), where it is stored (on the user's device only, encrypted), that no data is transmitted, shared, sold, or accessible to the developers, how to delete data (in-app option), user's rights under UK GDPR Article 15-22, contact details for the company as data controller, and the user's right to complain to the ICO.

**SED-PRIV-007 (v2.1 — revised framing):** Seed processes special-category data only on the user's device. The developer is **not a controller of that data** because no data leaves the device and the developer has no access to it. The first-launch disclaimer therefore is **not** relied upon as Article 9 consent for processing by the developer; it functions as a safety acknowledgement. If, in any future feature, any health data is transmitted off-device, the app MUST present a separate, dedicated, granular consent screen for that processing, distinct from the safety disclaimer, satisfying UK GDPR Articles 4(11), 7 and 9(2)(a).

> *Rationale for the v2.1 framing change:* v2.0 asserted both (a) that no controller relationship exists for health data (§7.1) and (b) that the disclaimer satisfies Article 9(2)(a) explicit consent (§7.2). These two positions are in tension. ICO and EDPB guidance is increasingly hostile to bundling Article 9 consent into a general disclaimer modal because it fails the specificity and freely-given tests. The cleaner position — and the one defensible to an ICO challenge — is that no Article 9 processing occurs at the controller level for the on-device data.

**SED-PRIV-008 (v2.1 — clarified basis):** The company MUST register with the ICO as a data controller (Tier 1 micro-organisation fee: £40, or £35 by direct debit). The basis for registration is the **purchase-data processing performed via RevenueCat** (admitted in SED-STORE-006), not the on-device health data. [Jon]

**SED-PRIV-009 (v2.1 — clarified):** The app MUST comply with the ICO Age Appropriate Design Code (Children's Code). The 15 standards apply to users up to 18 (not 16). Seed's architecture already meets the highest-privacy defaults. The onboarding question "Are you 16 or over?" is **informational only, not a gate**: selecting "No" simply records the answer and continues. Hint text MUST read: "We ask so we don't show certain content to under-16s. The app works either way." This avoids an ICO challenge over a misleading consent flow.

**SED-PRIV-010 (new in v2.1 — UK GDPR Article 20 data portability):** Settings MUST include an "Export all my data" option that generates a JSON file containing every Zustand store's serialised state (settings, contractions, kicks, weight, checklists, birth plan, appointments, name favourites), version-stamped with a top-level `schemaVersion` field. The file is shared via the OS share sheet (`expo-sharing`).

**SED-PRIV-011 (new in v2.1):** A human-readable PDF export option is also offered, structured by feature, suitable for sharing with a midwife or saving for personal records.

**SED-PRIV-012 (new in v2.1):** The privacy policy MUST mention the export right and the in-app route to it. The policy MUST also explicitly state that backup files (Section 7.4) contain health data and the user is responsible for the file once exported.

### 7.3 Terms of Service (new in v2.1)

**SED-LEG-001:** A Terms of Service MUST be hosted at `https://seed.health/terms` (or final brand domain) and linked from in-app Settings → Legal alongside the privacy policy.

**SED-LEG-002:** Required clauses (CSO + solicitor review): (a) Acceptance and age (16+); (b) Licence grant (single, non-exclusive, non-transferable); (c) Medical disclaimer restating SED-SAF-007; (d) No clinical relationship; (e) Pro purchase terms — one-time, non-refundable except via the originating store, no auto-renew; (f) Restore rights and Family Sharing scope; (g) Acceptable use (no reverse engineering, no resale); (h) IP ownership of content (and acknowledgement that NHS/NICE content is Crown copyright / OGL v3.0 where applicable, and ONS baby-names data is OGL v3.0); (i) Limitation of liability — capped at purchase price; (j) Indemnity; (k) Governing law (England and Wales), jurisdiction; (l) Changes to terms (with notice mechanism — since there's no remote update, changes effectively apply only to new installs and to existing installs on next app version); (m) Contact details and ICO complaints route; (n) Severability.

**SED-LEG-003:** EULA: rely on Apple's standard EULA on iOS; on Android, link to Seed's Terms which incorporate end-user terms.

### 7.4 Backup and restore (new in v2.1)

Everything-on-device means changing phones loses data. v2.1 specifies an encrypted backup/restore so users can move their data manually without introducing a server.

**SED-BAK-001:** Settings → "Back up my data to a file" generates an encrypted JSON archive named `Seed-backup-YYYYMMDD.seedbk`. Encryption: AES-256-GCM, key derived from a user-supplied passphrase (minimum 8 chars) via PBKDF2 (100k iterations). The passphrase is never stored.

**SED-BAK-002:** Settings → "Restore from a backup file" uses `expo-document-picker` to select the file and prompts for the passphrase. On success, all stores are repopulated. On failure (wrong passphrase): a clear, non-blaming message. Three wrong attempts in 60 seconds: 30-second cooldown.

**SED-BAK-003:** Backup files MUST NOT include the RevenueCat anonymous app user ID (purchase restore uses the store account, not the backup file).

**SED-BAK-004:** Privacy policy MUST disclose that backup files contain health data and that the user is responsible for the file once exported.

**SED-BAK-005 (new in v2.2 — Android picker MIME):** `.seedbk` is a custom extension with no registered MIME type. `expo-document-picker` MUST be invoked with `type: '*/*'` (not a MIME filter), then the file extension validated post-pick; otherwise Android Scoped Storage providers may hide the file.

**SED-CRYPTO-001 (new in v2.2 — encryption library):** Backup AES-256-GCM encryption MUST use `react-native-aes-gcm-crypto` (Tectiv3/Craftzdog), native, supports AES-GCM and PBKDF2-SHA256, New Architecture compatible. Pure-JS PBKDF2 at the required iteration count is too slow (8–15 s on Galaxy A14 — unacceptable UX). `react-native-quick-crypto` is the alternative but its AES-GCM via `subtle.generateKey` is incomplete as of May 2026; revisit at v1.0.x.

**SED-CRYPTO-002 (new in v2.2 — PBKDF2 iterations raised):** Backup passphrase derivation: PBKDF2-SHA256, **200,000 iterations** (raised from v2.1's 100k — OWASP 2025 minimum is 600k for SHA-256; 200k is the pragmatic floor that completes <1 s on Galaxy A14 and is materially stronger than 100k against offline brute force). Salt: 16 bytes per backup, prefixed in file header. Nonce: 12 bytes for GCM.

### 7.5 Privacy policy expanded content (new in v2.2)

v2.1 SED-PRIV-006 listed required clauses. v2.2 extends with controller/processor analysis and international transfer disclosure.

**SED-PRIV-013 (new in v2.2 — DSAR SOP):** Even though Seed holds no on-device health data accessible to the developer, a user may file a Data Subject Access Request (UK GDPR Article 15). The SOP MUST: (a) acknowledge within 5 working days; (b) explain that no personal data about the user is held by Seed except potentially purchase metadata via RevenueCat; (c) if the user supplies their RevenueCat anonymous app user ID (visible in app About per SED-SUP-004) or App Store / Play receipt, perform a RevenueCat lookup and supply the resulting record; (d) confirm in writing that no other personal data exists; (e) complete within 30 days per UK GDPR Article 12(3). Template stored at `/docs/support-templates/dsar-response.md`. Owner: [Jon] as data controller representative. Documented in `/docs/runbooks/support-triage.md`.

**SED-PRIV-014 (new in v2.2 — joint-controller / processor with RevenueCat):** The privacy policy MUST state RevenueCat's role explicitly. Per RevenueCat's standard Data Processing Addendum, RevenueCat is a **processor** to Seed for purchase data; Seed is the controller. Apple and Google are independent controllers for the underlying payment data. Document the chain: User → Apple/Google (independent controller, payment) → RevenueCat (processor for Seed, entitlement) → Seed (controller).

**SED-PRIV-015 (new in v2.2 — international transfers):** RevenueCat is US-based. UK-to-US transfers rely on the **UK Extension to the EU-US Data Privacy Framework** (in force October 2023) provided RevenueCat is certified, otherwise on Standard Contractual Clauses with a Transfer Impact Assessment. The privacy policy MUST disclose the transfer, the legal mechanism, and the data categories. Verify RevenueCat's DPF certification status at submission and annually thereafter (SED-CLIN-007 cadence).

**SED-PRIV-016 (new in v2.2 — DPIA-lite):** The company MUST produce and maintain a Data Protection Impact Assessment for Seed using the ICO's template, even though the on-device-only architecture (SED-PRIV-001, SED-ARCH-002) means the developer is not a controller of the health data per SED-PRIV-007. The DPIA documents: (a) the legal analysis under which Article 9 is not engaged for on-device processing; (b) the controllership analysis for purchase data flowing via RevenueCat (SED-PRIV-014); (c) the residual risk register (export/share-sheet → user becomes their own controller; backup file lifecycle; on-device-encryption failure modes); (d) consultation with the CSO; (e) sign-off date and review cycle (annual). Filed at `/docs/legal/DPIA-v1.md`.

**SED-PRIV-017 (new in v2.2 — RevenueCat breach playbook):** If RevenueCat notifies the company of a confirmed breach affecting customer purchase data, the company MUST: (a) within 72 hours assess whether the breach engages Article 33 UK GDPR; (b) if yes, notify the ICO via the breach-reporting portal; (c) post a notice at `https://seed.health/status`; (d) because the app has no user accounts/email, no individual notification under Article 34 is possible — instead post a prominent in-app banner via the next release (best-effort).

**SED-PRIV-018 (new in v2.2 — Children's Code post-DUAA monitoring):** The Data Use and Access Act 2025 came into force 5 Feb 2026. ICO will publish revised Children's Code guidance during 2026. The company MUST re-verify the Children's Code position against the ICO's post-DUAA 2026 guidance within 30 days of ICO publication and document the re-check in `CLINICAL_REVIEW_LOG.md`.

**SED-PRIV-019 (new in v2.2 — end-of-life commitment):** Privacy policy MUST commit: "If Seed Health Ltd is acquired or wound down, we will notify users by (a) a final app update with an in-app banner, and (b) a notice at seed.health, no later than 30 days before the change takes effect. User data remains on the user's device under the user's control regardless. RevenueCat purchase records will be deleted per RevenueCat's data-retention policy or transferred to the acquirer under equivalent terms."

### 7.6 Open-source licence acknowledgements (new in v2.2)

**SED-LEG-009 (new in v2.2):** Both stores require disclosure of third-party OSS licences in user-accessible form. The app MUST include Settings → Legal → "Open-source licences" rendering a scrollable acknowledgements view generated at build time. Tooling: `npm-licenses` or `license-checker` invoked in `prebuild` script writes `assets/licences.json`. Per-package format: name, version, licence SPDX ID, copyright notice, full licence text where required (BSD, MIT, Apache-2.0, MPL-2.0). Apache-2.0 packages require the `NOTICE` file content if present. Fonts: if Atkinson Hyperlegible (SIL OFL 1.1) or Inter (SIL OFL) is bundled, the OFL reserved-name and licence text MUST appear. The OGL master attribution (NHS/NICE Crown Copyright + ONS baby-names data per SED-F10-008) appears in the same Acknowledgements screen.

### 7.7 Accessibility statement (new in v2.2)

**SED-LEG-010 (new in v2.2):** Publish at `https://seed.health/accessibility`: (a) conformance claim — "Seed targets WCAG 2.2 Level AA" with date of latest audit; (b) known issues list (if any from SED-QA-005 audit); (c) contact for accessibility complaints (`accessibility@seed.health` or routed through `support@`); (d) statement that the app complies with Equality Act 2010 obligations on service providers; (e) review cadence — annual or on material UI change. Cross-link from in-app Settings → About.

**SED-LEG-011 (new in v2.2 — Equality Act reference):** Add to Section 9 design principles: "The app is designed to avoid placing disabled users at substantial disadvantage as required by the Equality Act 2010. The WCAG 2.2 AA commitment (SED-UX-017, SED-QA-005) is the operating standard. The accessibility statement (SED-LEG-010) is the public commitment."

### 7.8 Verbatim NHS quotation policy (new in v2.2)

**SED-CONTENT-003 (new in v2.2):** Clinical content sourced from NHS.uk, NICE.org.uk or other Crown Copyright / OGL-v3.0 sources MAY be reproduced **verbatim** in Seed provided each block is (a) wrapped in a visually distinct quotation card with the source URL displayed adjacent and the attribution text *"Source: NHS / NICE — Crown Copyright, licensed under the Open Government Licence v3.0"*; (b) listed in `data/provenance.json` per SED-CLIN-003. Paraphrasing remains permitted where editorial fit requires it (subject to SED-CLIN-001 sign-off) but is no longer mandatory. **Rationale:** v2.0/v2.1 required rewriting all content, which is content-production cost without legal benefit (Crown Copyright + OGL v3.0 permit verbatim reproduction) and adds a clinical-classification-drift attack surface that verbatim quotation does not.

---

## 8. Content strategy

### 8.1 Content sourcing rules

| Tier | Sources | Usage |
|---|---|---|
| Tier 1 (primary) | NHS website, NICE guidance, RCOG patient information | All weekly content, checklists, signposting, appointment schedules, birth plan evidence summaries |
| Tier 2 (supplementary) | Tommy's, Start4Life, Best Beginnings, Twins Trust, Lullaby Trust, PANDAS Foundation, Miscarriage Association, Sands | Fetal movement guidance, pregnancy loss signposting, twin-specific content, mental wellbeing signposting, safe sleep references |
| Tier 3 (practical/legal) | gov.uk (maternity rights, Child Benefit, workplace rights), ICO | Employment to-do items, GDPR compliance |
| Prohibited | US sources (CDC, March of Dimes, ACOG, WebMD), Wikipedia, blogs, forums, AI-generated clinical content, any source not listed above without CSO approval | Never used for any clinical or health-adjacent content |

### 8.2 UK terminology

| Use | Do not use |
|---|---|
| Midwife | OB-GYN, obstetrician (unless specifically relevant, e.g. consultant-led care) |
| Maternity unit / birth centre | Hospital L&D, delivery room |
| Dating scan | First ultrasound |
| Anomaly scan | Anatomy scan, 20-week scan (informal; use as secondary only) |
| Booking appointment | First prenatal visit |
| Maternity team | Healthcare provider, care team |
| NHS 111 | Urgent care hotline |
| 999 | 911 |
| Paracetamol | Acetaminophen, Tylenol |
| Gestational weeks | Months of pregnancy (weeks are the primary measure throughout) |
| Stones and pounds | Pounds only (for weight input) |
| Caesarean section / caesarean birth | C-section (acceptable in informal contexts only) |
| Theatre | Operating room, OR |
| Handheld notes / maternity notes | Medical records (in the context of patient-held notes) |
| Anti-D injection | RhoGAM |

### 8.3 Content review process

1. [Christian] drafts or integrates content from approved sources into JSON data files.
2. [Jon] reviews all clinical content for accuracy, tone, regulatory compliance, and source fidelity.
3. Sign-off recorded in `CLINICAL_REVIEW_LOG.md` with date, feature ID, and reviewer.
4. Content is frozen per release. No dynamic content updates without a new app version submitted through the stores.

### 8.4 Illustration requirements

Commission approximately 42 illustrations (weeks 4-42 plus 2-3 supplementary):
- Baby size comparison (fruit/vegetable) for each week
- Simple, warm, inclusive art style with diverse representation **across skin tone, hair texture, body size, age, visible disability (mobility aids, hearing aids), and family composition (single-parent, same-sex couples)**. v2.0 said only "diverse representation" — v2.1 is explicit.
- Vector format (SVG preferred for scalability) or high-resolution PNG
- No anatomically detailed medical illustrations
- Consistent style throughout the set
- All illustrations marked `accessibilityElementsHidden` / `importantForAccessibility="no"` per SED-A11Y-006; textual size description remains in the spoken card.

Budget: £1,000–£2,000. If budget is constrained, use placeholder vector icons for v1.0 and commission the full set for v1.1. The weekly tracker functions without illustrations; they enhance the experience but are not blocking.

---

## 9. User experience

### 9.1 Onboarding flow

Completable in under 60 seconds. Five screens maximum.

**Screen 1 — Welcome:**
App name, tagline ("Your private pregnancy toolkit"), three value propositions in clean iconographic layout: "100% offline", "No ads, no tracking", "NHS-aligned for UK parents".

**Screen 2 — Medical disclaimer (SED-SAF-007):**
Full disclaimer text. Non-dismissible. "I understand" button at bottom (not a checkbox — a full-width button that requires deliberate tap). Button is disabled until the user has scrolled to the bottom of the text.

**Screen 3 — Due date entry (v2.1 — sensitive wording):**
"When is your pregnancy due?" Large date picker. Secondary options:
- "I do not have a due date yet — calculate from my last period" (switches to LMP date picker with Naegele's calculation).
- "I'd rather not enter this now" (lets the user use the rest of the app without a week display; EDD can be added later in Settings).

After entry: "You can update or remove this any time in Settings. It's common for the date to change after a dating scan."

> *Rationale for v2.1 wording change ("your baby" → "your pregnancy"):* "Your baby" lands hard for users early after loss who are pregnant again. "Your pregnancy" is softer and still natural.

**Screen 4 — Quick preferences (v2.1 — inclusive options):**
- "Are you expecting more than one baby?" Toggle: One baby / Twins / Triplets or more
- "Who is using this device?" Selection: I'm pregnant / I'm a partner, co-parent or birth companion / I'm an intended parent following a surrogate's pregnancy / We share this phone — ask each time
- "Are you 16 or over?" Toggle: Yes / No — with hint "We ask so we don't show certain content to under-16s. The app works either way." (Selecting No does NOT block; per SED-PRIV-009.)
- "I already bought Pro" small link at the bottom (calls `Purchases.restorePurchases()` — see SED-REV-015).

**Screen 5 — Dashboard:**
Immediately show current gestational week (or a neutral "Welcome" if EDD declined), countdown, term window, and feature access. Onboarding complete.

**SED-UX-007 (new in v2.1 — onboarding edge cases):** First-launch dashboard adapts:
- &lt;8 weeks: emphasise the booking appointment to-do.
- 8–36 weeks: normal dashboard.
- 37+ weeks: dashboard highlights Labour Mode, hospital bag, and "Signs of labour" signposting. Hide "rising trend" content.
- LMP &gt; 42 weeks: see SED-EDGE-002.
- EDD declined ("I'd rather not enter this now"): show a neutral welcome with feature tiles but no week display.

**SED-UX-019 (new in v2.1 — postnatal not in v1.0):** Onboarding offers "Has your baby already been born?" — if yes: "Postnatal features are not yet available. Seed v1.0 is for pregnancy through to labour. You can keep your data for reference, or delete it." (See Section 18 Roadmap.)

### 9.2 Design principles

**SED-UX-001:** Clean, calm, uncluttered. Prioritise readability and ease of use. Especially during labour: large tap targets, high contrast, minimal cognitive load.

**SED-UX-002 (v2.1 — upgraded to WCAG 2.2):** All interactive elements MUST have minimum 44×44 pt tap targets, satisfying WCAG 2.2 Level AA SC 2.5.8 (which requires 24×24 CSS px minimum). Timer/counter buttons: minimum 80×80 pt. Labour Mode buttons: minimum 120×120 pt.

**SED-UX-003 (v2.1 — extended for Dynamic Type):** Typography: system fonts (San Francisco on iOS, Roboto on Android) for performance and familiarity. Clear hierarchy: title (24 pt bold), heading (18 pt semibold), body (16 pt regular), caption (14 pt regular). All `Text` components MUST allow font scaling — `allowFontScaling={false}` is forbidden anywhere in the codebase. Layouts MUST tolerate 200% font scale; buttons grow or wrap rather than truncate. `numberOfLines` is used only for one-line UI labels; when used, paired with `ellipsizeMode="tail"` and the full string exposed via `accessibilityLabel`.

**SED-UX-004:** Spacing: generous. Minimum 16 pt padding on all content areas. 24 pt between sections. Cards with 16 pt internal padding and 8 pt border radius.

**SED-UX-005:** Clinical signposting banners: warm amber background (not clinical red or yellow), clear readable text, non-dismissible, positioned at the top of the relevant screen before any interactive content.

**SED-UX-006:** Pro gate: locked features show a subtle lock icon overlay. Tapping opens the Pro unlock screen. No aggressive upsell, no popups, no "limited time offer" tactics. Single calm screen: "Unlock all tools — £4.99 one-time" with feature list and purchase button. The screen MUST include an "I already bought Pro" CTA with equal prominence — see SED-REV-015.

**SED-UX-008 (new in v2.1):** WCAG 2.2 Level AA conformance is the accessibility target across the app (replacing the v2.0 reference to 2.1 AA).

**SED-UX-009 (new in v2.1 — focus visible, SC 2.4.7 / 2.4.11):** Every focusable control MUST render a visible focus ring with ≥3:1 contrast against the adjacent background. Sticky banners (SED-F03-014, SED-F04-011) MUST NOT obscure the focused control (SC 2.4.11). Keyboard avoiding padding MUST keep focused elements above the soft keyboard.

**SED-UX-010 (new in v2.1 — dragging alternative, SC 2.5.7):** Every drag/long-press control MUST have a single-tap alternative. Affects F05 long-press delete (add a swipe-to-delete with confirm AND a tap-then-delete button in entry detail view) and any future drag-to-reorder lists.

**SED-UX-011 (new in v2.1 — consistent help, SC 3.2.6):** "Call maternity unit" (where stored), "Pause/end/hide tracking", disclaimer, and privacy policy MUST be reachable from a consistent location (Settings menu) on every screen.

**SED-UX-012 (new in v2.1 — redundant entry, SC 3.3.7):** Any data the user has already entered (name, EDD, maternity unit phone) MUST be pre-filled where it is requested again (e.g. the birth-plan PDF reuses `userName` from settings).

**SED-UX-013 (new in v2.1 — status messages, SC 4.1.3):** Post-session summaries, tracking-pause confirmations, and Pro-unlock confirmations MUST use `accessibilityLiveRegion="polite"`. Crash-recovery prompts MUST use `assertive`.

**SED-UX-014 (new in v2.1 — reflow, SC 1.4.10):** Every screen MUST render without horizontal scroll at 320 CSS px width / 400% zoom.

**SED-UX-015 (new in v2.1 — text spacing, SC 1.4.12):** Layouts MUST not break when line-height ≥1.5×, paragraph spacing ≥2×, letter-spacing ≥0.12em.

**SED-UX-016 (new in v2.1):** 44 pt minimum tap target (SED-UX-002) satisfies SC 2.5.8 — recorded as conformance evidence in the pre-launch checklist.

**SED-UX-018 (new in v2.1 — system dark mode):** The app SHOULD honour `useColorScheme()` and provide a full dark theme across every screen. Labour Mode remains a separately-enabled experience for the contraction timer. **v1.0 acceptable compromise:** lock to `userInterfaceStyle: 'light'` (per app.json) and add system dark mode as v1.1 — recorded in Risk Register R11.

### 9.2a Screen reader, dynamic type, motion (new in v2.1)

**SED-A11Y-001 — Labels:** Every interactive element MUST have a non-empty `accessibilityLabel`. Where the visible label is sufficient, it MAY be reused; otherwise, provide a descriptive label (e.g. start/stop contraction button: "Start contraction" / "Stop contraction, current duration 42 seconds").

**SED-A11Y-002 — Roles:** Every actionable control MUST set `accessibilityRole` (`button`, `link`, `header`, `switch`, `checkbox`, `radio`, `image`, `text`).

**SED-A11Y-003 — States:** Stateful controls MUST set `accessibilityState` (`{ selected, checked, disabled, expanded }`). Pro-locked tiles set `disabled: true` with `accessibilityHint: "Locked. Double-tap to open the Pro unlock screen."`.

**SED-A11Y-004 — Grouping:** Grouped content (week-card sections, checklist categories) MUST use `accessibilityRole="group"` with `accessibilityLabel` to allow grouped navigation.

**SED-A11Y-005 — Live regions:** Timer updates in Labour Mode MUST use `accessibilityLiveRegion="polite"` for rolling averages and `accessibilityLiveRegion="assertive"` for "session ended" announcements. Continuous per-second readouts MUST NOT be live regions; instead, announce on contraction end.

**SED-A11Y-006 — Decorative imagery:** Fruit illustrations and similar decorative images MUST set `accessibilityElementsHidden={true}` (iOS) / `importantForAccessibility="no"` (Android). The textual "size of an aubergine" remains in the spoken card.

**SED-A11Y-007 / 008 / 009 — Dynamic type:** Covered by SED-UX-003 v2.1. Additionally, Labour Mode numerics MAY use a maximum scale cap (e.g. 1.5×) to preserve glanceability — only after low-vision user testing confirms the cap. Default: unrestricted scaling.

**SED-A11Y-010 — Reduced motion:** All non-essential animation (page transitions, chart entry animations, sparkline draws, term-window progress fills) MUST check `AccessibilityInfo.isReduceMotionEnabled()` and degrade to instant transitions. Haptics MAY remain (they're not "motion"), but a **"Disable haptics"** toggle MUST be present in settings — some users find vibration uncomfortable or migraine-triggering.

**SED-A11Y-011 — Voice Control / Switch Control:** Every button MUST have a unique, speakable `accessibilityLabel` that Voice Control users can say to invoke. Disambiguate duplicate labels on the same screen (e.g. two "Delete" buttons → "Delete entry" / "Delete session").

### 9.2b Reading age and localisation (new in v2.1)

**SED-CONTENT-001:** All user-facing copy targets a reading age of 9–11 years (NHS service-manual aligned). Sentences average ≤15 words. Avoid passive voice where possible. Avoid Latinate medical terms when an Anglo-Saxon equivalent exists ("womb" over "uterus" in general content; clinical names retained when discussing tests). Use the Hemingway Editor or comparable tool; flag any content above grade 8 for rewrite.

**SED-CONTENT-002:** Copy MUST be in UK English (British spellings, UK terminology per §8.2). All user-facing strings MUST be sourced from a single locale file `data/copy.en-GB.json` to enable future translation without code changes. Welsh-language support (`cy-GB`) is targeted for v1.2 (see Roadmap §18).

### 9.3 Navigation

Bottom tab bar with four tabs:

| Tab | Icon (Lucide) | Label | Contents |
|---|---|---|---|
| 1 | `baby` or `heart` | Home | Dashboard: week card, countdown, term window, quick links |
| 2 | `timer` | Tools | Grid: Contraction timer, Kick counter, Weight tracker |
| 3 | `clipboard-check` | Lists | Grid: Hospital bag, To-dos, Appointments |
| 4 | `menu` | More | Birth plan, Baby names, Your wellbeing, **Help & support** (SED-SUP-008), Settings, About, Disclaimer, Privacy, **Terms** (SED-LEG-001), **Acknowledgements** (ONS OGL, NHS attribution) |

Pro-gated tools show a small lock badge on their grid tile. Tapping navigates to the tool with the Pro gate overlay if not unlocked. Per SED-UX-011, the Settings menu is reachable from every screen via the More tab and is the consistent location for help, pause/end/hide, disclaimer, and privacy.

---

## 10. App store compliance

### 10.1 Google Play Store

**SED-STORE-001 (v2.1 — disclaimer wording mandated by Google's Jan 2026 policy):** Complete the Health Apps Declaration Form. Declare: health data processed on-device only, no data transmitted, app is not a medical device, medical disclaimer presented on first launch. The Play Store listing copy AND the in-app About screen MUST include the exact wording: **"This app is not a medical device and does not diagnose, treat, cure, or prevent any medical condition. Always consult a healthcare professional."**

**SED-STORE-002:** Target SDK API 35 (Android 15) or higher. Builds MUST be 16 KB page-size compatible (SED-ARCH-009).

**SED-STORE-003:** Store listing must include medical disclaimer text in "About this app".

**SED-STORE-004:** Content rating: IARC, likely "Everyone" with health-information content descriptor.

**SED-STORE-005:** Listing copy must include: "100% offline — your data never leaves your phone", "No ads, no subscriptions", "NHS-aligned content for UK parents", and the medical disclaimer. Category: **Health & Fitness** (not Medical — Medical is reserved for clinical/SaMD products and would trigger heightened review scrutiny inconsistent with Seed's non-medical-device positioning).

### 10.2 Apple App Store

**SED-STORE-006 (v2.1 — clarified):** App Privacy nutrition label: **"Data Not Collected"** for every health, identifier, contact, location, and usage category. Declare purchase data only if RevenueCat's anonymous app user ID is treated as a "linked-to-user" identifier; the conservative position is to declare "Purchases — Not linked to you" since the app user ID is anonymous and not tied to identity.

**SED-STORE-007:** Include medical disclaimer in App Store description.

**SED-STORE-008:** Comply with App Store Review Guidelines 5.1.1 (Health, Medical, and Human Subject Research) — the app provides organisational tools, not medical advice. Category: **Health & Fitness**.

**SED-STORE-009 (new in v2.1 — Apple Spring 2026 regulated-medical-device declaration):** In App Store Connect, the **regulated medical device status for the United Kingdom MUST be set to "not a regulated medical device"** with supporting note: "Seed is a general wellness and organisational tool. It is not a medical device under UK MDR 2002." A reviewer-facing note MUST reference SED-SAF-001a (intended purpose statement).

**SED-STORE-010 (new in v2.1):** `NSUserTrackingUsageDescription` MUST NOT be present in Info.plist (no AppTrackingTransparency call is ever made). Apple Review flags inconsistencies between Info.plist declared usage and SDK behaviour.

---

## 11. Build plan and phased delivery

### 11.1 Phases (60-day target)

**Phase 1: Foundation (days 1-7)** [Christian]

- Initialise Expo project with TypeScript
- Install and configure: expo-router, nativewind, react-native-mmkv (v4 + named-instance encryption-key bootstrap per §6.5 v2.2), zustand (with inline persistence adapter — NOT `zustand-mmkv-storage`), lucide-react-native, expo-secure-store, expo-crypto. Gluestack v3 components MAY be used for v1.0 with selective accessibility overrides; full custom primitives are deferred to v1.0.x per §3.0a cut list.
- Initialise GitHub repository (see Section 12)
- Build bottom tab navigation shell
- Build onboarding flow (5 screens as specified in Section 9.1)
- Implement settings store with MMKV persistence
- Implement disclaimer gate in root layout (blocks app access until disclaimer accepted)
- Implement Pro gate component (RevenueCat integration)
- Define colour palette and base styling

**Phase 2: Free tier features (days 8-18)** [Christian]

- F01: Due date calculator and countdown with term window
- F02: Week-by-week tracker (full UI with placeholder JSON content pending clinical review)
- F06: Hospital bag checklist (with birth-setting personalisation)
- F08: Pregnancy to-do checklist (with work/practical UK tasks)
- Partner mode toggle and conditional content rendering
- Twin mode toggle and conditional content rendering

**Phase 2 clinical content (parallel, days 8-30)** [Jon]

- Draft week-by-week content (weeks 4-42): all JSON fields per schema
- Draft "Questions for your midwife" for each week
- Draft partner content for each week
- Draft twin adjustment content
- Review and approve all checklist items
- Review and finalise all signposting and disclaimer text
- Source/commission illustrations or define placeholder approach

**Phase 3: Pro tier features (days 19-35)** [Christian]

- F03: Contraction timer (timestamp delta hook, session history, rolling averages, Labour Mode dark UI, "Call maternity unit" button, partner coaching section)
- F04: Kick counter (tap recording, session timer, 7-day pattern view, PDF export)
- F05: Weight tracker (stones/lbs and kg input, chart with reference band)
- F09: Appointment reminders (manual entry, NHS pathway pre-population, "What to expect" briefs, local notifications)

**Phase 4: Secondary features and cross-cutting (days 36-45)** [Christian]

- F07: Birth plan builder (multi-step form, evidence popovers, caesarean pathway, PDF generation)
- F10: Baby name favourites (search, filters, ONS trends sparkline, dual shortlists)
- Pregnancy loss pathway (pause/end/delete flow, notification cancellation)
- Mental wellbeing signposting page
- Cross-cutting: ensure partner mode, twin mode, and birth setting adaptations work correctly across all features

**Phase 5: Polish and integration (days 46-53)** [Christian + Jon]

- Integrate final clinical content JSON and illustrations
- Design polish: consistent spacing, colour palette, typography, iconography
- Accessibility pass: tap targets, contrast ratios, screen reader labels (accessibilityLabel on all interactive elements)
- Manual testing on physical devices: minimum 2 Android devices (different screen sizes), 1 iOS device
- Edge case testing: very early pregnancy (week 4), overdue (week 42), paused tracking, empty states
- Performance testing: app launch time, MMKV read/write, chart rendering with 40+ data points

**Phase 6: Submission (days 54-60)** [Christian + Jon]

- Host privacy policy and Terms of Service at public URLs
- Finalise medical disclaimer text (CSO sign-off, version stamp set)
- Write app store listings (description, feature list, screenshots, feature graphic) — all CSO-reviewed against SED-SAF-005
- Generate screenshots for both platforms per SED-ASSET specs
- Configure ICO registration and trademark clearance (see SED-LEG-004)
- Google Play submission (Health Apps Declaration, content rating)
- Apple App Store submission (may extend 1-2 weeks beyond day 60 due to review times); set regulated-medical-device status to "not a regulated medical device" per SED-STORE-009

### 11.3 Release management (new in v2.1; renumbered correctly in v2.2)

*(v2.1 placed this section before §11.2 in the document; v2.2 preserves the section IDs for backward compatibility but readers should follow the order §11.1 → §11.2 → §11.3 as listed in the TOC. The release-management content remains under §11.3 below; content-deliverables under §11.2 follows.)*

**SED-REL-001 — Versioning:** Semantic versioning MAJOR.MINOR.PATCH. MAJOR = breaking content-schema change requiring data migration. MINOR = new features. PATCH = bug fixes, content corrections, copy edits. `versionCode` (Android) and `buildNumber` (iOS) increment monotonically on every submission via EAS `autoIncrement`.

**SED-REL-002 — Release notes:** `CHANGELOG.md` updated per release using Keep-a-Changelog format. Store release notes &lt;500 chars, user-facing language, with explicit mention of any clinical content updates. Every PATCH that touches `data/` requires CSO sign-off per SED-CLIN-001.

**SED-REL-003 — Phased rollout:**
- Google Play: 5% (day 1) → 20% (day 3 if no spike in 1-star reviews) → 50% (day 7) → 100% (day 14).
- iOS: Apple's 7-day default phased release enabled.

**SED-REL-004 — Bad-release detection without analytics:** because Seed has no telemetry (SED-ARCH-003), the operating model is explicit:
- (a) Daily store-review scan for the first 14 days (Play Console + App Store Connect notifications enabled; manual check first thing each morning by [Christian]).
- (b) Support inbox monitored daily; ≥3 reports of the same issue within 48 hours = halt rollout.
- (c) RevenueCat dashboard checked daily for purchase-failure rate spike.
- (d) Manual smoke test on Pixel 6a + iPhone 12 immediately after each production build is promoted (see Section 12.8 device matrix).

**SED-REL-005 — Rollback procedure:** halt rollout in Play Console (one click); on iOS, "Remove from Sale" is the rough equivalent and a hotfix is prepared in parallel. The App Store cannot truly roll back — only ship a corrective build. A "previous-good" tagged commit (`v-stable-YYYYMMDD`) MUST be maintained so reverting source is one command.

**SED-REL-006 — Crash-threshold proxy:** since no Crashlytics, define "release halt" as any user-reported crash reproducible in &lt;3 attempts on a reference device. Repro instructions captured in support inbox; reproduction attempted within 4 working hours of first report.

### 11.2 Content deliverables [Jon]

| Deliverable | Format | Needed by |
|---|---|---|
| All signposting and disclaimer text | `data/signposting.json` and `data/disclaimer.json` | Day 10 |
| Hospital bag checklist items (all birth settings) | `data/hospital-bag-*.json` | Day 14 |
| Trimester to-do items (incl. work/practical, twins) | `data/todos.json` and `data/todos-twins.json` | Day 14 |
| Week-by-week content (40 entries, all fields) | `data/weeks.json` | Day 30 |
| Twin adjustment content | `data/weeks-twin-adjustments.json` | Day 30 |
| Birth plan sections, options, evidence summaries | `data/birth-plan-*.json` | Day 30 |
| Appointment "What to expect" briefs | `data/appointment-briefs.json` | Day 30 |
| NHS pathway appointment schedules | `data/appointments-singleton.json`, `data/appointments-twins.json` | Day 30 |
| Illustration brief or placeholder specification | Markdown document | Day 14 |
| Clinical sign-off on all integrated content | Entry in `CLINICAL_REVIEW_LOG.md` | Day 50 |

---

## 12. Claude Code implementation guide

This section provides specific instructions for Christian when using Claude Code to build Seed. It is written to be directly usable as context for the AI coding agent.

### 12.1 Repository initialisation

**Step 1: Create the GitHub repository.**

```bash
gh repo create seed-app --private --description "Seed: Privacy-first maternity toolkit for UK parents" --clone
cd seed-app
```

**Step 2: Initialise the Expo project.**

```bash
npx create-expo-app@latest . --template tabs
```

**Step 3: Install core dependencies (v2.2 — corrected list).**

```bash
# Core React Native + Expo (SDK 55 = RN 0.83 + React 19.2 + mandatory New Arch)
npx expo install expo-router react-native-mmkv zustand nativewind tailwindcss \
  react-native-reanimated react-native-safe-area-context react-native-screens \
  react-native-gesture-handler

# UI icons + SVG (illustrations as SVG per §8.4 v2.2)
npx expo install lucide-react-native react-native-svg

# Charts (gifted-charts OR victory-native-xl per §6.1 v2.2)
npx expo install react-native-gifted-charts expo-linear-gradient

# Expo modules used directly by feature spec
npx expo install expo-notifications expo-print expo-keep-awake expo-haptics \
  expo-linking expo-splash-screen expo-secure-store expo-crypto \
  expo-document-picker expo-file-system expo-sharing expo-build-properties

# IAP
npm install react-native-purchases@^10   # RevenueCat v10 (Play Billing 8)

# v2.2: zustand-mmkv-storage REMOVED (single-maintainer supply-chain risk).
# v2.2: @gluestack-ui/themed RETAINED for v1.0 with selective overrides per
#       §3.0a cut list; full custom primitives deferred to v1.0.x.
# v2.2: NO `buffer` npm package — use globalThis.btoa per SED-PRI-009.
# v2.2: NO `expo-updates` — EAS Update disabled per SED-ARCH-013.
```

**Step 4: Initialise NativeWind.**

```bash
npx tailwindcss init
```

Configure `tailwind.config.js`:
```javascript
module.exports = {
  content: ['./app/**/*.{ts,tsx}', './components/**/*.{ts,tsx}'],
  presets: [require('nativewind/preset')],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

**Step 5: Create the directory structure as specified in Section 6.6.**

**Step 6: Initialise Git and push.**

```bash
git add .
git commit -m "feat: initialise Expo project with core dependencies"
git push origin main
```

### 12.2 Git workflow

**Branch naming:** `feature/F01-due-date-calculator`, `feature/F03-contraction-timer`, `fix/weight-unit-conversion`, `content/week-12-data`.

**Commit messages:** Conventional commits format.
- `feat: add contraction timer with timestamp delta hook`
- `content: add week 4-12 JSON data`
- `fix: correct gestational age calculation for leap years`
- `style: apply Labour Mode dark theme to contraction timer`
- `docs: update CLINICAL_REVIEW_LOG with F02 sign-off`

**Branching model:** Feature branches from `main`. Merge via pull request. No direct commits to `main` after initial setup.

### 12.3 Prompting Claude Code

When starting a new feature, provide Claude Code with:

1. The relevant feature section from this PRD (e.g. Section 3.3 for F03).
2. The relevant store definition from Section 6.5.
3. The file structure context from Section 6.6 (which files to create/modify).
4. The colour palette from Section 6.7.
5. Any specific implementation patterns (e.g. timestamp delta hook from Section 6.4).

**Example prompt for Claude Code — F03 Contraction Timer:**

> Build the contraction timer feature (F03) for the Seed pregnancy app.
>
> Requirements: [paste SED-F03-001 through SED-F03-015 from Section 3.3]
>
> Store: Use the ContractionsState store defined in stores/contractionsStore.ts. [paste interface from Section 6.5]
>
> Timer implementation: Use the timestamp delta method. [paste hook code from Section 6.4]
>
> Files to create:
> - app/contraction-timer.tsx (main timer screen)
> - app/labour-mode.tsx (dark theme Labour Mode variant)
> - components/contraction/ContractionList.tsx
> - components/contraction/ContractionSummary.tsx
> - components/contraction/RollingAverages.tsx
>
> Styling: Use NativeWind with the colour palette defined in constants/colours.ts. [paste palette from Section 6.7]
>
> Key constraints:
> - Timer MUST use timestamp delta, NOT setInterval for elapsed time tracking.
> - Clinical banner text is: [paste from SED-F03-014]
> - The banner is NON-DISMISSIBLE.
> - The app MUST NOT interpret contraction data or suggest labour stage.
> - Labour Mode: dark theme (#1A1A2E background), minimum 120pt tap targets, haptic feedback, "keep screen on" toggle using expo-keep-awake.
> - "Call maternity unit" button uses expo-linking to open phone dialler.

### 12.4 Testing checklist per feature

Before merging any feature branch, verify:

- [ ] Feature works fully offline (aeroplane mode test)
- [ ] Data persists after app kill and restart (force close, reopen)
- [ ] Timer survives backgrounding (start timer, lock screen for 5 minutes, reopen, verify accuracy)
- [ ] Clinical signposting banner is present and non-dismissible
- [ ] Pro gate blocks access if Pro is not unlocked (for Pro features)
- [ ] Partner mode content shows/hides correctly based on setting
- [ ] Twin mode adjustments show/hide correctly based on setting
- [ ] Tracking pause/end state prevents feature access and notifications
- [ ] No prohibited terms in any user-facing text (see SED-SAF-005)
- [ ] Minimum 44pt tap targets on all interactive elements
- [ ] No network requests in any code path (verify with network inspector)

### 12.5 EAS Build configuration

```json
// eas.json (v2.1)
{
  "cli": { "version": ">= 16.0.0" },
  "build": {
    "development": {
      "developmentClient": true,
      "distribution": "internal"
    },
    "preview": {
      "distribution": "internal",
      "android": { "buildType": "apk" }
    },
    "production": {
      "autoIncrement": true,
      "android": {
        "buildType": "app-bundle"
      },
      "ios": {
        "autoIncrement": true
      }
    }
  },
  "submit": {
    "production": {
      "android": {
        "serviceAccountKeyPath": "./google-service-account.json",
        "track": "production",
        "releaseStatus": "completed"
      },
      "ios": {
        "appleId": "APPLE_ID_HERE",
        "ascAppId": "ASC_APP_ID_HERE",
        "appleTeamId": "TEAM_ID_HERE"
      }
    }
  }
}
```

> *v2.1 note:* `expo-updates` channel is intentionally omitted — EAS Update is disabled (SED-ARCH-013). The `./google-service-account.json` path MUST be in `.gitignore` and supplied via EAS secrets.

### 12.6 app.json configuration

```json
// app.json (v2.1)
{
  "expo": {
    "name": "Seed",
    "slug": "seed-maternity-toolkit",
    "version": "1.0.0",
    "orientation": "portrait",
    "icon": "./assets/icon.png",
    "newArchEnabled": true,
    "splash": {
      "image": "./assets/splash.png",
      "resizeMode": "contain",
      "backgroundColor": "#FDF8F4"
    },
    "userInterfaceStyle": "light",
    "assetBundlePatterns": ["**/*"],
    "ios": {
      "supportsTablet": true,
      "bundleIdentifier": "com.yourcompany.seed",
      "buildNumber": "1"
    },
    "android": {
      "adaptiveIcon": {
        "foregroundImage": "./assets/adaptive-icon.png",
        "backgroundColor": "#FDF8F4"
      },
      "package": "com.yourcompany.seed",
      "versionCode": 1,
      "permissions": ["POST_NOTIFICATIONS"]
    },
    "plugins": [
      "expo-router",
      "expo-notifications",
      "expo-secure-store",
      [
        "expo-build-properties",
        {
          "android": {
            "minSdkVersion": 26,
            "compileSdkVersion": 35,
            "targetSdkVersion": 35,
            "enableProguardInReleaseBuilds": true,
            "extraProguardRules": "-keep class com.facebook.hermes.** { *; }\n-keep class com.mrousavy.mmkv.** { *; }"
          },
          "ios": { "deploymentTarget": "16.0" }
        }
      ]
    ],
    "scheme": "seed"
  }
}
```

> **v2.1 changes vs v2.0:**
> - Removed `NSUserTrackingUsageDescription`. v2.0 included it with the text "This app does not track you" — but if the app never calls `requestTrackingAuthorization`, the key is contradictory and Apple Review may flag the inconsistency.
> - Removed the `react-native-mmkv` plugin entry. MMKV v3/v4 do not require a config plugin; `{ "accessGroups": [] }` was a no-op.
> - Added `newArchEnabled: true` (mandatory under SDK 55; explicit is clearer).
> - Added `expo-secure-store` plugin entry (required for SED-PRI-006 encryption-key bootstrap).
> - Added `expo-build-properties` plugin with NDK r27 / 16 KB page-size support, ProGuard rules for Hermes + MMKV, and explicit deployment targets.
> - Added `POST_NOTIFICATIONS` to Android permissions for Android 13+ runtime prompt.

### 12.7 Support operating model (new in v2.1)

v2.0 did not specify a support model. Both stores require a working developer contact; users will need refund triage, Pro restore help, and a way to report clinical-content errors.

**SED-SUP-001:** A single inbound support address `support@seed.health` (or final brand domain) MUST be configured before submission. It MUST resolve to a shared inbox (Google Workspace shared inbox or Fastmail) monitored at least once per working day. Personal accounts MUST NOT be used (bus-factor 1 is unacceptable).

**SED-SUP-002:** A static HTML support page MUST be hosted at `https://seed.health/support` containing: FAQ, common issues (data missing after reinstall, Pro not restored, notification not firing), contact form (Formspree/Basin/Netlify Forms — no PII storage beyond message body), and the support email.

**SED-SUP-003:** Triage SLAs: refund-related &lt;72 hours, clinical-content error &lt;24 hours, all other &lt;5 working days. CSO [Jon] owns clinical-error triage; CTO [Christian] owns technical.

**SED-SUP-004:** Manual Pro purchase verification process: requester supplies platform (Apple/Google), store receipt or RevenueCat anonymous app user ID (visible in app "About"), and approximate purchase date. Support replies after RevenueCat dashboard lookup. Documented in `SUPPORT_PLAYBOOK.md`.

**SED-SUP-005:** Three template responses MUST be drafted pre-launch and stored in `/docs/support-templates/`: "Pro restore", "Refund declined — direct to store", "Clinical content correction acknowledged".

**SED-SUP-006:** Special handler for pregnancy-loss-related correspondence: a single empathetic acknowledgement; no automated chasers; signposting to Sands / Tommy's / Miscarriage Association.

**SED-SUP-007:** Inbox MUST be a shared mailbox; the support playbook documents on-call coverage during holidays.

**SED-SUP-008:** Settings → "Help and support" opens a screen with: (a) link to support web page, (b) "Email us" button using `expo-linking` to `mailto:support@seed.health?subject=Seed%20support%20[v1.0.0]` with version, platform, and anonymous appUserId pre-filled in the body to aid triage, (c) clear note "We do not collect any data from this app. The information above is only filled in to help us help you, and you can delete it before sending."

### 12.8 QA strategy (new in v2.1)

v2.0 had a 12-line testing checklist; v2.1 specifies a full QA programme.

**SED-QA-001 — Unit testing:** Jest + `@testing-library/react-native`. Coverage targets: `utils/` ≥90%, `hooks/` ≥80%, `stores/` ≥80%. Date arithmetic (Naegele, gestational age, term window, leap year, DST/GMT crossover), weight conversion, rolling averages, and trend-direction computation MUST have explicit test cases including edge dates (29 Feb, BST/GMT crossover, year boundaries).

**SED-QA-002 — Content schema validation:** Every PR touching `data/*.json` runs `ajv` against schemas committed at `data/schemas/*.schema.json`. Build fails on schema drift. Includes URL liveness check (`scripts/validate-urls.js`).

**SED-QA-003 — Device matrix (minimum):**
- **Android:** Pixel 6a (mid-tier reference), Samsung Galaxy A14 (low-end, popular UK device), Samsung Galaxy S22 (premium), one tablet (Tab A8). OS versions: Android 8 (min), 12, 14, 15.
- **iOS:** iPhone SE 2nd gen (small screen, min spec), iPhone 12, iPhone 15, iPad (9th gen). iOS 16.0 (min), 17, 18.

**SED-QA-004 — Manual QA test plan:** `/docs/qa/test-plan-template.md` covering: onboarding paths × 6 combinations (singleton/twin × pregnant/partner/intended-parent × EDD/LMP/declined); each feature happy path + 2 edge cases; Labour Mode in airplane mode + screen lock for 10 minutes; kick counter export PDF rendering; birth-plan PDF rendering at A4 with 10+ selections; pregnancy-loss pathway full traversal (hide, pause, end, delete); backup → uninstall → install → restore round-trip.

**SED-QA-005 — Accessibility QA:** A TalkBack pass (Android) and VoiceOver pass (iOS) MUST be completed and logged before submission. Every interactive element MUST have `accessibilityLabel`, `accessibilityRole`, and `accessibilityHint` where action is non-obvious. Contrast ratios verified per SED-UX-017. Reduced-motion and Dynamic-Type passes documented.

**SED-QA-006 — UK English and prohibited-term audit:** spellcheck pass with en-GB dictionary; `scripts/audit-terms.js` scans all `.json` and `.tsx` for SED-SAF-005 banned terms; US-spelling check (color/colour, fetus/foetus where applicable per UK style).

**SED-QA-007 — Performance budgets:**
- Cold start to interactive: &lt;2.0s on Pixel 6a, &lt;2.5s on Galaxy A14.
- Warm start: &lt;500ms.
- MMKV synchronous read: &lt;2ms for any store.
- Chart render with 40 weight entries: &lt;300ms.
- Bundle size (compressed APK): &lt;30 MB; iOS IPA: &lt;50 MB.
- Memory ceiling: &lt;150 MB resident on Galaxy A14 (3 GB RAM device class).
- Labour Mode with `expo-keep-awake` + screen at 50% brightness: &lt;8% battery per hour on Pixel 6a.

**SED-QA-008 — Beta programme:** TestFlight (iOS, internal then external up to 50 testers from Mumsnet / NCT contacts) and Play Internal Testing track. Minimum 7-day external beta with at least 10 pregnant users before production submission.

**SED-QA-009 — Pre-submission audit checklist:** see Section 12.10.

### 12.9 Continuous integration (new in v2.1)

**SED-CI-001:** ESLint with `eslint-config-expo`, `@typescript-eslint`, and a custom rule banning the SED-SAF-005 prohibited terms in JSX text nodes. Config at `.eslintrc.cjs`.

**SED-CI-002:** Prettier with shared config; `husky` + `lint-staged` pre-commit hook running `eslint --fix`, `prettier --write`, `tsc --noEmit` on staged files.

**SED-CI-003:** GitHub Actions workflow `ci.yml` on every PR: install → lint → typecheck → unit tests → JSON schema validation → URL liveness check → bundle-size check (`expo export` + size diff comment). Required to pass before merge to `main`.

**SED-CI-004:** Dependency security: `npm audit --audit-level=high` runs in CI. Dependabot configured for weekly security updates.

**SED-CI-005:** Bundle size budget: fails CI if production bundle grows >10% versus `main`.

**SED-CI-006:** Secret scanning: GitHub native + a `gitleaks` pre-commit hook. The RevenueCat public SDK key is not a secret; service account JSON paths MUST be in `.gitignore` and supplied via EAS secrets.

**SED-CI-007:** Crash diagnostics without telemetry: a "Share diagnostics" button in Settings writes a structured stack trace + last-100 navigation events to `FileSystem.documentDirectory + 'crash-{timestamp}.txt'`; the user voluntarily shares via `expo-sharing`. Use `react-native-exception-handler` and Hermes stack traces. The app MUST NOT transmit any crash log automatically (SED-ARCH-003).

### 12.10 Pre-submission audit checklist (new in v2.1)

The single tick-list before submitting to either store. Every item is gating.

**Legal and policy:**
- [ ] Legal entity confirmed and registered as Play Console / App Store Connect publisher (SED-LEG-006)
- [ ] Trademark search complete; final brand name locked (SED-LEG-004)
- [ ] Domain registered; privacy policy live at public URL
- [ ] Terms of Service live at public URL (SED-LEG-001)
- [ ] Privacy policy in-app link works
- [ ] ICO registration paid and certificate filed (SED-PRIV-008)

**Clinical:**
- [ ] All entries in `CLINICAL_REVIEW_LOG.md` signed by [Jon] (SED-CLIN-002)
- [ ] Prohibited-term audit script passes 0 violations (SED-SAF-005 + exception list signed off)
- [ ] All Tier 1 source URLs verified accessible within 30 days of submission (`scripts/validate-urls.js`)
- [ ] Medical disclaimer text reviewed and signed off; `disclaimer_v` stamped
- [ ] Crisis-content footer present on every free-text input (SED-CC-020)
- [ ] Intended purpose statement (SED-SAF-001a) attached to App Store Connect reviewer notes

**Support:**
- [ ] `support@seed.health` provisioned, shared inbox, monitored
- [ ] Support web page live with contact form and FAQ
- [ ] Three support response templates drafted (SED-SUP-005)
- [ ] Support playbook (`SUPPORT_PLAYBOOK.md`) committed
- [ ] In-app "Help and support" entry point present and tested (SED-SUP-008)

**Purchases:**
- [ ] RevenueCat configured for both platforms
- [ ] iOS IAP set to Family Shareable (SED-REV-008)
- [ ] Apple Small Business Program enrolment confirmed; Google Play 15% tier confirmed (SED-REV-001a)
- [ ] "Restore purchases" button present on Pro screen AND Settings (SED-REV-007)
- [ ] "I already bought Pro" CTA tested on a clean device (SED-REV-015)
- [ ] Refund policy text published in-app and on store listings (SED-REV-006)

**QA:**
- [ ] Unit-test coverage ≥targets across `utils/`, `hooks/`, `stores/` (SED-QA-001)
- [ ] Manual QA test plan executed and signed off
- [ ] Device matrix smoke tests complete (4 Android, 3 iOS minimum)
- [ ] TalkBack + VoiceOver passes complete
- [ ] Performance budgets verified on Pixel 6a + Galaxy A14
- [ ] Bundle size under budget
- [ ] Beta cohort feedback addressed (minimum 10 external testers, 7-day window)

**CI/CD:**
- [ ] `ci.yml` green on `main`
- [ ] `npm audit` clean at high severity
- [ ] JSON schema validation enforced in CI
- [ ] Pre-commit hooks installed

**Edge cases verified (SED-EDGE-001 through 010):**
- [ ] All 10 SED-EDGE cases tested
- [ ] Tracking pause/end/hide/delete confirmations work (SED-CC-021 three-step)
- [ ] Backup export/import round-trips losslessly (SED-BAK-001/002)
- [ ] Data export (JSON + PDF) produces valid output (SED-PRIV-010/011)

**Release:**
- [ ] CHANGELOG.md entry written
- [ ] Store release notes written and CSO-reviewed
- [ ] Phased rollout plan documented and roles assigned (SED-REL-003)
- [ ] Rollback procedure rehearsed (dry run) (SED-REL-005)
- [ ] Bad-release detection routine assigned (morning store-review check) (SED-REL-004)

**Assets:**
- [ ] All assets per SED-ASSET-001 through 007 produced and reviewed
- [ ] Screenshots audited for prohibited terms

**Compliance:**
- [ ] Google Play Health Apps Declaration submitted (with mandated disclaimer text per SED-STORE-001)
- [ ] App Privacy nutrition label completed for iOS ("Data Not Collected") (SED-STORE-006)
- [ ] Content rating obtained
- [ ] In-app disclaimer gate verified working on first launch
- [ ] App Store Connect regulated-medical-device status set per SED-STORE-009

---

## 12b. New v2.1 sections: notifications, assets, edge cases, metrics, trademark

### 12b.1 Notification permission UX (new in v2.1)

**SED-F09-010:** Notification permission MUST NOT be requested at app launch. It is requested only when the user first creates an appointment with reminders enabled, or first opts in to pre-populate the NHS schedule.

**SED-F09-011:** Pre-permission priming screen: "To remind you of your appointments, Seed needs permission to send notifications. We never send promotional notifications or anything that requires the internet — only the reminders you set." Two buttons: "Enable reminders" → triggers system prompt; "Not now" → appointments still save without reminders.

**SED-F09-012:** Denied state: appointments save without reminders; UI shows "Reminders are off. Enable in Settings." with a tappable button calling `Linking.openSettings()`.

**SED-F09-013:** iOS provisional authorization is NOT used (would deliver quietly to Notification Center, contradicting user expectations).

**SED-F09-014:** Android 13+ POST_NOTIFICATIONS runtime permission handled identically.

**SED-F09-015:** Before scheduling any notification, the app MUST create a notification channel via `setNotificationChannelAsync('appointments', {...})`. Without this, Android 13+ will not show the runtime permission prompt.

### 12b.2 Asset specifications (new in v2.1)

**SED-ASSET-001:** App icon source: 1024×1024 PNG, sRGB, no transparency, no rounded corners (stores apply masks).

**SED-ASSET-002:** Android adaptive icon: foreground 432×432 within a 1024×1024 canvas with 264×264 safe zone; background colour `#FDF8F4`.

**SED-ASSET-003:** Splash: 1284×2778 PNG (largest iPhone) with logo centred within 512×512 safe zone; background `#FDF8F4`. Expo `splash.resizeMode: contain`.

**SED-ASSET-004:** Google Play assets: 512×512 hi-res icon; 1024×500 feature graphic; minimum 2 phone screenshots (1080×1920+); optional 7-inch tablet (1024×1800) and 10-inch (1080×1920) screenshots; optional 30-second YouTube preview.

**SED-ASSET-005:** App Store Connect assets: 1024×1024 marketing icon; 6.7" iPhone screenshots (1290×2796) minimum 3, recommend 5; 6.5" fallback (1242×2688); iPad 13" (2064×2752) if iPad supported; optional App Preview video (15–30 s).

**SED-ASSET-006:** Screenshot copy MUST be CSO-reviewed (no prohibited terms; no implied medical claims). Suggested set: (1) "Your week", (2) "Contraction timer — Labour Mode", (3) "Kick counter", (4) "Birth plan", (5) "Private. Offline. No ads."

**SED-ASSET-007:** App Preview video, if produced, MUST NOT use stock music with copyright claims and MUST NOT depict any clinical interpretation of data.

### 12b.3 Edge case taxonomy (new in v2.1)

**SED-EDGE-001 (Onboarding, future LMP):** If entered LMP > today, reject inline: "That date is in the future. Please check the date." Same field, no modal.

**SED-EDGE-002 (Onboarding, LMP >42 weeks ago):** Allow but show: "Based on this date, your due date has passed. If your baby has already been born, congratulations — Seed's postnatal features are not yet available." Offer "Update due date" or "Pause tracking".

**SED-EDGE-003 (Contraction timer crash recovery):** On launch, if `contractionsStore` has `isActive: true` for a session with no `endTimestamp`, show recovery prompt: "Your last session was not ended. Resume it, or end it now?" Recovery from MMKV is automatic because of synchronous persistence; no in-flight contraction is lost.

**SED-EDGE-004 (Kick counter, zero movement):** If user ends a session with 0 movements, save it as-is. Display: "Session saved with 0 movements." Do NOT highlight, colour-code, or warn — would breach SED-SAF-005 / SED-F04-007.

**SED-EDGE-005 (Weight tracker, sanity range):** Covered by SED-F05-009.

**SED-EDGE-006 (Birth plan PDF, out of storage):** Catch `expo-print` error; surface "Could not save PDF — your device may be low on storage. Free up some space and try again." Offer "Share" as alternative (in-memory).

**SED-EDGE-007 (Appointment notification scheduled in the past):** `expo-notifications` will not fire past notifications. On scheduling, if `date+time < now`, refuse with inline message. If the scheduled time elapses while the device is off, the notification does NOT fire (system behaviour). On app foreground, check for any appointments whose reminder time elapsed in the last 24h and surface a non-modal banner: "You had an appointment reminder for [title] — did you attend?" with dismiss.

**SED-EDGE-008 (MMKV corruption):** Wrap MMKV reads at app launch in try/catch. On exception, preserve the encrypted file as `seed-storage.corrupt.bak`, initialise a fresh store, show a one-time modal: "Seed could not read your saved data. We have started fresh. If you need help recovering, contact support@seed.health." Provide the backup file path via "Settings → Export diagnostics".

**SED-EDGE-009 (Payment success, RevenueCat sync fail):** Documented under SED-REV-010.

**SED-EDGE-010 (Disclaimer accepted, app upgraded with new disclaimer text):** Version the disclaimer (`disclaimer_v` integer). On version mismatch at launch, re-prompt the user with the new text. Trivial typo fixes MAY skip re-prompting at CSO discretion (SED-SAF-007a).

### 12b.4 Legal entity and trademark (new in v2.1)

**SED-LEG-004:** UK trademark search via IPO TM database (https://www.gov.uk/search-for-trademark) for "Seed" in Class 9 (mobile apps) and Class 44 (medical / health services). If clashes exist (likely — "Seed" is common), select alternative (suggestions: "Sprout", "Bloom UK", "Kin Maternity"). Decision required by day 30. CSO + CTO sign-off.

**SED-LEG-005:** Domain `seed.health` (or final brand) registered and pointed at static support / privacy / terms pages.

**SED-LEG-006:** Publisher name on stores: the company's registered legal entity name (not "Seed"). App display name and store listing can differ from publisher.

**SED-LEG-007:** ICO registration (SED-PRIV-008) MUST list the same legal entity as publisher.

**SED-LEG-008:** Companies House registration confirmed; VAT registration unnecessary at projected revenue but flagged for review if revenue exceeds £85k.

### 12b.5 Tax, accounting, and insurance (new in v2.2)

**SED-LEG-012 (new in v2.2 — insurance):** Pre-launch, the company MUST hold: (a) **professional indemnity** (clinical content errors) — typical UK indie health-tech £500k–£1m cover, ~£400–£900/yr; (b) **cyber liability** (RevenueCat breach exposure, support inbox compromise) — £250k cover ~£300–£600/yr; (c) **public liability** standard SME cover ~£100–£250/yr; (d) **product liability** — bundled with PI in most policies. Estimated total: £800–£2,500/yr. Brokers: Hiscox, Markel, PolicyBee. Document in `/docs/legal/insurance.md`.

**SED-FIN-001 (new in v2.2 — tax and accounting):** Pre-launch: (a) HMRC corporation tax registration (auto on Companies House incorporation); (b) accountant engaged — Xero or FreeAgent for bookkeeping; (c) **R&D tax credits** — Seed's clinical-content engineering, accessibility work, and privacy architecture likely qualify under HMRC's RDEC scheme for digital health; (d) if seeking external funding, **SEIS/EIS advance assurance** filed with HMRC before any share issuance; (e) VAT not required at projected revenue (under £85k) but reviewed at year-end. Document in `/docs/finance/`.

### 12b.6 User communication channels without telemetry (new in v2.2)

**SED-COMM-001 (new in v2.2 — What's-new screen):** On first launch after an update where `app.version` differs from the persisted `lastSeenVersion`, present a single screen titled "What's new in Seed v{version}". Copy sourced from `data/release-notes.en-GB.json`. ≤3 bullets, CSO-reviewed if any clinical-content update is mentioned. Skippable after 3 seconds with a "Got it" button.

**SED-COMM-002 (new in v2.2 — store promotional text):** App Store "Promotional text" (170 chars) and Play Store "What's new" (500 chars) are the only channels for reaching users without a code change. Reserve App Store promotional text for: (a) seasonal NHS guidance reminders, (b) acknowledging known issues. Never marketing. Owner: [Christian] writes; [Jon] approves if content-related.

**SED-COMM-003 (new in v2.2 — changelog feed):** Maintain `https://seed.health/changelog` as a chronological human-readable list. Atom/RSS feed at `/changelog.xml` for opt-in subscribers. Canonical channel for security-fix announcements that the offline-only architecture has no way to push.

**SED-COMM-004 (new in v2.2 — security advisory channel):** Critical security fixes MUST be announced via: (a) the changelog (SED-COMM-003), (b) the status page (SED-OBS-005), (c) App Store / Play Store release notes, (d) a dated entry in `CHANGELOG.md` under "Security". The "What's new" screen MUST mention the fix without exposing exploit detail.

**SED-COMM-005 (new in v2.2 — privacy-policy versioning):** Bundle `policy_v: integer` with each release. On app launch, if `settingsStore.policyAcknowledgedV < bundledPolicyV`, show a non-blocking banner: "Our privacy policy has been updated. [Review changes]". Tap dismisses; opens the policy URL. Treat as a UX surface, not a gate — the disclaimer is the consent surface.

### 12.11 Incident response (new in v2.2)

**SED-INC-001 (new in v2.2 — RevenueCat breach):** Although Seed holds no user health data, RevenueCat processes anonymous app user IDs and purchase receipts. If RevenueCat notifies of a breach affecting Seed customers, the company MUST: (a) confirm exposure within 24 hours via the RevenueCat dashboard; (b) determine whether the UK GDPR 72-hour ICO notification clock applies; (c) post a status update at `https://seed.health/security` within 24 hours; (d) email affected users only if RevenueCat supplies an identifiable channel (none in v1.0 — so the status page IS the channel); (e) file the ICO notification at the company's discretion with solicitor sign-off. Owner: [Christian]. Escalation: [Jon] for any clinical-impact dimension. Maintain `/docs/runbooks/incident.md` with this playbook.

**SED-INC-002 (new in v2.2 — security vulnerability disclosure):** A `SECURITY.md` at repo root MUST publish a coordinated-disclosure policy: (a) report channel `security@seed.health` (separate alias from support); (b) PGP key published; (c) response SLA — acknowledgement within 72 hours, triage within 7 days, fix or mitigation roadmap within 30 days for high-severity; (d) safe-harbour statement for good-faith researchers; (e) credit policy. Publish at `https://seed.health/.well-known/security.txt` per RFC 9116 with `Contact: mailto:security@seed.health`, `Expires:` refreshed every 11 months, `Preferred-Languages: en`, `Policy: https://seed.health/security/policy`.

**SED-INC-003 (new in v2.2 — store-suspension contingency):** If Apple or Google takes Seed down: (a) within 4 hours, publish an explanatory post at `https://seed.health/status` (existing users keep the app and Pro entitlement); (b) within 24 hours, file the appeal/review request through the store's developer portal; (c) within 48 hours, post a CSO statement re-affirming clinical position if the takedown cites health-content concerns; (d) maintain a one-page "Restoration steps" runbook noting the previous-good `v-stable-` tag, IPO/Companies House documents, the clinical-review log, and the intended-purpose statement (SED-SAF-001a) all immediately retrievable.

**SED-INC-004 (new in v2.2 — purchase outage):** If StoreKit / Play Billing is degraded during launch, the in-app paywall MUST display: "Pro isn't available to purchase right now. This is a problem with the app store, not Seed. Your existing features still work. Try again in a few hours." Detection: a `purchases-unavailable` error code from RevenueCat.

**SED-INC-005 (new in v2.2 — incident log):** All incidents MUST produce a public-facing post-mortem at `https://seed.health/status` within 14 days, in the lightweight Google SRE format (timeline, root cause, what went well, what didn't, action items).

### 12.12 Service health and observability (new in v2.2)

**SED-OBS-001:** UptimeRobot or BetterStack MUST monitor at 5-minute intervals: `seed.health` (root), `/privacy`, `/terms`, `/support`, `/security`, `/status`. SLA: page-down for >15 minutes triggers email + SMS to [Christian]. Free tier sufficient.

**SED-OBS-002:** Apple App Store Connect exposes a public per-app RSS feed for customer reviews. Wire to Feedly/Inoreader monitored daily. Google Play Console supports email notifications for new reviews — enable for all severity. Document in `/docs/runbooks/store-review-response.md`.

**SED-OBS-003:** Configure the `support@seed.health` shared inbox with: (a) auto-acknowledgement on receipt; (b) follow-up rule that escalates any unread thread >72 hours old to [Christian]'s personal inbox; (c) weekly "unactioned" sweep.

**SED-OBS-004:** Daily for first 30 days post-launch, then weekly: review the RevenueCat "Charts" tab for purchase-success rate, restore-success rate, and refund rate. Threshold for alarm: restore-success <90%, refund rate >5%.

**SED-OBS-005:** A simple static page at `https://seed.health/status` MUST publish operational state. Two states: "All systems normal" and "Investigating an issue". Maintained manually. Updated within 1 hour of any user-facing incident.

### 12.13 Tooling inventory (new in v2.2)

Table of every dev/QA tool referenced anywhere in the PRD, with link and config-file path. Reproduced from research/05 and research/06 reports.

| Need | Tool | Config path |
|---|---|---|
| Content readability | `vale` with Hemingway-grade-6 rule set | `.vale.ini` + `styles/Hemingway/` |
| Prohibited-term audit | `scripts/audit-terms.js` + ESLint custom rule | `eslint-plugin-seed-terms/` |
| URL liveness | `scripts/validate-urls.js` using `undici` HEAD | `package.json` script |
| JSON schema validation | `ajv` | `data/schemas/*.schema.json` |
| Network-call verification (zero network) | mitmproxy on workstation; `__DEV__` guard on `global.fetch` | `app/_layout.tsx` |
| iOS a11y audit | Xcode Accessibility Inspector | manual |
| Android a11y audit | Accessibility Scanner (Play Store app) | manual |
| Bundle size tracking | `expo export` + `bundlesize` | `bundlesize.config.json` |
| Cold-start measurement | `react-native-startup-time` (on-device console-logged only — no telemetry) | dev only |
| Memory profiling | Xcode Instruments / Android Studio Profiler | manual |
| RevenueCat testing | RevenueCat sandbox + `__DEV__` purchase-mock toggle | `app/_layout.tsx` |
| Unit tests | Jest + `@testing-library/react-native` + `jest-expo` 55.x preset | `jest.config.js`, `jest.setup.ts` |
| TypeScript strict | `tsc --noEmit` with `"strict": true`, `"noUncheckedIndexedAccess": true`, `"exactOptionalPropertyTypes": true` | `tsconfig.json` |
| Pre-commit hooks | `husky` + `lint-staged` | `.husky/`, `package.json` |
| Secret scanning | `gitleaks` pre-commit + GitHub native | `.gitleaks.toml` |
| Backup encryption | `react-native-aes-gcm-crypto` (Tectiv3/Craftzdog) | `package.json` |

**SED-TOOL-001 (new in v2.2):** Each tool listed above MUST be installed and its config file committed before the corresponding QA gate (SED-QA-001 unit tests, SED-CI-* CI checks, SED-QA-005 a11y audit) can pass.

### 12.14 Dependency version pinning (new in v2.2)

**SED-ARCH-019 (new in v2.2):** All dependencies in `package.json` MUST use exact versions or `~patch` ranges only. No `^` caret ranges. This prevents Expo SDK 55.x.y unintended bumps between submission and launch. Renovate/Dependabot configured for weekly minor-version PRs reviewed manually. Pin Node version via `.nvmrc` (Node 20.x LTS) and `engines` field in package.json.

**SED-ARCH-020 (new in v2.2 — RevenueCat Paywalls disabled):** RevenueCat's remote-paywall feature (`Paywalls` v2) is explicitly disabled in the Seed RevenueCat project configuration. The in-app paywall is rendered entirely from local code per SED-ARCH-003 and SED-ARCH-013. Verify by inspecting the RevenueCat dashboard "Paywalls" tab is empty and the `react-native-purchases-ui` package is NOT installed.

**SED-ARCH-021 (new in v2.2 — RevenueCat Targeting and Experiments disabled):** RevenueCat Targeting (cohort rules) and Experiments (A/B) are not used. The RevenueCat dashboard MUST show no active experiments at submission.

### 11.4 Brand-rename SOP (new in v2.2)

"Seed" is a high-collision word in IPO Class 9. If trademark clearance fails on day 30, the cost of renaming late is high — every UI string, app icon, store listings, contracts, privacy policy, and support page change. This SOP minimises that cost.

**SED-LEG-013 (new in v2.2):** All app-name strings MUST live in `data/copy.en-GB.json` referenced via a `{appName}` token. Renaming = JSON edit + asset regeneration. No hard-coded "Seed" string in any other file.

**SED-LEG-014 (new in v2.2):** Reserve TWO bundle identifiers at Apple and Google before day 1: the primary (`com.seedhealth.app`) and a fallback (`com.{fallback}.app` — choose from the trademark-shortlist alternatives). If the rename fires, the fallback bundle is already provisioned.

**SED-LEG-015 (new in v2.2):** All git directory names use `seed/` and remain unchanged on rename — only the publisher-visible name changes. Bundle identifier change is the most painful technical step (RevenueCat product IDs are bundle-scoped; promo codes must be regenerated).

### 12.15 Documentation and bus-factor mitigation (new in v2.2)

**SED-DOC-001 (new in v2.2):** Repo MUST contain `/docs/adr/` with one Architecture Decision Record per major decision: (a) MMKV vs SQLite for persistence, (b) Zustand vs Redux for state, (c) Expo Router vs React Navigation, (d) no Gluestack as design system, (e) on-device-only / no telemetry, (f) synchronous encryption-key bootstrap. Each ADR follows the Michael Nygard template (context / decision / consequences).

**SED-DOC-002 (new in v2.2):** Repo MUST contain `CONTRIBUTING.md` with environment setup, branch naming, PR template, review SLA. `docs/cookbook.md` for repeating tasks: adding a new week entry, adding a new birth-plan section, regenerating illustrations, bumping disclaimer version. `docs/architecture.png` (mermaid) one-page architecture diagram.

**SED-DOC-003 (new in v2.2):** Documentation cadence: (a) PRD revised on every MINOR release or every 6 months whichever sooner; (b) `CHANGELOG.md` updated per release; (c) runbooks reviewed quarterly with a dated entry; (d) `research/` updated when material new evidence emerges. Owner: [Christian] for technical docs; [Jon] for clinical and `CLINICAL_REVIEW_LOG`. `RUNBOOKS_INDEX.md` at `/docs/runbooks/` is the table of contents for all runbooks.

---

## 13. Go-to-market

### 13.1 App Store Optimisation (ASO)

**Target keywords:** "pregnancy tracker UK", "NHS pregnancy app", "contraction timer UK", "kick counter", "pregnancy toolkit", "offline pregnancy app", "private pregnancy app", "no ads pregnancy", "UK maternity app", "pregnancy checklist UK".

**SED-GTM-001:** App title: "Seed: UK Pregnancy Toolkit". Subtitle (iOS) / Short description (Android): "Private, offline, NHS-aligned. No ads, no tracking."

**SED-GTM-002:** The listing MUST NOT claim NHS endorsement or use the NHS logo. The phrase "NHS-aligned" indicates content sourced from NHS guidance, not official NHS affiliation.

### 13.2 Organic marketing (zero budget)

- **Reddit:** r/BabySeeds, r/pregnant, r/UKParenting, r/PregnancyUK, r/BabySeedsUK. Authentic engagement, sharing the indie developer / clinical safety story and privacy-first positioning. Not spam.
- **Mumsnet:** Organic participation in pregnancy app recommendation threads.
- **LinkedIn:** Professional updates targeting NHS digital health, maternity services, and health tech audiences. Focus on the clinical standards story: "We built a pregnancy app that takes clinical quality seriously, even though it is not a medical device."
- **Product Hunt:** Launch for initial visibility burst.
- **Twins Trust / multiples communities:** Direct outreach highlighting the twin/multiple pregnancy support, which is a genuine unmet need.

### 13.3 Differentiation narrative

The marketing message is not "another pregnancy tracker." It is: **"The pregnancy app that respects you."**

Core pillars:
1. **Privacy as architecture, not a promise.** Your data is encrypted on your phone. It never touches a server. We cannot see it even if we wanted to. *(Strengthened by the **September 2025 Flo + Google $56M class-action settlement** over SDK data leakage and the 2021 FTC Flo settlement — concrete evidence that "privacy promises" from competitors do not hold.)*
2. **Built for the UK.** NHS pathways, midwife-first language, stones and pounds, your antenatal schedule.
3. **Designed for reality.** Partner mode (inclusive of same-sex couples, solo parents, surrogacy intended parents). Twin support because you deserve more than an afterthought. A pregnancy-loss pathway with ARC, Ectopic Pregnancy Trust, Petals, Birth Trauma Association, and Twins Trust bereavement signposting — because not every pregnancy ends as hoped.
4. **Clinical quality without being a medical device.** Every piece of content reviewed by a senior NHS clinician. Every disclaimer honest and clear. The MHRA "intended purpose" statement (SED-SAF-001a) is publicly available on request.

### 13.4 Closest UK competitor — Baby Buddy (now Babyzone)

Baby Buddy is the only major UK-first, NHS-aligned, ad-free pregnancy app. Seed's differentiation against Baby Buddy is the **Pro toolkit** (contraction timer with Labour Mode, kick counter, birth-plan PDF, weight chart, appointment briefs) and the **offline / no-account architecture**. A store-page row "vs. Baby Buddy" is **deliberately omitted** from marketing — Baby Buddy is a charity-led product and direct comparison is bad form. The strategic position is complementary, not competitive.

### 13.5 ASO target keywords (v2.1 — refreshed from competitive research)

| Keyword cluster | Specific terms |
|---|---|
| Core utility | "pregnancy tracker UK", "due date calculator", "contraction timer UK", "labour timer", "kick counter", "kick counter UK" |
| Privacy / ad-free positioning | "pregnancy app no subscription", "pregnancy app no ads", "private pregnancy tracker", "offline pregnancy app" |
| UK-specific | "NHS pregnancy app", "midwife appointments", "antenatal appointments", "pregnancy weight tracker stones", "birth plan template UK", "NHS birth plan", "hospital bag checklist UK", "baby names UK", "ONS baby names" |
| Underserved | "twin pregnancy app UK", "multiples pregnancy", "partner pregnancy app" |

**Subtitle suggestion (iOS, 30 chars):** "Private, offline. No ads." (25 chars).

### 13.6 Press kit (new in v2.2)

**SED-GTM-003 (new in v2.2):** Pre-launch the company MUST publish at `https://seed.health/press`:
- High-res logo (SVG + 1024 px PNG, light and dark variants)
- Founder headshots ([Jon], [Christian]) with short bios
- 6 product screenshots at App Store 6.7" resolution
- One-page fact sheet (founding, team, what Seed is, what it isn't, regulatory position per SED-SAF-001a)
- "Privacy architecture" technical whitepaper (2-page PDF) derived from §6.2 of PRD and the 2025 Flo settlement context
- Boilerplate paragraphs (1-line, 3-line, 5-line, 100-word)
- Press contact `press@seed.health`

Whitepaper CSO-reviewed per SED-CLIN-001.

---

## 14. Success metrics

v2.1 adds an explicit measurement source for each metric — required because SED-ARCH-003 prohibits all analytics SDKs.

| Metric | Target (year one) | Measurement source (no telemetry) |
|---|---|---|
| Google Play downloads (free) | 15,000+ | Play Console "Acquisition" (no SDK needed) |
| iOS downloads | 5,000+ (first 6 months post-launch) | App Store Connect "Sales and Trends" |
| Pro conversion rate | 5%+ (note: median freemium ~2.18% — this is an aggressive target) | RevenueCat dashboard (purchases ÷ first-seen anonymous app user IDs) |
| Net revenue | £3,000+ (v2.2 — implies ~17,000 downloads at 5% conversion at £3.53 net per sale, OR 15,000 at 5.7% conversion, OR 12,000 at 7%) | RevenueCat dashboard "Revenue", reconciled monthly against Play Console / App Store Connect financial reports |
| Play Store rating | 4.5+ stars | Polled manually weekly; Play Console new-review notifications enabled |
| App Store rating | 4.5+ stars | App Store Connect ratings panel |
| Clinical content accuracy | 100% | CSO review log — pass rate of internal audits (SED-CLIN-002) |
| Pregnancy-loss complaints | 0 | Store review keyword scan + support inbox tag |
| Privacy / data complaints | 0 | Store review keyword scan + support inbox tag |
| Prohibited-term violations | 0 | Pre-release audit script (`scripts/audit-terms.js`) + CI check |
| Crash rate | &lt;1% sessions (proxy: user-reported, since no Crashlytics) | Support inbox + manual smoke tests (SED-REL-006) |
| Restore-purchase success rate | &gt;95% within 30s | RevenueCat dashboard "Restore" events |

**SED-MET-001:** A monthly metrics review meeting MUST happen on the first Monday: review Play/App Store/RevenueCat dashboards, store reviews, support inbox themes. Logged in `/docs/metrics/YYYY-MM.md`.

---

## 15. Risk register

| ID | Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|---|
| R01 | Low conversion rate (<3%) | Medium | Medium | A/B test price points. Consider £2.99 fallback. Increase free tier value to drive downloads. |
| R02 | Illustration costs exceed budget | Low | Medium | Use licensed set or placeholder vectors for v1.0. Commission full set for v1.1. |
| R03 | Google Play rejects health app declaration | Low | High | Pre-draft declaration. Architecture is strictly offline. Disclaimer covers all requirements. |
| R04 | Feature interpreted as medical device by MHRA | Low | Critical | Maintain bright line: no interpretation, no thresholds, no alerts based on data. CSO review of all features and text. |
| R05 | Competitor launches privacy-first UK app | Low | Medium | Speed to market. Clinical quality story provides long-term differentiation. |
| R06 | UK GDPR enforcement action | Very low | High | Offline architecture minimises risk. ICO registration. Data is user-controlled and deletable. |
| R07 | Content inaccuracy in weekly tracker | Low | High | All content CSO-reviewed and sourced from Tier 1 UK authorities. Version-controlled JSON with audit trail. |
| R08 | RevenueCat service disruption blocks purchases | Low | Medium | Offline entitlement caching means existing Pro users are unaffected. New purchases delayed until service restores. |
| R09 | React Native / Expo breaking changes | Low | Medium | Pin dependency versions. Test on preview builds before SDK upgrades. |
| R10 | App store listing rejected for health claims | Low | Medium | Use approved medical disclaimer language. Do not claim health outcomes. |
| R11 | "Postnatal toolkit" claim in v2.0 unsupported by v1.0 feature set | High | Medium | v2.1 strikes "postnatal" from §1.1 and store copy until v1.1 F11 ships (see Section 18). |
| R12 | WCAG 2.2 conformance not yet measured | Medium | Medium | Scheduled accessibility audit pre-launch with TalkBack + VoiceOver passes (SED-QA-005); contrast palette corrected in v2.1 §6.7. |
| R13 | Hard-coded MMKV encryption key in v2.0 source compromises every user's data | Closed | — | **Closed in v2.1** via SED-PRI-006; **further hardened in v2.2** via SED-PRI-006/008/009 (synchronous bootstrap, `WHEN_UNLOCKED_THIS_DEVICE_ONLY` Keychain, `globalThis.btoa` not `Buffer`). Pre-submission audit confirms no literal key in bundle. |
| R14 | Timer hook in v2.0 wrote to unencrypted unnamed MMKV instance | Closed | — | **Closed in v2.1** via SED-ARCH-016 (pass named instance). |
| R15 | EAS Update / OTA ambiguity in v2.0 left clinical review cadence undefined | Closed | — | **Closed in v2.1** — SED-ARCH-013 disables EAS Update for v1.0; `expo-updates` not installed. |
| R16 | v2.1 encryption-key bootstrap (Proxy + async `initMMKV`) would crash app at first launch because Zustand persist hydrates synchronously | Closed | — | **Closed in v2.2** via SED-PRI-008 (synchronous bootstrap using `SecureStore.getItem` + `Crypto.getRandomBytes`). |
| R17 | v2.1 pricing math claimed £4.16 net "after VAT and commission"; it was VAT-net only. Real net is ≈£3.53. Target scenario fell below £3k success metric | Closed | — | **Closed in v2.2** via corrected SED-REV-001 and §14 success-metric update. |
| R18 | v2.1 tagged-PDF requirement (SED-F07-009) is physically infeasible with on-device `expo-print` | Closed | — | **Closed in v2.2** — SED-F07-009 downgraded to semantic HTML + plain-text alternative (SED-F07-010 new); tagged PDF deferred to v1.0.x or v1.1. |
| R16 | Cross-device Pro restore confusing without an account | High | Medium | **Mitigated in v2.1** — "I already bought Pro" CTA on onboarding and Pro screen (SED-REV-015); FAQ documents new-device journey. |
| R17 | Trademark "Seed" likely clashes in Class 9 / 44 | Medium | High | Trademark search by day 30 (SED-LEG-004); alternatives prepared. |
| R18 | ICO Children's Code update (Data Use & Access Act 2025 review) lands during build | Medium | Medium | 30-day pre-submission re-check of ICO Children's Code page (Risk-owner: Jon). |
| R19 | Source URLs (NHS Best Start in Life, RCOG GTG-57 2nd ed.) change mid-build | Medium | Medium | `scripts/validate-urls.js` runs in CI; CSO content-provenance log (SED-CLIN-003) drives re-verification. |
| R20 | Major competitor (Flo, Pregnancy+) launches privacy-first response | Low | Medium | The 2025 Flo settlement makes such a pivot costly and slow; Seed's architectural advantage (no server) is hard to replicate without rebuild. Maintain speed-to-market. |
| R21 | CSO ([Jon]) unavailable for emergency clinical correction (illness, leave, departure) | Medium | Critical | **SED-CLIN-006** deputy CSO on retainer; named, GMC/NMC PIN logged; reachable within 24 hours; quarterly review of arrangement. |
| R22 | RevenueCat data breach exposes purchase metadata | Low | Medium | **SED-INC-001 / SED-PRIV-017** playbook; status page within 24 hours; ICO notification per UK GDPR 72-hour clock if triggered; DPA in place. |
| R23 | External security researcher reports a vulnerability with no response channel | Low | High | **SED-INC-002** `SECURITY.md` and `security.txt`; 72-hour acknowledgement SLA; PGP key published. |
| R24 | Apple or Google removes app from store mid-launch | Low | Critical | **SED-INC-003** contingency; previous-good `v-stable-` tag; appeal within 24 hours. |
| R25 | Public web property at `seed.health` goes down breaking in-app privacy-policy link | Medium | Medium | **SED-OBS-001** synthetic monitoring at 5-minute intervals. |
| R26 | Support inbox missed; user complaint escalates to ICO or store review | Low | High | **SED-OBS-003** auto-ack + 72-hour escalation rule; weekly unactioned sweep. |
| R27 | OSS licence non-compliance triggers Apple/Google review rejection | Low | Medium | **SED-LEG-009** acknowledgements screen + `npm-licenses` build step. |
| R28 | DSAR received with no SOP, response missed within 30-day UK GDPR window | Low | High | **SED-PRIV-013** SOP; template at `/docs/support-templates/dsar-response.md`. |
| R29 | Children's-Code / child-on-shared-device incident reported to ICO | Low | Medium | Neutral iconography; pregnancy-loss pathway 2-tap minimum; crisis footer ubiquitous; **SED-PRIV-018** monitoring of post-DUAA Children's Code guidance. |
| R30 | RevenueCat US transfer mechanism (DPF) revoked or invalidated by ECJ-style ruling | Low | Medium | **SED-PRIV-015** transfer mechanism documented; Standard Contractual Clauses as fallback. |
| R31 | Insurance gap — clinical-content claim with no professional indemnity cover | Low | Critical | **SED-LEG-012** PI + cyber + public + product liability in force pre-launch. |
| R32 | Company end-of-life leaves users without app updates or restore | Low | Medium | **SED-PRIV-019** commitment; final-update mechanism; 30-day notice. |
| R33 | Trademark "Seed" likely clashes in Class 9 / 44 (IPO TM database high-collision) | Medium | High | **SED-LEG-013/014/015** brand-rename SOP; primary + fallback bundle IDs reserved before day 1; copy uses `{appName}` token throughout. |
| R34 | 60-day timeline unachievable for full v2.1 scope (~81-111 dev-day realistic envelope) | High | High | **§3.0a v1.0 cut list** defers F10, SED-BAK, custom UI primitives, tagged PDF, crash-diagnostics share, multi-language refactor to v1.0.x or v1.1, saving ~15-23 dev-days. |
| R35 | v2.1 encryption-key bootstrap (Proxy + async `initMMKV`) crashes app at first launch | Closed | — | **Closed in v2.2** via SED-PRI-008 synchronous bootstrap. |
| R36 | v2.1 pricing math (£4.16) inflated revenue projections by ~15% | Closed | — | **Closed in v2.2** corrected to £3.53 net; §14 success target updated. |
| R37 | v2.1 prohibited terms ("recommends", "harmless", "normal", "you should") slipped into v2.1 content edits | Closed | — | **Closed in v2.2** — all four occurrences fixed; SED-SAF-005 exception list (§5.2a) formalises legitimate Tier-1-quoted uses. |
| R38 | v2.1 tagged-PDF requirement (SED-F07-009) physically infeasible with on-device `expo-print` | Closed | — | **Closed in v2.2** — SED-F07-009 downgraded to semantic HTML + plain-text alternative (SED-F07-009a). |

---

## 16. Appendix A: Static data schemas

### A.1 weeks.json — complete schema

```json
[
  {
    "week": 4,
    "baby_size_comparison": "Poppy seed",
    "baby_length_mm": 1,
    "baby_weight_g": null,
    "development_points": [
      "The fertilised egg has implanted in your womb.",
      "The cells are dividing rapidly to form the embryo.",
      "The placenta is beginning to develop."
    ],
    "maternal_changes": [
      "You may not know you are pregnant yet.",
      "Some women notice light spotting (implantation bleeding).",
      "You may feel tired or notice breast tenderness."
    ],
    "midwife_questions": [
      "If you think you might be pregnant, take a home pregnancy test.",
      "Start taking 400 micrograms of folic acid daily if you are not already."
    ],
    "key_appointments": null,
    "partner_content": {
      "this_week": "Your partner may not know they are pregnant yet, or may have just found out. This is a time of big emotions.",
      "how_to_help": "Be supportive and patient. If they are feeling anxious, listen without trying to fix things."
    },
    "twin_adjustments": null,
    "nhs_link": "https://www.nhs.uk/best-start-in-life/pregnancy/week-by-week-guide-to-pregnancy/1st-trimester/week-4/",
    "illustration_key": "week_04_poppy_seed"
  }
]
```

This schema repeats for all 42 weeks (4-42, plus weeks 1-3 as a "before you knew" informational entry if desired). [Jon] is responsible for populating all clinical content fields. [Christian] builds the UI to render whatever data is present, gracefully handling null fields.

**v2.2 null-field convention (additions to A.1):**

| Field type | Convention | Renderer behaviour |
|---|---|---|
| Optional object (`partner_content`, `twin_adjustments`, `key_appointments`) | Always present in the JSON; set to `null` when the week has no data | Renderer: `if (data.partner_content && userMode === 'partner_companion')` etc. |
| Optional array (`development_points`, `maternal_changes`, `midwife_questions`) | Always present; empty `[]` when no entries | Renderer iterates; empty array → section omitted |
| Optional numeric (`baby_weight_g`) | Always present; `null` when too early | Renderer hides the weight chip when null |
| String (`illustration_key`) | Always present; `null` if illustration not yet commissioned | Renderer shows placeholder icon when null |

This means `weeks.json` schema is fully populated for every week; no fields are *absent*. Schema validator (SED-QA-002) enforces.

### A.2 hospital-bag-hospital.json — example structure

```json
[
  {
    "id": "hb-labour-01",
    "category": "For you during labour",
    "label": "Your birth plan",
    "checked": false,
    "isCustom": false,
    "birthSettingFilter": ["hospital", "birth_centre"]
  },
  {
    "id": "hb-labour-02",
    "category": "For you during labour",
    "label": "Comfortable old t-shirt or nightdress",
    "checked": false,
    "isCustom": false,
    "birthSettingFilter": ["hospital", "birth_centre", "caesarean"]
  },
  {
    "id": "hb-caesar-01",
    "category": "For you during labour",
    "label": "Loose high-waisted underwear and clothing (comfortable over your wound)",
    "checked": false,
    "isCustom": false,
    "birthSettingFilter": ["caesarean"]
  }
]
```

### A.3 birth-plan-options.json — example section

```json
{
  "sections": [
    {
      "id": "pain-relief",
      "title": "Pain relief",
      "type": "multi_select",
      "intro": "You can change your mind at any time during labour. This records your current preferences to discuss with your midwife.",
      "options": [
        {
          "id": "pr-breathing",
          "label": "Breathing and relaxation techniques",
          "evidence": "Many women find breathing techniques helpful in early labour. Your antenatal classes will cover these."
        },
        {
          "id": "pr-water",
          "label": "Water (birth pool or bath)",
          "evidence": "Warm water can help with pain relief in labour. NICE recommends it as an option for pain management."
        },
        {
          "id": "pr-tens",
          "label": "TENS machine",
          "evidence": "TENS is most effective in early labour. You can hire or buy a TENS machine designed for labour."
        },
        {
          "id": "pr-gas",
          "label": "Gas and air (Entonox)",
          "evidence": "Entonox is available in all birth settings. It takes effect quickly and wears off quickly."
        },
        {
          "id": "pr-pethidine",
          "label": "Pethidine or diamorphine injection",
          "evidence": "An injection that can help you relax between contractions. It can make you feel drowsy and may affect the baby's breathing if given close to delivery."
        },
        {
          "id": "pr-epidural",
          "label": "Epidural",
          "evidence": "The most effective form of pain relief in labour. Available in hospitals. Your anaesthetist will explain the procedure, benefits, and risks."
        },
        {
          "id": "pr-remifentanil",
          "label": "Remifentanil PCA (if available at your unit)",
          "evidence": "A patient-controlled pain relief option available in some units. Speak to your midwife about whether this is offered where you plan to give birth."
        },
        {
          "id": "pr-open",
          "label": "I would like to keep my options open",
          "evidence": "You do not have to decide now. Your midwife will discuss options with you during labour."
        }
      ]
    }
  ]
}
```

### A.4 names.json — example entries

```json
[
  {
    "name": "Olivia",
    "nameStyle": "feminine",
    "origin": "Latin",
    "meaning": "Olive tree",
    "ons_current_rank": 1,
    "ons_trend": [3, 2, 1, 1, 1, 1, 1, 1, 1, 1],
    "trend_direction": "stable_high"
  },
  {
    "name": "Noah",
    "nameStyle": "masculine",
    "origin": "Hebrew",
    "meaning": "Rest, comfort",
    "ons_current_rank": 1,
    "ons_trend": [10, 7, 4, 2, 1, 1, 1, 1, 1, 1],
    "trend_direction": "rising"
  }
]
```

`ons_trend` is an array of annual rankings over the **last 10 years** (oldest to newest; v2.2 confirms 10-year window — not 20). `trend_direction` is computed: "rising" (rank improved by >20 positions over 5 years), "falling" (rank dropped by >20), "stable_high" (consistently top 200), "stable" (all others). v2.2 renames the v2.0 `gender` field to `nameStyle` per SED-F10-002 — the name has a style, but the baby it's assigned to need not be gendered.

---

## 17. Appendix B: Key external references

**NHS and NICE:**
- NHS Week-by-week guide to pregnancy (Best Start in Life IA from 2024): https://www.nhs.uk/best-start-in-life/pregnancy/week-by-week-guide-to-pregnancy/
- NHS Hospital bag checklist: https://www.nhs.uk/pregnancy/labour-and-birth/preparing-for-the-birth/pack-your-bag-for-labour/
- NHS Birth plan: https://www.nhs.uk/pregnancy/labour-and-birth/preparing-for-the-birth/how-to-make-a-birth-plan/
- NHS Signs of labour: https://www.nhs.uk/pregnancy/labour-and-birth/signs-of-labour/signs-that-labour-has-begun/
- NHS Mental health in pregnancy: https://www.nhs.uk/pregnancy/keeping-well/mental-health/
- NICE NG201 Antenatal care (Aug 2021, Quality Statement 6 added Jan 2025): https://www.nice.org.uk/guidance/ng201
- NICE NG137 Twin and triplet pregnancy (last updated April 2024): https://www.nice.org.uk/guidance/ng137
- NICE NG229 Fetal monitoring in labour (Dec 2022): https://www.nice.org.uk/guidance/ng229
- NICE CG190 Intrapartum care for healthy women and babies (2014, updated 2017): https://www.nice.org.uk/guidance/cg190
- NICE NG192 Caesarean birth (Mar 2021, updated June/July 2025): https://www.nice.org.uk/guidance/ng192
- MHRA "Crafting an intended purpose in the context of software as a medical device" (SaMD Change Programme): https://www.gov.uk/government/publications/software-and-ai-as-a-medical-device-change-programme
- MHRA Standalone software and apps medical-device flowchart: https://assets.publishing.service.gov.uk/government/uploads/system/uploads/attachment_data/file/1105233/Medical_device_stand-alone_software_including_apps.pdf
- MHRA Digital Mental Health Technology guidance (Feb 2025): https://assets.publishing.service.gov.uk/media/6866572fadfe29730ea3a9d5/MHRA_guidance_on_DMHT_-_Device_characterisation_regulatory_qualification_and_classification.pdf

**Charities and support organisations:**
- Tommy's fetal movements: https://www.tommys.org/pregnancy-information/symptoms-and-complications/baby-movements-in-pregnancy
- Tommy's helpline: 0800 014 7800 / https://www.tommys.org
- Miscarriage Association: https://www.miscarriageassociation.org.uk / Helpline: 0303 003 6464 (Mon, Tue, Thu 9–4; Wed, Fri 9–8)
- Ectopic Pregnancy Trust: https://www.ectopic.org.uk / Helpline: 020 7733 2653
- ARC (Antenatal Results and Choices — TFMR, screening decisions): https://www.arc-uk.org / Helpline: 020 7713 7486
- Petals (specialist counselling after baby loss): https://www.petalscharity.org
- Aching Arms: https://www.achingarms.co.uk
- Birth Trauma Association: https://www.birthtraumaassociation.org
- Child Bereavement UK: https://www.childbereavementuk.org
- Mariposa International (parent charity of Saying Goodbye): https://www.sayinggoodbye.org
- Sands (stillbirth and neonatal death): https://www.sands.org.uk / Helpline: 0808 164 3332
- Twins Trust: https://twinstrust.org
- PANDAS Foundation: https://www.pandasfoundation.org.uk / Helpline: 0808 196 1776
- Samaritans: https://www.samaritans.org / 116 123
- Lullaby Trust: https://www.lullabytrust.org.uk
- Maternal Mental Health Alliance: https://www.maternalmentalhealthalliance.org

**RCOG:**
- Reduced fetal movements (Green-top Guideline No. 57): https://www.rcog.org.uk/guidance/browse-all-guidance/green-top-guidelines/reduced-fetal-movements-green-top-guideline-no-57/

**Regulatory and legal:**
- ICO Age Appropriate Design Code: https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/childrens-information/childrens-code-guidance-and-resources/age-appropriate-design-code/
- Google Play Health Apps Declaration: https://support.google.com/googleplay/android-developer/answer/14738291
- ONS Baby names statistics: https://www.ons.gov.uk/peoplepopulationandcommunity/birthsdeathsandmarriages/livebirths/bulletins/babynamesenglandandwales/2023
- gov.uk Maternity pay and leave: https://www.gov.uk/maternity-pay-leave
- gov.uk Child Benefit: https://www.gov.uk/child-benefit

**Technical:**
- react-native-mmkv: https://github.com/mrousavy/react-native-mmkv
- Zustand: https://zustand.docs.pmnd.rs
- NativeWind: https://www.nativewind.dev
- Expo Router: https://docs.expo.dev/router/introduction/
- RevenueCat React Native: https://www.revenuecat.com/docs/getting-started/installation/reactnative
- react-native-gifted-charts: https://www.npmjs.com/package/react-native-gifted-charts
- lucide-react-native: https://lucide.dev/guide/packages/lucide-react-native
- expo-notifications: https://docs.expo.dev/versions/latest/sdk/notifications/
- expo-print: https://docs.expo.dev/versions/latest/sdk/print/
- expo-keep-awake: https://docs.expo.dev/versions/latest/sdk/keep-awake/
- expo-haptics: https://docs.expo.dev/versions/latest/sdk/haptics/

---

## 18. Roadmap (new in v2.1)

The PRD is deliberately silent on post-launch direction in v2.0. v2.1 specifies a minimal roadmap to make the "v1.0 scope discipline" sustainable.

### v1.1 (months 3–6 post-launch)

- **F11 Postnatal module (0–6 weeks):** "days since birth" counter, postnatal check schedule (6-week GP check, health visitor visits, baby red book pointer), feeding log (count only — taps recorded, no interpretation, per SED-SAF-005 logic), nappy log (count only), postnatal wellbeing signposting (PANDAS, Birth Trauma Association, NCT postnatal support, NSPCC), static "in the weeks after birth" content (lochia, perineal care, c-section recovery, mental wellbeing flags) with the same banner/disclaimer pattern as F03/F04. ~15–20 dev-days.
- **Welsh-language content** (`cy-GB` locale) per SED-CONTENT-002.
- **System dark mode** across the full app (per SED-UX-018; v1.0 acceptable compromise was light-only).
- **Commissioned illustration set** if v1.0 used placeholders.
- **Chorionicity-specific twin schedules** (MC/DC vs DC/DA).

### v1.2 (months 6–12)

- **Pregnancy-after-loss pathway** — separate, opt-in mode with gentler onboarding, no celebratory milestones by default, no fruit-size comparisons unless explicitly enabled.
- **IVF integration** — cycle tracking pre-conception.
- **Partner standalone-app variant** (same codebase, partner-mode default).
- **EAS Update reconsidered** on a JS-bugfix-only channel with written policy excluding content JSON (SED-ARCH-015).

### v2.0 (year 2)

- **Optional iCloud / Google Drive encrypted sync** (still no Seed server) — uses each platform's native end-to-end-encrypted storage.
- **Apple Health / Google Fit handoff** — read-only export of weight to Apple Health, opt-in, one-way only.
- **Additional UK community languages** (Polish, Urdu, Punjabi, Bengali, Romanian, Arabic) per UK maternity populations.

### Explicit non-roadmap (deliberately never)

- Social features
- Community forum
- AI chat / conversational agent
- Symptom checker / decision support
- Threshold-based alerting on user data
- Advertising or sponsored content
- Any feature that interprets user-entered data

Preserves the regulatory boundary in perpetuity.

---

*Document version: 2.2. Authors: Jonathan Watchorn (CSO) and Claude (Anthropic). v2.1 enhancements via a multi-agent research pass (May 2026); v2.2 via a second pass auditing v2.1 (May 2026). For clinical content sign-off: Jonathan Watchorn. For technical implementation: Christian Baverstock using Claude Code.*

*This document is the single source of truth for Seed. Where any conflict exists between this document and any other material (v2.0/v2.1 PRD, research documents, conversation notes, earlier drafts), v2.2 takes precedence. See `PRD_v2_1_to_v2_2_CHANGELOG.md` for the full delta from v2.1; `PRD_v2_0_to_v2_1_CHANGELOG.md` for the prior delta.*

# Seed PRD — Regulatory & Clinical Validation Report

Validation of Sections 5, 7, 8, and Appendix B of the Seed Maternity Toolkit PRD v2.0 against current 2025–2026 UK regulatory and clinical sources. Findings target the May 2026 launch context.

---

## Confirmed facts

1. **The "bright line" interpretation principle (SED-SAF-001/002).** MHRA's qualification approach explicitly excludes from medical-device status "apps which are limited to storage or display of manually entered user data" and software that "monitors fitness/health/wellbeing and/or stores medical information without change". The PRD's framing — record/display = notepad; interpret = device — is consistent with MHRA's published position and with the 2025 MHRA Digital Mental Health Technology guidance which uses the same qualification flowchart for SaMD.

2. **MHRA "Crafting an intended purpose" guidance** is the right reference. Now the authoritative intended-purpose template (structure/function, intended population, intended user, intended use environment). Seed should explicitly cite it.

3. **DTAC is out of scope for Seed.** DTAC applies to digital health technologies trialled/purchased/embedded/advocated by NHS bodies. A consumer-facing offline app sold via the public app stores does not need DTAC certification.

4. **ICO £40 micro-org fee is correct for 2026** (Tier 1: ≤10 staff or ≤£632,000 turnover). Direct-debit payers get a £5 discount (effective £35).

5. **Children's Code = 15 standards, still in force.** "Likely to be accessed by children" remains the trigger.

6. **NICE NG201 (Antenatal care)** — published 19 Aug 2021, still current; Quality Statement 6 added Jan 2025.

7. **NICE NG137 (Twin and triplet pregnancy)** — first published 4 Sept 2019, last updated 9 April 2024.

8. **NICE NG229 (Fetal monitoring in labour)** — published 14 Dec 2022. NG229's actual scope is **Fetal monitoring in labour**, not "Intrapartum care" generally. The broader intrapartum-care guideline is **CG190**. The PRD mislabels NG229.

9. **Helpline numbers confirmed current:** Tommy's 0800 014 7800; Sands 0808 164 3332; PANDAS 0808 1961 776; Samaritans 116 123.

10. **App store category guidance.** Apple lists "pregnancy" under **Health & Fitness**, not Medical. PRD positioning correct.

11. **Tommy's / RCOG language alignment (F04 SED-F04-007/011).** RCOG Green-top 57 explicitly rejects "count to 10" charts and population thresholds; focuses on "a change from what is normal for the individual baby." PRD's signposting is faithful to current guidance.

---

## Corrections needed

### C1. NICE NG232 does not exist — Caesarean birth is NG192

The PRD cites "NICE NG232 (Caesarean birth)" four places. The correct ID is **NG192**, published 31 March 2021, with notable updates 10 June 2025 and July 2025 (oxytocin/carbetocin, opioid pain management aligned to MHRA safety advice).
Source: https://www.nice.org.uk/guidance/ng192

### C2. NICE NG229 is "Fetal monitoring in labour", not "Intrapartum care"

NG229's title is **"Fetal monitoring in labour"**; it replaces the monitoring section of CG190.
Source: https://www.nice.org.uk/guidance/ng229 ; https://www.nice.org.uk/guidance/cg190

### C3. NHS week-by-week URL has moved

NHS.uk migrated to the **"Best Start in Life"** information architecture during 2024–2025. Current canonical URL pattern:
`https://www.nhs.uk/best-start-in-life/pregnancy/week-by-week-guide-to-pregnancy/{trimester}/week-{N}/`

### C4. Miscarriage Association helpline number is out of date

PRD lists **01924 200 799**. The Miscarriage Association has moved to a freephone number: **0303 003 6464** (Mon, Tue, Thu 9–4; Wed, Fri 9–8).
Source: https://www.miscarriageassociation.org.uk/how-we-help/helpline/

### C5. RCOG Green-top 57 is now in its 2nd edition

The 2nd edition (peer-reviewed via BJOG, 2024–2025) supersedes the Feb 2011 first edition.

---

## Strengthening recommendations

### R1. Add an explicit "intended purpose statement" to Section 5.1 aligned to the MHRA template

Add as **SED-SAF-001a**:

> "Seed's intended purpose is to act as a personal organisational and record-keeping aid for users navigating pregnancy and the early postnatal period in the UK. It does not diagnose, screen, prevent, monitor, predict, prognose, treat or alleviate any disease, injury or disability, nor does it provide information derived from in vivo data that is intended to provide information for medical decision-making. Structure and function: a static-content reference and user-entered data log. Intended population: adults aged 16+ in the UK who are pregnant or in the postnatal period (and their partners). Intended user: lay person, self-selecting. Intended use environment: personal mobile device, used at the user's own initiative, outside any clinical care pathway."

### R2. Strengthen Article 9(2)(a) explicit-consent mechanics (SED-PRIV-007)

Because Seed processes data **only on-device**, a strong argument is that *no controller processing occurs* and Article 9 is not engaged at all. The PRD already gestures at this in 7.1 but then asserts 9(2)(a) compliance in 7.2 — these positions are in tension. Resolve as **SED-PRIV-007 (revised)**:

> "Seed processes special-category data only on the user's device. The developer is not a controller of that data because no data leaves the device and the developer has no access to it. The first-launch disclaimer therefore is **not** relied upon as Article 9 consent for processing by the developer; it functions as a safety acknowledgement..."

### R3. Expand SED-SAF-005 prohibited-terms list

Add: monitor, monitoring, detect, detection, predict, prediction, screen, screening, assess, assessment, healthy, warning sign, red flag, reassure, reassuring, accurate, medically accurate.

### R4. Add Google Play Health Apps Declaration explicit disclaimer wording

Google's policy (effective Jan 2026 enforcement) requires the exact disclaimer text "this app is not a medical device and does not diagnose, treat, cure, or prevent any medical condition" plus a "consult a healthcare professional" reminder, included in the app description and in-app.

### R5. Add Apple Spring 2026 regulated-medical-device status declaration

Apple is rolling out (Spring 2026) a requirement that all Health & Fitness and Medical category apps declare their regulated-medical-device status in App Store Connect for EEA/UK/US distributions.

### R6. Reframe ICO compliance basis

The ICO registration is required, but on the basis of **purchase-data processing performed via RevenueCat**, not the on-device health data.

### R7. Clinical-review-versioning requirement

Add **SED-SAF-003a**: CLINICAL_REVIEW_LOG.md MUST record specific edition/version of source guideline used.

### R8. Children's Code: age trigger clarification

The Children's Code applies to anyone under 18, not under 16. The onboarding age question is informational only — not a gate.

---

## Open risks

1. Birth-plan PDF export (F07) edges toward "clinical document"; mitigation footer required.
2. F01 due-date calculator output — use NHS terminology "estimated due date".
3. F09 "What to expect" and F10 birth-plan evidence summaries are highest classification-drift risk content.
4. Apple's Spring 2026 medical-device declaration still rolling out — verify at submission.
5. Data (Use and Access) Act 2025 has triggered ICO Children's Code review — pre-submission re-check.
6. RCOG GTG-57 2nd edition introduced more nuanced language — cross-check SED-F04-011 wording.
7. PANDAS Foundation historic helpline service disruption — re-verify operating hours pre-launch.
8. "Are you 16 or over?" risks an under-16 user lying and getting full features. PRD says it doesn't block — make explicit it is not a gate.

---

## Sources

- MHRA Crafting an Intended Purpose guidance
- MHRA standalone-software/apps flowchart (gov.uk)
- MHRA Digital Mental Health Technology guidance (Feb 2025)
- NHS Transformation Directorate DTAC
- ICO data protection fee guidance
- ICO Children's Code (15 standards)
- NICE NG201, NG137, NG229, NG192
- NHS Best Start in Life week-by-week
- RCOG Green-top 57 (2nd edition)
- Apple App Review Guidelines 5.1.1
- Google Play Health Apps Declaration
- Miscarriage Association helpline (new freephone 0303 003 6464)
- Sands, Tommy's, Samaritans

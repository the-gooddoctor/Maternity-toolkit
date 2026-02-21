# Seed: Maternity Toolkit

## Product Requirements Document and Technical Specification

**Version 2.0 | February 2026**

**Prepared for:** Jonathan Watchorn (CSO) and Christian Baverstock (CTO)

**Confidential**

*Working title: "Seed". Final brand name subject to trademark clearance. The product is developed by the same entity as the company's clinical-grade maternity information agent, and shares its commitment to clinical quality, privacy, and UK-focused content. Seed is a distinct, standalone product.*

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
3. Feature specification (F01-F10)
4. Cross-cutting enhancements
5. Safety, clinical standards, and regulatory position
6. Technical specification
7. Privacy and data protection
8. Content strategy
9. User experience
10. App store compliance
11. Build plan and phased delivery
12. Claude Code implementation guide
13. Go-to-market
14. Success metrics
15. Risk register
16. Appendix A: Static data schemas
17. Appendix B: Key external references

---

## 1. Executive summary

### 1.1 Purpose

Seed is a privacy-first, offline-only pregnancy and postnatal toolkit for UK users. It provides high-utility tracking tools (contraction timer, kick counter, weight tracker), NHS-aligned weekly developmental information, and practical checklists, with zero data collection, zero ads, and zero subscriptions.

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

**Primary:** Pregnant women in the UK, aged 16-45, seeking practical tracking tools with NHS-aligned terminology and pathways. First-time mothers are the highest-value segment (highest information anxiety, highest engagement).

**Secondary:** Partners and birth companions who want to understand and support the pregnancy. Seed includes a dedicated partner mode (see Section 4.1).

**Tertiary:** Women expecting twins or multiples, who are chronically underserved by existing apps (see Section 4.2).

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

**SED-REV-001:** Price: £4.99 one-time. This signals quality, compensates for volume constraints of a paid tier competing against free alternatives, and is substantially cheaper than subscription competitors (Pregnancy+ at £3.99/month, Flo at approximately £43/year). After platform commission (15% under small business programmes on both Google Play and Apple App Store), net per sale is approximately £4.24.

### 2.3 Revenue projections (UK market, year one)

| Scenario | Free downloads | Pro conversions (5%) | Net annual revenue |
|---|---|---|---|
| Pessimistic | 5,000 | 250 | £1,060 |
| Realistic | 15,000 | 750 | £3,180 |
| Optimistic | 40,000 | 2,000 | £8,480 |
| Strong performer | 80,000 | 4,000 | £16,960 |

Context: approximately 650,000 UK births per year. Over 50% of pregnant women download at least one pregnancy app. The average is 3 apps per pregnancy.

### 2.4 Purchase implementation

**SED-REV-002:** Use RevenueCat for in-app purchase management across both platforms. RevenueCat provides: unified API for Google Play Billing and Apple StoreKit, receipt validation without a custom backend, analytics dashboard for conversion tracking, and offline entitlement caching.

**SED-REV-003:** The app MUST function fully offline after initial purchase validation. RevenueCat's offline entitlement caching ensures Pro features remain accessible without network connectivity.

**SED-REV-004:** The free tier MUST be fully functional without any account creation, email entry, or network connectivity.

**SED-REV-005:** Pro features MUST be gated with a single, clearly labelled unlock screen accessible from any locked feature. The screen MUST show: what is included, the price (£4.99, one-time), and a clear "no subscription, no recurring charges" statement. No dark patterns, no misleading trial flows.

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

**SED-F01-008:** For twin/multiple pregnancies (see Section 4.2), display an additional note: "Twins are often born earlier than singleton babies. Your consultant will discuss timing with you, usually between 36 and 37 weeks."

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

**SED-F02-003:** Content MUST be sourced from NHS "Week-by-week guide to pregnancy" and/or other Tier 1 UK sources (NICE, RCOG, Tommy's). Content MUST be rewritten to avoid copyright infringement while preserving clinical accuracy. All content requires CSO sign-off [Jon].

**SED-F02-004:** Data MUST be stored as a static JSON file bundled within the app. No network requests.

**SED-F02-005:** The user MUST be able to browse any week (past or future), not only the current week. Swipe left/right or tap navigation.

**SED-F02-006:** Use high-quality 2D illustrations or vector graphics for baby size comparisons. Do NOT attempt 3D fetal models.

**SED-F02-007:** For twin/multiple pregnancies, display adjusted content where it diverges from singleton pregnancy (primarily from week 24 onwards: increased monitoring, earlier delivery planning, different weight expectations). This content is conditional on the `isMultiple` setting flag.

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
    "You may notice Braxton Hicks contractions — irregular, painless tightenings.",
    "Heartburn and indigestion are common as your uterus presses on your stomach.",
    "You may feel more tired as your body works harder."
  ],
  "midwife_questions": [
    "Ask about your blood group antibodies and whether you need anti-D.",
    "Ask about your glucose tolerance test results if you had one.",
    "Discuss your birth preferences and where you would like to give birth."
  ],
  "key_appointments": "You should have an antenatal appointment around 28 weeks. Your midwife will check your blood pressure, test your urine, and measure your bump. You may be offered a blood test for anaemia.",
  "partner_content": {
    "this_week": "Your partner may be experiencing more tiredness, heartburn, and backache. Braxton Hicks contractions can feel alarming but are usually harmless.",
    "how_to_help": "Offer to take on more household tasks. Ask about their birth preferences and attend antenatal classes together if you can."
  },
  "twin_adjustments": {
    "development_note": "With twins, your babies may be slightly smaller than the singleton sizes shown. This is normal.",
    "appointment_note": "You will have more frequent scans and appointments. Your next scan may be around 28 weeks.",
    "additional_question": "Ask your consultant about your delivery plan and when they expect your babies to arrive."
  },
  "nhs_link": "https://www.nhs.uk/pregnancy/week-by-week/28-to-40-plus/28-weeks/",
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

**SED-F03-014:** Display a persistent, non-dismissible banner at the top of the contraction timer screen: "This timer logs your contractions. It does not provide medical advice. The NHS advises contacting your maternity unit when your contractions are regular, coming every 5 minutes, and each lasting about 60 seconds. If you are unsure or worried at any point, call your maternity unit."

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

**Export (SED-F04-009 to SED-F04-010):**

**SED-F04-009:** Provide an export function that generates a one-page PDF or shareable text summary of movement history (last 7 or 14 days, user-selectable). Format: date, session start time, number of movements, session duration. Clean, legible, professional layout suitable for showing a midwife at an appointment.

**SED-F04-010:** PDF header: "Fetal Movement Log — generated by Seed. This is a record of movements logged by the user. It has not been clinically reviewed."

**Clinical signposting (static, non-personalised):**

**SED-F04-011:** Display a persistent, non-dismissible banner: "This tool helps you log your baby's movements. It does not analyse them. There is no set number of movements that is 'normal'. What matters is your baby's usual pattern. If you think your baby is moving less than usual, contact your maternity unit immediately. Do not wait until the next day."

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

---

### 3.6 F06: Hospital bag checklist (personalised by birth setting)

**Purpose:** Reduce anxiety and cognitive load in the final weeks of pregnancy with a checklist tailored to the user's planned birth setting.

**Requirements:**

**SED-F06-001:** On first access, ask: "Where are you planning to give birth?" Options:
- Hospital labour ward
- Birth centre / midwife-led unit
- Home birth
- Planned caesarean section
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
8. Feeding intentions (breast, bottle, combination, undecided)
9. If things change (preferences for assisted delivery, emergency caesarean section, neonatal care if needed)
10. Special considerations (free text: cultural, religious, disability, previous birth experience, anxiety)

**SED-F07-002:** Each option within a section MUST include a one-line evidence summary in a subtle, expandable "Why this matters" area. Examples:

- **Delayed cord clamping:** "NICE recommends clamping the cord no earlier than one minute after birth unless there is a concern about the baby (NICE NG229, 2023)."
- **Epidural:** "An epidural is the most effective form of pain relief in labour. It is available in hospital and some birth centres. Your midwife or anaesthetist can discuss the benefits and risks with you."
- **Skin-to-skin contact:** "The NHS recommends skin-to-skin as soon as possible after birth. It helps regulate your baby's temperature and supports bonding and breastfeeding."
- **Planned caesarean section:** "A planned caesarean is a valid birth choice. NICE recommends discussing the reasons, the procedure, and recovery with your maternity team (NICE NG232)."

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

| Appointment | What to expect |
|---|---|
| Booking appointment (8-10 weeks) | "Your midwife will take your medical history, discuss screening tests, check your height, weight, and blood pressure, and take blood and urine samples. This appointment usually takes about an hour. You will receive your handheld maternity notes." |
| Dating scan (11-14 weeks) | "An ultrasound scan to check your baby's development and confirm your due date. If you have chosen to have screening for Down's syndrome, Edwards' syndrome, and Patau's syndrome, the nuchal translucency measurement may be taken at this scan." |
| Anomaly scan (18-21 weeks) | "A detailed ultrasound scan to check your baby's development. The sonographer will check your baby's bones, heart, brain, spinal cord, face, kidneys, and abdomen. You may be able to find out your baby's sex if you wish." |
| 28-week appointment | "Your midwife will check your blood pressure, test your urine, and measure your bump (symphysis-fundal height). You may be offered a blood test to check for anaemia. If you are Rhesus negative, you will be offered anti-D." |
| 36-week appointment | "Your midwife will check your baby's position. If your baby is breech, you may be offered an external cephalic version (ECV). You will discuss your birth plan and your options for where to give birth." |

**SED-F09-008:** "What to expect" content is static, NHS-sourced, and requires CSO sign-off [Jon].

**SED-F09-009:** Users MUST be able to edit, delete, or add to the pre-populated schedule. They MUST be able to dismiss the pre-population offer and manage appointments manually.

---

### 3.10 F10: Baby name favourites with ONS popularity trends

**Purpose:** Provide a searchable name database with UK popularity data, personal shortlists, and trend visualisation.

**Requirements:**

**SED-F10-001:** Searchable database of baby names. Target: 10,000+ names. Source: ONS baby name statistics (public domain, published annually). Include names from England and Wales dataset.

**SED-F10-002:** Filter by: first letter, gender (boy/girl/unisex), popularity ranking (current ONS top 100, top 500, all), origin/cultural background where available.

**SED-F10-003:** Each name entry displays: name, gender classification, current ONS ranking (if in top 1000), meaning (where available), origin (where available).

**SED-F10-004:** For names in the ONS dataset, display a small sparkline chart showing popularity trend over the last 10-20 years (rising, falling, stable). This uses historical ONS data bundled as static JSON. Label: "Popularity trend (England and Wales)."

**SED-F10-005:** Filter option: "Currently trending" (names that have risen significantly in ranking over the last 5 years) and "Classic" (names that have remained in the top 200 consistently).

**SED-F10-006:** Save names to "My favourites" shortlist. Separate "Partner's favourites" shortlist stored locally. No account sync.

**SED-F10-007:** Display "Names we both like" — the intersection of both shortlists (simple array comparison).

**SED-F10-008:** Data stored as static JSON, bundled in the app. No network dependency.

---

## 4. Cross-cutting enhancements

These enhancements apply across multiple features and represent Seed's key differentiators.

### 4.1 Partner Mode

**SED-CC-001:** Settings toggle: "I am..." with options: "The pregnant person" / "A partner or birth companion". Default: "The pregnant person".

**SED-CC-002:** When partner mode is active:
- The week-by-week tracker (F02) displays the "For your birth partner" section on each weekly card.
- The contraction timer (F03) always shows the "For your birth partner" coaching tips section.
- Pronouns throughout the app adjust: "your baby" remains unchanged, but body-specific content uses third person ("they may be experiencing..." rather than "you may be experiencing...").
- The hospital bag checklist (F06) highlights the "For your birth partner" category.

**SED-CC-003:** Partner mode is a UI presentation toggle stored in MMKV settings. It does not change the underlying data or create a separate user profile. The same device can switch between modes.

**SED-CC-004:** Partner mode content requirements: all "For your birth partner" content is practical, respectful, and actionable. It avoids being patronising ("she needs you to be strong") and focuses on concrete actions. Examples of good partner content: "Offer water between contractions", "Ask if they want the lights dimmed", "Have the maternity unit number ready on your phone." Examples of content to avoid: "Remember, this is harder for her than for you", "Your job is to stay calm."

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

### 4.3 Pregnancy loss pathway

**SED-CC-009:** The settings menu MUST include an option: "Pause or end tracking". This option MUST be accessible at all times.

**SED-CC-010:** Tapping "Pause or end tracking" displays a compassionate screen:

> "If your circumstances have changed, you can pause or stop all tracking, notifications, and updates. We are sorry if you are going through a difficult time.
>
> **Pause** — temporarily stop notifications and weekly updates. Your data is kept and you can resume at any time.
>
> **End tracking** — stop all tracking. Your data remains on your device until you choose to delete it.
>
> **Delete everything** — remove all data and reset the app.
>
> If you need support:
> - Miscarriage Association: www.miscarriageassociation.org.uk — Helpline: 01924 200 799
> - Tommy's: www.tommys.org — Helpline: 0800 014 7800
> - Sands (stillbirth and neonatal death): www.sands.org.uk — Helpline: 0808 164 3332"

**SED-CC-011:** "Pause" sets a `trackingPaused` flag in MMKV settings. While paused: all push notifications are cancelled, the home screen shows a neutral message ("Tracking is paused. You can resume at any time or access your data in settings."), no gestational-age content is displayed.

**SED-CC-012:** "End tracking" sets `trackingEnded` flag. Similar to pause but the home screen messaging changes to: "Take care of yourself. Your data is still here if you need it."

**SED-CC-013:** "Delete everything" triggers the full data wipe (SED-PRIV-004) and returns to the onboarding screen.

**SED-CC-014:** The app MUST NOT send any push notifications after tracking is paused or ended.

**SED-CC-015:** The app MUST NOT send notifications that reference gestational milestones in a celebratory tone (e.g. "Congratulations, you are 20 weeks today!"). All notifications MUST be neutral and factual (e.g. "Appointment reminder: dating scan tomorrow").

### 4.4 Mental wellbeing signposting

**SED-CC-016:** The "More" tab MUST include a "Your wellbeing" section. This is a static page, not a mood tracker or questionnaire (which would risk device classification). Content:

> "Pregnancy can bring a mix of emotions, and that is completely normal. If you are feeling anxious, low, overwhelmed, or not like yourself, you are not alone.
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

**The bright line is interpretation.** A tool that records data the user enters and displays it back to them is a digital notepad. A tool that interprets, analyses, or draws clinical conclusions from that data is a medical device. Seed stays firmly on the notepad side. This principle MUST be applied to every feature and every piece of user-facing text.

### 5.2 Clinical content standards

**SED-SAF-002:** All static clinical content MUST be sourced from Tier 1 UK authorities: NHS, NICE, RCOG, Tommy's. See Section 8 for the complete content strategy.

**SED-SAF-003:** All clinical content MUST be reviewed and signed off by the CSO [Jon] before inclusion in any release.

**SED-SAF-004:** Clinical signposting text MUST reproduce or closely paraphrase publicly available NHS/Tommy's guidance. It MUST NOT be original clinical advice.

**SED-SAF-005:** The app MUST NOT use the following terms in any user-facing context: "safe", "unsafe", "normal" (when applied to user data), "abnormal", "diagnosis", "diagnose", "treatment", "treat", "prescribe", "recommend" (in a clinical sense), "risk" (when applied to the user specifically), "you should" (in clinical contexts). Permitted alternatives: "your midwife can advise", "speak to your maternity team", "the NHS advises", "you may wish to discuss with your midwife".

**SED-SAF-006:** Every feature that records health-related data (F03, F04, F05) MUST display a persistent, non-dismissible clinical signposting banner as specified in the relevant feature section.

### 5.3 Medical disclaimer

**SED-SAF-007:** On first launch, the app MUST present a non-dismissible, full-screen modal requiring explicit acknowledgement (tap "I understand") before access to any features. The disclaimer text:

> **Before you start**
>
> Seed is a tracking and organisational tool. It does not provide medical advice, diagnosis, or treatment.
>
> Using this app does not create a clinical relationship between you and the developers.
>
> Always speak to your midwife, GP, or maternity unit about any concerns regarding your pregnancy or your baby's health.
>
> **In an emergency, call 999.** If you are worried about your baby's movements, have bleeding, severe pain, or any sudden change, contact your maternity unit immediately. Do not wait.
>
> Information in this app is based on NHS and other UK public health sources. It is general guidance and may not apply to your individual circumstances.
>
> [I understand]

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

---

## 6. Technical specification

### 6.1 Technology stack

| Component | Package / technology | Version to target | Purpose |
|---|---|---|---|
| Runtime | React Native | Latest stable (0.76+) | Cross-platform mobile framework |
| Toolchain | Expo (managed workflow) | SDK 52+ | Build toolchain, OTA updates, native modules |
| Navigation | expo-router | v4+ | File-based routing |
| State management | zustand | v5+ | Minimal-boilerplate reactive state |
| Persistence | react-native-mmkv | v3+ | Synchronous encrypted local storage via JSI |
| Persistence adapter | zustand-mmkv-storage | Latest | Automatic Zustand-to-MMKV persistence |
| Styling | nativewind | v4+ | Tailwind CSS utility classes for React Native |
| UI components | @gluestack-ui/themed | v2+ | Accessible pre-built primitives |
| Charts | react-native-gifted-charts | Latest | Lightweight performant charting |
| Icons | lucide-react-native | Latest | 1,000+ consistent SVG icons |
| Notifications | expo-notifications | Latest | Local-only scheduled notifications |
| PDF export | expo-print | Latest | HTML-to-PDF generation |
| Screen wake | expo-keep-awake | Latest | Prevent screen lock during Labour Mode |
| Haptics | expo-haptics | Latest | Tactile feedback on timer/counter taps |
| In-app purchases | react-native-purchases (RevenueCat) | Latest | Cross-platform billing |
| Linking | expo-linking | Latest | Phone dialler and external URL links |
| Splash screen | expo-splash-screen | Latest | Controlled splash during initialisation |

### 6.2 Architecture principles

**SED-ARCH-001:** Zero network dependency. The app MUST operate with zero network connectivity after initial download and (if Pro) purchase validation. No analytics, no telemetry, no crash reporting, no remote config, no feature flags fetched from a server.

**SED-ARCH-002:** On-device only. All user data MUST be stored exclusively on the user's device using MMKV with encryption enabled. No data leaves the device under any circumstances.

**SED-ARCH-003:** No tracking SDKs. The app MUST NOT include any third-party SDK that transmits data off-device. Explicitly excluded: Firebase Analytics, Google Analytics, Facebook SDK, Sentry, Crashlytics, Amplitude, Mixpanel, and any advertising SDK.

**SED-ARCH-004:** Zustand + MMKV persistence. State management MUST use Zustand stores with `zustand-mmkv-storage` persistence middleware. Every store mutation is automatically and synchronously persisted to encrypted MMKV storage. No manual save/load logic. No async loading states for persisted data.

**SED-ARCH-005:** Static content bundling. All clinical content (weekly data, checklists, name database, birth plan options, appointment descriptions) MUST be bundled as static JSON files within the app binary. No network fetches for content.

**SED-ARCH-006:** Single codebase. The same React Native / Expo codebase MUST produce both Android and iOS builds via EAS Build. No platform-specific native code except where Expo modules handle it internally.

### 6.3 Platform targets

**SED-ARCH-007:** Primary: Android (Google Play Store).
**SED-ARCH-008:** Secondary: iOS (Apple App Store). Target submission 2-4 weeks after Android launch.
**SED-ARCH-009:** Minimum Android API: 26 (Android 8.0). Target SDK: API 35 (Android 15).
**SED-ARCH-010:** Minimum iOS: 16.0.

### 6.4 Timestamp delta method

All timer features (F03 contraction timer, F04 kick counter session timer) MUST use this pattern. This is critical for accuracy during app backgrounding.

```javascript
// === TIMESTAMP DELTA TIMER HOOK ===
// This hook provides a timer that survives app backgrounding.
// It does NOT use react-native-background-timer or any native background module.

import { useRef, useState, useEffect, useCallback } from 'react';
import { AppState } from 'react-native';
import { useMMKVString } from 'react-native-mmkv';

const useTimestampDeltaTimer = (storageKey: string) => {
  const [startTimestamp, setStartTimestamp] = useMMKVString(`${storageKey}_start`);
  const [elapsedMs, setElapsedMs] = useState(0);
  const [isRunning, setIsRunning] = useState(false);
  const intervalRef = useRef<NodeJS.Timeout | null>(null);

  // Recalculate elapsed time from stored timestamp
  const recalculate = useCallback(() => {
    if (startTimestamp) {
      const start = parseInt(startTimestamp, 10);
      setElapsedMs(Date.now() - start);
      setIsRunning(true);
    }
  }, [startTimestamp]);

  // Listen for app foregrounding
  useEffect(() => {
    const subscription = AppState.addEventListener('change', (nextState) => {
      if (nextState === 'active' && startTimestamp) {
        recalculate();
      }
    });
    return () => subscription.remove();
  }, [startTimestamp, recalculate]);

  // Foreground-only display interval (UI tick, not the source of truth)
  useEffect(() => {
    if (isRunning) {
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
    const now = Date.now().toString();
    setStartTimestamp(now);
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
**SED-ARCH-012:** Timer features MUST register an AppState listener that recalculates elapsed time on every foreground event.

### 6.5 Zustand store definitions

Each feature has its own Zustand store file. All stores use MMKV persistence middleware.

```typescript
// stores/settingsStore.ts
import { create } from 'zustand';
import { persist } from 'zustand/middleware';
import { mmkvStorage } from './mmkvStorage';

interface SettingsState {
  edd: string | null;                  // ISO date string
  lmp: string | null;                  // ISO date string
  userMode: 'pregnant' | 'partner';    // Partner mode toggle
  pregnancyType: 'singleton' | 'twins' | 'triplets_plus';
  birthSetting: 'hospital' | 'birth_centre' | 'home' | 'caesarean' | 'undecided';
  weightUnit: 'kg' | 'stones';
  maternityUnitPhone: string | null;
  trackingStatus: 'active' | 'paused' | 'ended';
  disclaimerAccepted: boolean;
  proUnlocked: boolean;
  ageConfirmed: boolean;
  userName: string | null;             // Optional, for birth plan PDF only
  // Actions
  setEdd: (edd: string) => void;
  setLmp: (lmp: string) => void;
  setUserMode: (mode: 'pregnant' | 'partner') => void;
  setPregnancyType: (type: 'singleton' | 'twins' | 'triplets_plus') => void;
  setBirthSetting: (setting: string) => void;
  setWeightUnit: (unit: 'kg' | 'stones') => void;
  setMaternityUnitPhone: (phone: string) => void;
  setTrackingStatus: (status: 'active' | 'paused' | 'ended') => void;
  acceptDisclaimer: () => void;
  unlockPro: () => void;
  confirmAge: () => void;
  resetAll: () => void;
}

export const useSettingsStore = create<SettingsState>()(
  persist(
    (set) => ({
      edd: null,
      lmp: null,
      userMode: 'pregnant',
      pregnancyType: 'singleton',
      birthSetting: 'undecided',
      weightUnit: 'kg',
      maternityUnitPhone: null,
      trackingStatus: 'active',
      disclaimerAccepted: false,
      proUnlocked: false,
      ageConfirmed: false,
      userName: null,
      setEdd: (edd) => set({ edd }),
      setLmp: (lmp) => set({ lmp }),
      setUserMode: (mode) => set({ userMode: mode }),
      setPregnancyType: (type) => set({ pregnancyType: type }),
      setBirthSetting: (setting) => set({ birthSetting: setting }),
      setWeightUnit: (unit) => set({ weightUnit: unit }),
      setMaternityUnitPhone: (phone) => set({ maternityUnitPhone: phone }),
      setTrackingStatus: (status) => set({ trackingStatus: status }),
      acceptDisclaimer: () => set({ disclaimerAccepted: true }),
      unlockPro: () => set({ proUnlocked: true }),
      confirmAge: () => set({ ageConfirmed: true }),
      resetAll: () => set({
        edd: null, lmp: null, userMode: 'pregnant', pregnancyType: 'singleton',
        birthSetting: 'undecided', weightUnit: 'kg', maternityUnitPhone: null,
        trackingStatus: 'active', disclaimerAccepted: false, proUnlocked: false,
        ageConfirmed: false, userName: null,
      }),
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
// stores/namesStore.ts
interface FavouriteName {
  name: string;
  gender: 'boy' | 'girl' | 'unisex';
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
// stores/mmkvStorage.ts
import { MMKV } from 'react-native-mmkv';
import { StateStorage } from 'zustand/middleware';

const mmkv = new MMKV({ id: 'seed-storage', encryptionKey: 'seed-enc-key' });

export const mmkvStorage: StateStorage = {
  getItem: (name) => mmkv.getString(name) ?? null,
  setItem: (name, value) => mmkv.set(name, value),
  removeItem: (name) => mmkv.delete(name),
};

// Export for direct MMKV access (timer timestamps)
export { mmkv };
```

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
export const colours = {
  // Core
  background: '#FDF8F4',       // Warm cream
  surface: '#FFFFFF',          // Card backgrounds
  surfaceSubtle: '#F5F0EB',   // Subtle section backgrounds
  
  // Text
  textPrimary: '#2D2A26',     // Near-black, warm
  textSecondary: '#6B6560',   // Muted brown-grey
  textTertiary: '#9B9590',    // Placeholder, disabled
  
  // Accent
  primary: '#7B9E87',         // Sage green (primary actions, selected states)
  primaryDark: '#5C7A66',     // Darker sage (pressed states)
  primaryLight: '#E8F0EB',    // Light sage (backgrounds, badges)
  
  // Secondary
  secondary: '#C4A882',       // Warm tan (secondary elements)
  
  // Semantic
  proGate: '#8B6F47',         // Pro badge and unlock accent
  
  // Clinical signposting
  bannerBackground: '#FFF3E0', // Warm amber background for clinical banners
  bannerBorder: '#E6A817',    // Amber border
  bannerText: '#5D4037',      // Dark brown text
  
  // Labour Mode (dark theme)
  labourBackground: '#1A1A2E', // Deep navy
  labourSurface: '#25253E',    // Slightly lighter navy
  labourText: '#F0EDE8',       // Warm white
  labourAccent: '#E8B4B8',     // Soft rose (start/stop button)
  labourAccentPressed: '#D4979C',
  
  // System
  error: '#C62828',
  success: '#2E7D32',
  divider: '#E8E2DC',
};
```

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

**SED-PRIV-007:** The app MUST obtain explicit consent for processing health data via the first-launch disclaimer (SED-SAF-007), satisfying UK GDPR Article 9(2)(a).

**SED-PRIV-008:** The company MUST register with the ICO as a data controller (annual fee: £40 for micro-organisations). [Jon]

**SED-PRIV-009:** The app MUST comply with the ICO Age Appropriate Design Code: highest-privacy defaults (already met), no nudge techniques for purchases, and an age confirmation during onboarding ("Are you 16 or over?"). Under-16s are not blocked but the app must not use design techniques that exploit younger users.

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
- Simple, warm, inclusive art style with diverse representation
- Vector format (SVG preferred for scalability) or high-resolution PNG
- No anatomically detailed medical illustrations
- Consistent style throughout the set

Budget: £1,000-£2,000. If budget is constrained, use placeholder vector icons for v1.0 and commission the full set for v1.1. The weekly tracker functions without illustrations; they enhance the experience but are not blocking.

---

## 9. User experience

### 9.1 Onboarding flow

Completable in under 60 seconds. Five screens maximum.

**Screen 1 — Welcome:**
App name, tagline ("Your private pregnancy toolkit"), three value propositions in clean iconographic layout: "100% offline", "No ads, no tracking", "NHS-aligned for UK parents".

**Screen 2 — Medical disclaimer (SED-SAF-007):**
Full disclaimer text. Non-dismissible. "I understand" button at bottom (not a checkbox — a full-width button that requires deliberate tap). Button is disabled until the user has scrolled to the bottom of the text.

**Screen 3 — Due date entry:**
"When is your baby due?" Large date picker. Secondary option: "I do not have a due date yet — calculate from my last period" which switches to LMP date picker with Naegele's calculation. After entry: "You can update this at any time in settings."

**Screen 4 — Quick preferences (single screen):**
- "Are you expecting more than one baby?" Toggle: One baby / Twins / Triplets or more
- "I am..." Toggle: The pregnant person / A partner or birth companion
- "Are you 16 or over?" Toggle: Yes / No

**Screen 5 — Dashboard:**
Immediately show current gestational week, countdown, term window, and feature access. Onboarding complete.

### 9.2 Design principles

**SED-UX-001:** Clean, calm, uncluttered. Prioritise readability and ease of use. Especially during labour: large tap targets, high contrast, minimal cognitive load.

**SED-UX-002:** All interactive elements MUST have minimum 44x44 point tap targets (WCAG 2.1 AA compliance). Timer/counter buttons: minimum 80x80 points. Labour Mode buttons: minimum 120x120 points.

**SED-UX-003:** Typography: system fonts (San Francisco on iOS, Roboto on Android) for performance and familiarity. Clear hierarchy: title (24pt bold), heading (18pt semibold), body (16pt regular), caption (14pt regular).

**SED-UX-004:** Spacing: generous. Minimum 16pt padding on all content areas. 24pt between sections. Cards with 16pt internal padding and 8pt border radius.

**SED-UX-005:** Clinical signposting banners: warm amber background (not clinical red or yellow), clear readable text, non-dismissible, positioned at the top of the relevant screen before any interactive content.

**SED-UX-006:** Pro gate: locked features show a subtle lock icon overlay. Tapping opens the Pro unlock screen. No aggressive upsell, no popups, no "limited time offer" tactics. Single calm screen: "Unlock all tools — £4.99 one-time" with feature list and purchase button.

### 9.3 Navigation

Bottom tab bar with four tabs:

| Tab | Icon (Lucide) | Label | Contents |
|---|---|---|---|
| 1 | `baby` or `heart` | Home | Dashboard: week card, countdown, term window, quick links |
| 2 | `timer` | Tools | Grid: Contraction timer, Kick counter, Weight tracker |
| 3 | `clipboard-check` | Lists | Grid: Hospital bag, To-dos, Appointments |
| 4 | `menu` | More | Birth plan, Baby names, Your wellbeing, Settings, About, Disclaimer, Privacy |

Pro-gated tools show a small lock badge on their grid tile. Tapping navigates to the tool with the Pro gate overlay if not unlocked.

---

## 10. App store compliance

### 10.1 Google Play Store

**SED-STORE-001:** Complete the Health Apps Declaration Form. Declare: health data processed on-device only, no data transmitted, app is not a medical device, medical disclaimer presented on first launch.

**SED-STORE-002:** Target SDK API 35 (Android 15) or higher.

**SED-STORE-003:** Store listing must include medical disclaimer text in "About this app".

**SED-STORE-004:** Content rating: IARC, likely "Everyone" with health information content descriptor.

**SED-STORE-005:** Listing copy must include: "100% offline — your data never leaves your phone", "No ads, no subscriptions", "NHS-aligned content for UK parents", and the medical disclaimer.

### 10.2 Apple App Store

**SED-STORE-006:** App Privacy nutrition label: "Data Not Collected" for all health categories. Declare purchase data if RevenueCat collects purchase identifiers.

**SED-STORE-007:** Include medical disclaimer in App Store description.

**SED-STORE-008:** Comply with App Store Review Guidelines 5.1.1 (Health, Medical, and Human Subject Research) — the app provides organisational tools, not medical advice.

---

## 11. Build plan and phased delivery

### 11.1 Phases (60-day target)

**Phase 1: Foundation (days 1-7)** [Christian]

- Initialise Expo project with TypeScript
- Install and configure: expo-router, nativewind, gluestack-ui, react-native-mmkv, zustand, lucide-react-native
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

- Host privacy policy at public URL
- Finalise medical disclaimer text
- Write app store listings (description, feature list, screenshots, feature graphic)
- Generate screenshots for both platforms
- Google Play submission (Health Apps Declaration, content rating)
- Apple App Store submission (may extend 1-2 weeks beyond day 60 due to review times)

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

**Step 3: Install core dependencies.**

```bash
npx expo install expo-router react-native-mmkv zustand nativewind tailwindcss react-native-reanimated react-native-safe-area-context react-native-screens react-native-gesture-handler
npx expo install lucide-react-native react-native-svg
npx expo install react-native-gifted-charts react-native-linear-gradient
npx expo install expo-notifications expo-print expo-keep-awake expo-haptics expo-linking expo-splash-screen
npm install react-native-purchases  # RevenueCat
npm install zustand-mmkv-storage     # Zustand MMKV adapter (verify package name; may need manual adapter as shown in Section 6.5)
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
// eas.json
{
  "cli": { "version": ">= 13.0.0" },
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
        "track": "production"
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

### 12.6 app.json configuration

```json
{
  "expo": {
    "name": "Seed",
    "slug": "seed-maternity-toolkit",
    "version": "1.0.0",
    "orientation": "portrait",
    "icon": "./assets/icon.png",
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
      "buildNumber": "1",
      "infoPlist": {
        "NSUserTrackingUsageDescription": "This app does not track you. This permission is not used."
      }
    },
    "android": {
      "adaptiveIcon": {
        "foregroundImage": "./assets/adaptive-icon.png",
        "backgroundColor": "#FDF8F4"
      },
      "package": "com.yourcompany.seed",
      "versionCode": 1,
      "permissions": []
    },
    "plugins": [
      "expo-router",
      "expo-notifications",
      [
        "react-native-mmkv",
        { "accessGroups": [] }
      ]
    ],
    "scheme": "seed"
  }
}
```

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
1. **Privacy as architecture, not a promise.** Your data is encrypted on your phone. It never touches a server. We cannot see it even if we wanted to.
2. **Built for the UK.** NHS pathways, midwife-first language, stones and pounds, your antenatal schedule.
3. **Designed for reality.** Partner mode for birth companions. Twin support because you deserve more than an afterthought. A pregnancy loss pathway because not every pregnancy ends as hoped.
4. **Clinical quality without being a medical device.** Every piece of content reviewed by a senior NHS clinician. Every disclaimer honest and clear.

---

## 14. Success metrics

| Metric | Target (year one) | Measurement |
|---|---|---|
| Google Play downloads (free) | 15,000+ | Play Console |
| Pro conversion rate | 5%+ | RevenueCat |
| Net revenue | £3,000+ | RevenueCat + Play Console |
| Play Store rating | 4.5+ stars | Play Console |
| iOS App Store downloads | 5,000+ (first 6 months post-launch) | App Store Connect |
| Clinical content accuracy | 100% | CSO review log |
| Pregnancy loss complaints | 0 | Store reviews |
| Privacy / data complaints | 0 | Store reviews |
| Prohibited term violations | 0 | Pre-release text audit |

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
    "nhs_link": "https://www.nhs.uk/pregnancy/week-by-week/1-to-12/4-weeks/",
    "illustration_key": "week_04_poppy_seed"
  }
]
```

This schema repeats for all 42 weeks (4-42, plus weeks 1-3 as a "before you knew" informational entry if desired). [Jon] is responsible for populating all clinical content fields. [Christian] builds the UI to render whatever data is present, gracefully handling null fields.

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
    "gender": "girl",
    "origin": "Latin",
    "meaning": "Olive tree",
    "ons_current_rank": 1,
    "ons_trend": [3, 2, 1, 1, 1, 1, 1, 1, 1, 1],
    "trend_direction": "stable_high"
  },
  {
    "name": "Noah",
    "gender": "boy",
    "origin": "Hebrew",
    "meaning": "Rest, comfort",
    "ons_current_rank": 1,
    "ons_trend": [10, 7, 4, 2, 1, 1, 1, 1, 1, 1],
    "trend_direction": "rising"
  }
]
```

`ons_trend` is an array of annual rankings over the last 10 years (oldest to newest). `trend_direction` is computed: "rising" (rank improved by >20 positions over 5 years), "falling" (rank dropped by >20), "stable_high" (consistently top 200), "stable" (all others).

---

## 17. Appendix B: Key external references

**NHS and NICE:**
- NHS Week-by-week guide to pregnancy: https://www.nhs.uk/pregnancy/week-by-week/
- NHS Hospital bag checklist: https://www.nhs.uk/pregnancy/labour-and-birth/preparing-for-the-birth/pack-your-bag-for-labour/
- NHS Birth plan: https://www.nhs.uk/pregnancy/labour-and-birth/preparing-for-the-birth/how-to-make-a-birth-plan/
- NHS Signs of labour: https://www.nhs.uk/pregnancy/labour-and-birth/signs-of-labour/signs-that-labour-has-begun/
- NHS Mental health in pregnancy: https://www.nhs.uk/pregnancy/keeping-well/mental-health/
- NICE NG201 Antenatal care: https://www.nice.org.uk/guidance/ng201
- NICE NG137 Twin and triplet pregnancy: https://www.nice.org.uk/guidance/ng137
- NICE NG229 Intrapartum care: https://www.nice.org.uk/guidance/ng229
- NICE NG232 Caesarean birth: https://www.nice.org.uk/guidance/ng232

**Charities and support organisations:**
- Tommy's fetal movements: https://www.tommys.org/pregnancy-information/symptoms-and-complications/baby-movements-in-pregnancy
- Tommy's helpline: 0800 014 7800 / https://www.tommys.org
- Miscarriage Association: https://www.miscarriageassociation.org.uk / Helpline: 01924 200 799
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

*Document version: 2.0. Authors: Jonathan Watchorn (CSO) and Claude (Anthropic). For clinical content sign-off: Jonathan Watchorn. For technical implementation: Christian Baverstock using Claude Code.*

*This document is the single source of truth for Seed. Where any conflict exists between this document and any other material (research documents, conversation notes, earlier drafts), this document takes precedence.*

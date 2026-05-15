# PRD v2.2 → v2.3 Changelog

**Date:** 15 May 2026
**Source:** Third multi-agent research pass — 12 parallel agents covering voice-of-user research, every feature F01–F10 in depth, partner / twin / loss / wellbeing cross-cutting modes, daily-ritual & retention, and two new feature categories (F11 postnatal, F12 nutrition/lifestyle).

The 12 agent reports are preserved under `research/12_*` through `research/23_*`.

---

## Critical content fixes (must-do, blocking)

### 1. Appendix A.3 birth-plan option still used "NICE recommends"

v2.2 fixed all the body-text "recommends" → "advises" instances but missed `data/birth-plan-options.json` example at line 2868. v2.3 fixes:
- Before: `"NICE recommends it as an option for pain management."`
- After: `"The NHS lists it as an option for pain relief (NICE CG190 / NG229)."`
- This was a hard SED-SAF-005 violation; the `scripts/audit-terms.js` CI would have failed.

### 2. F04 cold-drink prompt — actively contraindicated by Tommy's

The original F04 deep-dive brief proposed "Have you eaten / had a cold drink / lain down?" prompts on the session-start screen. v2.3 **excludes the cold-drink prompt entirely** — Tommy's Movements Matter campaign exists specifically to combat this myth: telling worried users to "try a cold drink first" delays clinical presentation and creates false reassurance. v2.3 codifies this as **SED-F04-ANTI** and adds it to the **SED-UX-027 anti-pattern catalogue (AP-21)**. CI audit script extended.

### 3. F04 zero-movement save copy softened

Before (SED-EDGE-004): *"Session saved with 0 movements."*
After (SED-F04-024): *"Saved. If you're worried your baby is moving less than usual, or differently from their usual pattern, please call your maternity unit. Don't wait."* Identical regulatory behaviour, kinder surface at the moment of fright.

---

## New feature categories

### F11 Postnatal toolkit (v1.1 — full spec in v2.3)

40 new requirements (SED-F11-001 through 040). Bright line is tighter for F11 than F01–F10 because newborn safety and postnatal psychosis are high-stakes regulatory tripwires. Highlights:
- `lifecycleStage: 'pregnancy' | 'postnatal' | 'memorial'` (SED-CC-035)
- 6-step postnatal onboarding (date of birth + birth mode + sensitivity flag)
- Days-since-birth dashboard (SED-F11-005)
- Postnatal check schedule per NICE NG194 + UKHSA universal-five (SED-F11-007)
- Count-only feeding log with inclusive methods (breast/chest, expressed, formula, donor, combination, tube) (SED-F11-008/009)
- Count-only nappy log (SED-F11-011)
- Per-baby logging for twins (SED-F11-012)
- Static physical-recovery content (SED-F11-013), with first-class caesarean recovery (SED-F11-014)
- Mental wellbeing signposting-only — SED-CC-017 PHQ-9/EPDS/GAD-7 ban extended (SED-F11-015/016)
- Postnatal psychosis card prominently placed (SED-F11-017)
- Harm-thoughts emergency wording verbatim (SED-F11-018)
- Lullaby Trust safer-sleep card pinned (SED-F11-019/020)
- Memorial mode + Memories PDF (SED-F11-024/025/031)
- NICU pathway with Bliss signposting (SED-F11-026)
- **Dual clinical sign-off requirement** (SED-F11-033) — perinatal-MH-trained second reviewer in addition to CSO
- Grade 6–7 reading age (lower than SED-CONTENT-001 baseline) (SED-F11-034)
- **MHRA Regulatory Advice meeting booked pre-v1.1 GA** (SED-F11-040)

Effort: 32–45 days total; realistic v1.1 GA at 8–12 weeks post v1.0 launch.

### F12 "What the NHS advises" — food/drink/lifestyle (v1.0-lite + v1.0.x full)

15 new requirements. Verbatim NHS reference library. v1.0-lite (1 dev-day) ships as a single static screen with 12 NHS deep-links; full F12 (~106 entries with search) ships v1.0.x. Free tier. Highlights:
- Three classification chips (SED-F12-004): "The NHS advises avoiding" / "The NHS advises limiting" / "The NHS says you can usually" — "safe/unsafe" framing banned (SED-SAF-005 + SED-F12-006)
- No calculator, no totaliser, no diary, no per-user filter (SED-F12-005)
- Healthy Start signposting prominent (SED-F12-008) — public-health intervention with ~171k eligible UK families missing out
- BUMPS integration for OTC medicines (SED-F12-009)
- Eating-disorder sensitivity flag (SED-F12-011) and hyperemesis flag (SED-F12-012) with corresponding content reframing

Categories at full launch: foods to avoid (~25); drink incl. alcohol and caffeine reference card (~10); vitamins and supplements (~8); OTC medicines (~12); herbal remedies (~6); activity (~12); hair/beauty (~8); sex (~3); travel (~8); vaccinations 2026/27 schedule (~5); pets/toxoplasmosis (~3); Healthy Start (1); common-scenario reassurance (~5).

---

## Anti-pattern catalogue — engagement-farming explicitly prohibited

**SED-UX-027** introduces 30 explicit anti-patterns (AP-01 through AP-30) — engagement, manipulation, social, telemetry, clinical-interpretation, advertising, and AI-analysis patterns that are forbidden at any version of Seed. CI / code review must flag attempts. Highlights:
- AP-01 Streaks; AP-02 Leaderboards; AP-03/04 Engagement push notifications
- AP-06 Threshold alerts on user data; AP-09 Mood/wellbeing tracker
- AP-14 Behaviour-targeted Pro pitch; AP-15 Endless scroll
- AP-21 F04 cold-drink prompt (Tommy's contraindicates)
- AP-23 Crying tracker; AP-24 AI photo analysis
- AP-25 Comparative milestone tracker for baby
- AP-27 Weight centile plot for baby weight
- AP-29 "Your mood seems low" inferred from free-text
- AP-30 A/B test on clinical content without CSO sign-off

Encodes the engagement-farming prohibition that until v2.3 was implicit in §18 non-roadmap. Makes it CI-enforceable.

---

## Memorial state lifecycle + pregnancy-after-loss lite

- **SED-CC-035** `lifecycleStage: 'pregnancy' | 'postnatal' | 'memorial'` is now a first-class state, orthogonal to `trackingStatus`
- **SED-CC-036** "Save a record" memorial page — quiet typographic composition, optional photo (locally stored), optional anniversary acknowledgement
- **SED-CC-031** previous-loss declaration in onboarding (optional) triggers `birthSensitivity = 'trauma_aware'`
- **SED-CC-032** sensitive mode toggle (independent of previous-loss declaration)
- **SED-CC-033** soft pause "while I find out" — fifth option on compassionate screen for the Schrödinger's-pregnancy state v2.2 didn't anticipate
- **SED-CC-034** gestation-aware charity highlighting on the compassionate screen
- **SED-CC-038** lost-one-of-twins branch with "are you continuing your pregnancy with the remaining baby?"
- **SED-CC-039** TFMR-specific compassionate-screen line + ARC pinning when entry comes from anomaly-scan note

This is the most differentiating territory in v2.3 — no UK pregnancy app currently offers a memorial state or anniversary acknowledgement.

---

## Partner / IP / solo / same-sex couple paths (new structural shape)

Partner mode is no longer just a content filter. **SED-CC-004d** introduces a dedicated partner-mode Home tab layout. New fields:
- **SED-CC-004b** `pregnantPersonName`, `pregnantPersonPronouns`
- **SED-CC-004c** `partnerRole: 'co_parent' | 'birth_companion' | null` (same-sex co-parents are recognised; HFEA 2008 ss.42–43 framework)
- **SED-CC-004e** partner education content library — 9 sections × multiple cards (NHS / NCT / Tommy's / Sands / Lullaby Trust / Bliss / PANDAS sourced; gender-neutral throughout)
- **SED-CC-004f** surrogacy IP path — parental-order timeline (E/W in v1.0; Scotland v1.0.x), Brilliant Beginnings / Surrogacy UK / COTS / Cafcass signposting
- **SED-CC-004g** solo-parent path — `soloParent: true` flag, SMBC UK + Solo Mum Society + Donor Conception Network signposting
- **SED-CC-004h** donor-conception opt-in (v1.0.x)
- **SED-CC-004i/j** long-distance partner mode, older-children-in-household toggle

---

## Twin / multiples mode (substantially deepened)

- **SED-CC-022** updated: chorionicity captured in onboarding (was deferred to v1.1 in v2.2)
- **SED-CC-022a** twin welcome screen
- **SED-CC-022b** twin delivery-window display (around 37 wk DC/DA, 36 wk MC/DC, 35 wk MC/MA)
- **SED-CC-022c** week-specific twin callouts (14 high-value weeks; not just a generic line)
- **SED-CC-022d** multiples educational library — 12 cards covering MC/DC vs DC/DA, TTTS, sIUGR, feeding two newborns, NICU prep, twin birth options, maternity leave, financial reality, older siblings, emotional reality, vanishing twin, postnatal help
- **SED-CC-022e** Twins Trust as named partner with Settings card (helpline 0800 138 0509, antenatal course, bereavement)
- **SED-F09-017** DC/DA pre-populated schedule (8 appointments per NICE NG137) — v1.0
- **SED-F09-018** MC/DC pre-populated schedule (11 appointments, fortnightly from 16w per NICE NG137 TTTS surveillance) — v1.0 schedule preset (per-week clinical commentary stays v1.1)
- **SED-F09-019** "adjust to match your consultant" banner
- **SED-F06-013/014** twin-mode bag preset + NICU "go bag" sub-section
- **SED-F08-016** twin to-dos expanded to 12 items
- **SED-F07-022** twin birth-plan branch (mode of delivery, epidural prominence, twin B preferences, skin-to-skin sequence, NICU access for either baby)

---

## F01 / F02 dashboard transformation

F02 was the most-used surface in v2.2 but generic. v2.3 makes it profile-aware and capture-capable:
- **SED-F02-008** profile-aware rendering via `userJourneyProfile`
- **SED-F02-009** experience-level toggle ("This isn't my first pregnancy") — drops "you may not know you're pregnant yet" content
- **SED-F02-010** per-week diary (free text, MMKV-encrypted, excluded from default PDF share)
- **SED-F02-011** mood note (single sentence, NOT a tracker; CSO sign-off REQUIRED — borderline SED-CC-017)
- **SED-F02-012** appointment-questions list, pre-populated from F02 weekly card content
- **SED-F02-013** alternative size representations (fruit / object / measurement-only) — solves the gender-coded review complaint without dropping the iconic fruit comparison
- **SED-F02-014** next-week preview card
- **SED-F02-015** milestone log
- **SED-F02-016** daily prompt rotation (deterministic, not engagement-farmed)
- **SED-F02-017** viability acknowledgement at 24+0
- **SED-F02-018** labour-readiness home re-ordering from 36+0
- **SED-F02-019** share-this-week (PNG/PDF, diary excluded by default)
- **SED-F02-020** Memories PDF generation at week 37+ / lifecycle-stage transition
- **SED-F01-010** daily progression header ("Day 198 of your pregnancy")
- **SED-F01-011** EDD-change reconciliation screen
- **SED-F01-012/013** extended term window (37–42+ wide context; amber banner post-42)
- **SED-F01-014** extra-care self-declaration (optional, signposting-only, never management content)

---

## F03 Labour Mode + F04 Movement diary

F03 (10 new requirements + SED-F03-ANTI):
- **SED-F03-016** confirm-on-end modal (prevents accidental destructive tap)
- **SED-F03-017/018/019** enriched session summary + "Read to your midwife" card + call-now button on summary
- **SED-F03-020** partner-operated UI toggle (140×140 pt buttons; rotated static partner prompts on generic timer, never on contraction-interpretation)
- **SED-F03-021** rotation lock in Labour Mode
- **SED-F03-022** night auto-dim 22:00–06:00
- **SED-F03-023** battery banner from 30% / 15%
- **SED-F03-024** call-interruption resilience formal spec
- **SED-F03-025** "What I'm trying" diary toggles (TENS / Water / Breathing / Position / Massage)
- **SED-F03-026** "Start contraction timer" home-dashboard entry from week 36

F04 (12 new requirements + SED-F04-ANTI):
- **SED-F04-014** conceptual rename to "Movement diary" (RCOG GTG-57 2nd edition deprecates count-based framing)
- **SED-F04-015** session-start screen elevates signposting BEFORE the tap counter
- **SED-F04-016** "Haven't felt movement" static page with verbatim Tommy's Movements Matter guidance — explicitly debunks cold-drink myth
- **SED-F04-017** REVISED practical-prep: removed cold-drink prompt
- **SED-F04-018** per-session free-text note
- **SED-F04-019** time-of-day dot strip (v1.0.x; identical visual treatment for all days)
- **SED-F04-020** session duration in 7-day view
- **SED-F04-021** sensitive-mode toggle (v1.0.x)
- **SED-F04-022** twin mode with optional A/B/unsure toggle
- **SED-F04-023** email-to-midwife flow (v1.0.x)
- **SED-F04-024** softened zero-movement save copy
- **SED-F04-025** call-now quick-action button
- **SED-F04-ANTI** anti-requirements codified

---

## F05 weight tracker — sensitive defaults

Material change from v2.2: F05 is now **opt-in not opt-out**, and the reference band is **default-off**.
- **SED-F05-011** opt-in onboarding gate
- **SED-F05-012** 5-step wizard with explicit "We won't use this to calculate BMI or compare you to anyone else" copy on the pre-pregnancy weight step
- **SED-F05-013** write-only mode (no echo on save) for users triggered by seeing numbers
- **SED-F05-014** reference band default-OFF (was default-ON in v2.2)
- **SED-F05-015** per-entry notes
- **SED-F05-016** frequency framing (no nudges; F05 never sends weight reminders)
- **SED-F05-017** widget privacy guarantee (F05 data MUST NOT appear on home-screen widget)
- **SED-F05-018** PDF weight-record export (v1.0.x)
- **SED-F05-019** hyperemesis mode (v1.0.x)
- **SED-F05-020** multiples mode (reference band suppressed for twin/triplet)
- **SED-F05-021** BEAT signposting in F05 footer when chart hidden or write-only
- **SED-F05-022** **RECOMMEND moving F05 to Free tier** — gating ED-protective infrastructure behind £4.99 is the wrong shape. CTO/CSO decision required before v1.0 lock.

---

## F06 / F08 / F09 admin trio (substantial expansion)

F06 (9 new requirements): week-34 dashboard prompt; Labour-Mode grab-now subset; birth-partner micro-list with name; item-type chip; twin bag preset; NICU go-bag sub-section; week-38 panic-check panel; NHS public-info per-item links; section-packed silent caption.

F08 (9 new requirements): deadline-anchored items + urgency sort; F08↔F09 auto-tick; **UK maternity rights subsection — full 10-item spec with gov.uk / Acas / NHSBSA / HSE / Healthy Start links** (the largest content addition in v2.3); subsequent-pregnancy mode; custom-deadline-creates-reminder; FW8/Mat B1 prominence in T1; partner to-do list; twin items expansion.

F09 (12 new requirements): free-text appointment with custom brief; questions-to-ask per appointment; outcomes capture; add-to-phone-calendar (`expo-calendar` plumbing); configurable multi-reminder cadence; travel-time offset; appointment letter paste; recurring series; missed-appointment recovery; DC/DA twin schedule; MC/DC twin schedule; "adjust to match your consultant" banner.

---

## F07 birth plan (substantial new structure)

13 new requirements:
- **SED-F07-016** auto-save with version history (rolling 20 versions + 3 pinnable named versions)
- **SED-F07-017** "If things change" 5 sub-scenarios with pre-written first-person preferences (assisted VB, unplanned caesarean, general anaesthetic, neonatal care, postpartum haemorrhage)
- **SED-F07-018** structured cultural/religious section (modesty, prayer, dietary, cultural practices, interpreter)
- **SED-F07-019** structured disability/access needs section (mobility, communication, sensory, condition)
- **SED-F07-020** trauma-informed section (avoids "trauma" in heading; explicit consent prompts; BTA card)
- **SED-F07-021** VBAC pathway
- **SED-F07-022** twin birth-plan branch
- **SED-F07-023** must-haves max-5 tagging (forced prioritisation; renders on page 1 of PDF)
- **SED-F07-024** Labour-Mode quick reference (must-haves only, glance-readable)
- **SED-F07-025** birth-partner companion PDF (separate ≤2-page output with action prompts)
- **SED-F07-026** multi-page PDF architecture (must-haves first; full preferences; if-things-change last; metadata footer)
- **SED-F07-027** uninterrupted skin-to-skin preference checkable
- **SED-F07-028** print + email-to-midwife from app

Plus content fix: Appendix A.3 "NICE recommends" → "The NHS lists" (was the only surviving SED-SAF-005 violation in PRD content examples).

---

## Mental wellbeing library (4 organisations → 18 cards)

**SED-CC-017a** expands §4.4 from 4 named organisations (NHS, PANDAS, Samaritans, MMHA) to a topic-card library of ~18 cards: antenatal anxiety; antenatal/perinatal depression; **perinatal OCD and intrusive thoughts** (Maternal OCD, OCDUK, RCPsych); **postnatal psychosis** (Action on Postpartum Psychosis); **tokophobia** (NHS pathway); PTSD from previous birth (Birth Trauma Association); bonding worries; hyperemesis-related distress (Pregnancy Sickness Support); ED relapse risk (BEAT); bipolar in pregnancy; **Black maternal mental health** (The Motherhood Group, Muma Nurture, MMHA Black MH Project); telling your midwife — what happens; partner mental health; mental health after a previous loss; sleep and mental health; when to call urgent help. All Tier-1 sourced.

Plus **SED-CC-017b** "your midwife will not judge you" disclosure framing; **SED-CC-017c** NHS 111 option 2 in universal crisis footer.

---

## Universal "Call your maternity unit" footer

**SED-CC-030** — persistent footer accessible from every screen (not just F03/F04 banners), dialling the user-stored maternity unit number. Voice-of-user research identified this as the highest-leverage missing surface — pregnant users at unpredictable worry moments need a 1-tap path to their unit. Already exists in F03/F04; v2.3 universalises it.

---

## Pro conversion + retention discipline

**SED-UX-028** — Pro upsell limited to exactly 5 permitted touchpoints (locked-tile first-touch, week 28 banner, week 32 banner, F09 brief preview, Restore equal prominence). All other Pro surfaces forbidden per SED-UX-027 AP-07/08/14/19.

**SED-UX-026** — 60-second time-to-first-value flow with a value-confirmation beat between onboarding Screen 4 and Screen 5: *"You're 22 weeks and 3 days. Your baby is about the size of a papaya. Welcome."*

---

## New SED-* requirements (v2.3 total: ~120 net)

By prefix:
- **SED-CC-022a–e, 004b/c/d/e/f/g/h/i/j, 017a/b/c, 030, 031, 032, 033, 034, 035, 036, 037, 038, 039**
- **SED-F01-010 to 014**
- **SED-F02-008 to 020**
- **SED-F03-016 to 026, ANTI**
- **SED-F04-014 to 025, ANTI**
- **SED-F05-011 to 022**
- **SED-F06-009 to 017**
- **SED-F07-016 to 028**
- **SED-F08-008 to 016**
- **SED-F09-008 to 019**
- **SED-F10-PREVIEW-001**
- **SED-F11-001 to 040** (new feature category)
- **SED-F12-001 to 015** (new feature category)
- **SED-UX-026, 027 (AP-01..30), 028**

---

## What did NOT change

- Architecture: 100% offline, MMKV-encrypted, no telemetry, no accounts, no remote config, no EAS Update
- Regulatory bright line: notepad-not-device principle; SED-SAF-001a intended-purpose statement
- £4.99 one-time price; ~£3.53 net per sale (v2.2 corrected arithmetic)
- WCAG 2.2 AA accessibility target
- 100% Tier-1 UK content sourcing (NHS / NICE / RCOG / Tommy's)
- Document conventions
- All v2.2 critical fixes (encryption-key bootstrap, SettingsState reconciliation, pricing math) remain locked

---

## File deliverables

- `Seed_Maternity_Toolkit_PRD_v2_3.md` — full updated PRD
- `PRD_v2_2_to_v2_3_CHANGELOG.md` — this file
- `research/12_*` through `research/23_*` — 12 v2.3 agent reports preserved
- Prior versions (v2.0, v2.1, v2.2) retained unchanged for reference; prior changelogs retained

---

*v2.3 closes the feature-optimisation pass started by the user request "use multi agents to optimise the features ... think about what people want but also need". The three multi-agent passes (v2.0→v2.1 regulatory/technical, v2.1→v2.2 audit/operational, v2.2→v2.3 feature optimisation) have together transformed the PRD from a sound regulatory document into an implementable spec with depth across every feature, two new feature categories, an anti-pattern catalogue, and a memorial-state lifecycle that no UK pregnancy app currently offers.*

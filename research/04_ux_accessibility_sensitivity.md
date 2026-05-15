# Seed Maternity Toolkit — UX, Accessibility, Sensitivity and Inclusion Audit

**PRD version reviewed:** v2.0, February 2026

The PRD is unusually disciplined for a v1.0 spec — the regulatory bright line, offline architecture and pregnancy-loss pathway are all strong. The audit focuses on gaps in accessibility specificity, inclusive language, contrast, and the postnatal scope.

---

## 1. WCAG 2.2 AA conformance matrix

PRD references **WCAG 2.1 AA**. WCAG 2.2 (October 2023) adds 9 new SC (6 AA). Upgrade to **WCAG 2.2 Level AA**.

| SC | Name | Level | PRD coverage | Gap |
|---|---|---|---|---|
| 1.4.3 | Contrast (Minimum) | AA | Asserted but not measured | **Fails** for sage primary, text-tertiary, secondary tan |
| 1.4.4 | Resize Text 200% | AA | Not addressed | Dynamic Type requirement needed |
| 1.4.10 | Reflow | AA | Not addressed | 320 CSS px / 400% zoom |
| 1.4.11 | Non-text Contrast | AA | Not measured | Banner border `#E6A817` on `#FFF3E0` = 1.92:1 — fails 3:1 |
| 1.4.12 | Text Spacing | AA | Not addressed | Layouts must not break at increased spacing |
| **2.4.11** | **Focus Not Obscured** | **AA — new in 2.2** | Not addressed | Sticky banners must not cover focused control |
| **2.5.7** | **Dragging Movements** | **AA — new in 2.2** | Partial | Long-press to delete in F05 needs single-tap alternative |
| **2.5.8** | **Target Size (Minimum)** | **AA — new in 2.2** | Already met | 44pt minimum exceeds 24×24 CSS px |
| 3.2.6 | Consistent Help | A — new in 2.2 | Not addressed | Call maternity unit / pause / disclaimer in consistent location |
| **3.3.7** | **Redundant Entry** | **A — new in 2.2** | Partial | Birth plan re-asks user's name |
| 4.1.3 | Status Messages | AA | Not addressed | Use `accessibilityLiveRegion="polite"` |

---

## 2. Colour palette audit (Section 6.7)

| Pair | Ratio | Required | Result |
|---|---:|---:|---|
| `#2D2A26` textPrimary on `#FDF8F4` | 13.54:1 | 4.5 | PASS |
| `#6B6560` textSecondary on `#FDF8F4` | 5.45:1 | 4.5 | PASS |
| `#9B9590` textTertiary on `#FDF8F4` | **2.81:1** | 4.5 | **FAIL** |
| `#7B9E87` sage primary as text | **2.81:1** | 4.5 | **FAIL** |
| `#FFFFFF` on `#7B9E87` primary button | **2.96:1** | 4.5 (3 for large/UI) | **FAIL** |
| `#5C7A66` primaryDark | 4.49:1 | 4.5 | Marginal — fails by 0.01 |
| `#C4A882` secondary tan | **2.15:1** | 4.5 | **FAIL** |
| `#8B6F47` proGate | 4.46:1 | 4.5 | Marginal |
| `#FFF3E0` banner bg with `#5D4037` text | 8.50:1 | 4.5 | PASS |
| `#E6A817` banner border on `#FFF3E0` | **1.92:1** | 3.0 (non-text) | **FAIL** |
| Labour Mode `#F0EDE8` on `#1A1A2E` | 14.61:1 | 4.5 | PASS |

### Proposed palette fixes

```ts
textTertiary:   '#767670',  // 4.34:1 — large-text/disabled only; #595550 (7.03:1) for body
primary:        '#4A6B53',  // 5.66:1; white-on-primary = 5.97:1
primaryDark:    '#3A5742',  // pressed state
secondary:      '#9C8050',  // tan darkened ~4.5:1
proGate:        '#7A5A38',  // 5.97:1
bannerBorder:   '#B07A00',  // darker amber, 3.0:1+ on #FFF3E0
```

---

## 3. Inclusive language fixes

| Location | Current | Proposed |
|---|---|---|
| §1.5 | "Pregnant women in the UK..." | "People who are pregnant in the UK... The majority are women; the app uses 'pregnant person' or 'you' throughout..." |
| §1.5 | "First-time mothers" | "First-time parents (most often mothers)" |
| §1.5 | "Women expecting twins" | "People expecting twins" |
| §3.7 step 8 | Feeding (breast/bottle/combination/undecided) | Add chestfeeding, expressing, donor milk, tube feeding |
| §3.10 F10 | gender (boy/girl/unisex) filter | "traditionally given to boys / girls / used for any gender"; rename type `nameStyle` |
| §4.1 SED-CC-001 | Binary partner toggle | 4 options: pregnant / partner-companion / surrogacy-intended-parent / shared-device, plus independent partner-content toggle for solo parents |
| §3.5 F05 | No body-shape sensitivity option | Add "Hide the chart" toggle |
| §F10 names type | `'boy' \| 'girl' \| 'unisex'` | `'masculine' \| 'feminine' \| 'unisex'` |
| §8.4 illustration brief | "diverse representation" (vague) | Explicit: skin tone, hair texture, body size, age, visible disability, family composition |

---

## 4. Pregnancy loss pathway enhancements

### Wording — add softer 4th option

> **"Hide pregnancy content"** — keep your data, hide weekly updates and reminders. Resume any time.

### Charity coverage — add

| Charity | Coverage |
|---|---|
| Ectopic Pregnancy Trust | Ectopic and PUL — Helpline 020 7733 2653 |
| Petals | Specialist counselling after baby loss |
| Aching Arms | Baby loss at any stage |
| **ARC** (Antenatal Results and Choices) | **TFMR — critical and currently missing** — Helpline 0207 713 7486 |
| Twins Trust Bereavement Support Group | Loss of one or both twins/multiples |
| Mariposa Trust / Saying Goodbye | Loss support and remembrance |
| Lily Mae Foundation | Stillbirth, neonatal death, miscarriage |
| Child Bereavement UK | When a baby dies, including sibling support |

### Notification suppression — enumerate all categories

Add SED-CC-018 specifying that when paused/ended, all categories of notifications are cancelled (appointment reminders, due date updates, due-date-after-scan reminders, hospital-bag nudges, weekly-card reminders).

### Notification copy

Ban anthropomorphised foetus language: "Your baby is the size of an aubergine!" → "Week 28 information is available."

---

## 5. Partner mode inclusivity expansion

### Proposed onboarding question

> **Who is using this device?**
> - I'm pregnant
> - I'm a partner, co-parent or birth companion
> - I'm an intended parent following a surrogate's pregnancy
> - We share this phone — let me pick each time

### Downstream impacts

- **Surrogacy:** suppress body/symptom content; keep weekly development; suppress contraction timer and kick counter as primary tools; phrasing "the person carrying your baby".
- **Same-sex female partners:** all partner copy gender-neutral; audit `partner_content` JSON for "dad", "father".
- **Trans / non-binary partners:** all partner copy gender-neutral.
- **Solo parents:** independent toggle to hide partner-companion content entirely.

---

## 6. Postnatal gap

§1.1 says "pregnancy and postnatal toolkit" but F01–F10 are all antenatal/intrapartum. Store-listing mismatch risk.

**Option A (preferred for v1.0):** Strike "postnatal" from §1.1, §1.3, app title, store listings, ASO. Mark postnatal as **v1.1 scope** explicitly.

**Option B (if leadership wants postnatal at launch):** F11 module:
- F11.1 "Days since birth" counter
- F11.2 Postnatal check schedule (6-week GP check, health visitor)
- F11.3 Feeding log (count only)
- F11.4 Nappy log (count only)
- F11.5 Postnatal wellbeing signposting (PANDAS, Birth Trauma Association)
- F11.6 Static NHS content (lochia, perineal care, c-section recovery)

Option B adds 15–20 dev-days.

---

## 7. Screen reader, dynamic type, motion, voice control

### Proposed SED-A11Y-001 to 011

- **001 Labels:** every interactive element has non-empty `accessibilityLabel`
- **002 Roles:** every actionable control sets `accessibilityRole`
- **003 States:** stateful controls set `accessibilityState`
- **004 Grouping:** grouped content uses `accessibilityRole="group"`
- **005 Live regions:** timer updates `polite`; session-end `assertive`
- **006 Decorative imagery:** fruit illustrations `accessibilityElementsHidden`
- **007–009 Dynamic Type:** `allowFontScaling={false}` forbidden; tolerate 200% scale
- **010 Reduced motion:** check `AccessibilityInfo.isReduceMotionEnabled()`; add "Disable haptics" toggle
- **011 Voice/Switch Control:** every button has unique speakable label

---

## 8. Onboarding sensitivity (Section 9.1)

### Screen 3 — replace "your baby" with "your pregnancy" + third option

> **When is your pregnancy due?**
>
> [Date picker]
>
> I don't have a date yet — calculate from my last period
>
> I'd rather not enter this now
>
> You can update or remove this any time in Settings. It's normal for the date to change after a dating scan.

### Screen 4 — clarify age question is not a gate

"Are you 16 or over?" — hint: "We ask so we don't show certain content to under-16s. The app works either way."

---

## 9. Reading age / Plain English

Add **SED-CONTENT-001**: All copy targets reading age 9–11 (NHS service manual aligned). Hemingway grade 6. Sentences ≤15 words average.

Apply immediately:
- SED-SAF-007: "Using this app does not create a clinical relationship..." → "Using this app doesn't make us your healthcare team."

---

## 10. Localisation, dark mode, disclaimer, PDF, kick counter

- **Localisation:** PRD silent on Welsh, Polish, Urdu, Punjabi, Bengali, Romanian, Arabic. v1.0 English-only is defensible. Externalise strings to `data/copy.en-GB.json`. Welsh v1.2.
- **Dark mode:** PRD specs Labour Mode only. Add full system dark mode (SED-UX-018) or risk-register entry for v1.1.
- **Disclaimer:** scroll-to-bottom gate has screen-reader issues. When VoiceOver/TalkBack active, enable button after full announcement. Add TTS "Read to me" option.
- **Birth plan PDF (F07):** mandate tagged-PDF (`<h1>` not styled divs), large print toggle, Atkinson Hyperlegible/Inter font, predictable filename, footer disclaimer.
- **Kick counter (F04):** confirm no mean/median lines, no trend arrows, no colour coding. Add SED-F04-013.

---

## 11. New requirements (numbered)

- **SED-UX-008** WCAG 2.2 AA replaces 2.1 AA
- **SED-UX-009** Focus visible (SC 2.4.7, 2.4.11)
- **SED-UX-010** Dragging alternative (SC 2.5.7)
- **SED-UX-011** Consistent help (SC 3.2.6)
- **SED-UX-012** Redundant entry (SC 3.3.7)
- **SED-UX-013** Status messages (SC 4.1.3)
- **SED-UX-014** Reflow (SC 1.4.10)
- **SED-UX-015** Text spacing (SC 1.4.12)
- **SED-UX-016** 44pt confirmation (SC 2.5.8)
- **SED-UX-017** Measured contrast in code comments + unit test
- **SED-UX-018** System dark mode
- **SED-A11Y-001 to 011** As listed
- **SED-CONTENT-001** Reading age 9–11
- **SED-CONTENT-002** UK English locale file
- **SED-CC-018** Total notification suppression while paused
- **SED-CC-019** "Hide pregnancy content" softer option
- **SED-CC-020** Universal crisis footer on free-text inputs
- **SED-CC-021** Surrogacy / solo-parent options in onboarding
- **SED-F04-013** No comparative visual cues in 7-day view
- **SED-F05-008** Hide-the-chart option
- **SED-F07-009 to 012** Tagged PDF, large print, accessible font, predictable filename
- **SED-F11-001 onwards** (if Option B for postnatal)

---

## Summary

Five priority gaps:

1. **WCAG 2.1 → 2.2 AA upgrade** with several SC not yet covered.
2. **Colour palette fails 1.4.3** in three places — concrete replacements supplied.
3. **Pregnancy-loss pathway** undercounts UK loss charities; notification suppression must enumerate every category.
4. **Partner mode** needs four-option onboarding (pregnant / partner / surrogacy-intended-parent / shared device) plus solo-parent toggle.
5. **"Postnatal toolkit" claim** unsupported in feature set — either remove the wording until v1.1 or scope F11.

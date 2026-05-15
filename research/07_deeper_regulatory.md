# Seed PRD v2.1 — Second-Pass Deeper Regulatory Review

## Corrections to v2.1

### C1. Birth Trauma Association domain
v2.1 line 744: `birthtraumaassociation.org.uk` → correct domain is **`birthtraumaassociation.org`** (the .org.uk parks/redirects).

### C2. Mariposa rebrand (October 2024)
v2.1 line 746 "The Mariposa Trust / Saying Goodbye" → **"Mariposa International (parent charity of Saying Goodbye)"**.

### C3. Google Play disclaimer wording placement
v2.1 SED-STORE-001 has the disclaimer text but Google's policy mandates it appears in the **first paragraph** of the Play Store long description. Add placement requirement.

### C4. App Store Connect declaration field labels finalised (March 2026)
The form field is binary "Yes/No"; Apple publicly displays "Not a regulated medical device" but the form value is just "No". v2.1 SED-STORE-009 wording is functionally correct but mentions the public-facing label not the form field.

## New requirements

### N1. SED-PRIV-013 — DPIA-lite documentation
ICO mandatory DPIA list includes "innovative technology + sensitive data + vulnerable subjects" (pregnant women). Even though the developer is not a controller of on-device data per SED-PRIV-007, doing a lightweight DPIA costs ~1 day and removes a litigation argument. Document at `/docs/legal/DPIA-v1.md`.

### N2. SED-SEC-001 — Vulnerability disclosure
Publish `SECURITY.md` at repo root + `security.txt` at `seed.health/.well-known/security.txt` per RFC 9116. Coordinated-disclosure policy at `seed.health/security/policy` modelled on NCSC Vulnerability Disclosure Toolkit V2.

### N3. SED-STORE-011 — Store-suspension contingency
Apple's App Store Improvements / dispute process can suspend a Health & Fitness app with minimal notice. Document the playbook: communication template, appeal paths, commercial impact assessment.

### N4. SED-PRIV-014 — RevenueCat breach playbook
RevenueCat's DPA commits to notify customers on confirmed breach. The company must: assess Article 33 trigger within 72h, notify ICO if engaged, post to seed.health/status, in-app banner via next release.

### N5. SED-CONTENT-001 — Verbatim NHS reproduction policy
Crown Copyright + OGL v3.0 expressly permit verbatim reproduction with attribution. v2.1's "MUST be rewritten" requirement adds clinical-review burden without legal benefit and introduces drift risk. Replace with: MAY be verbatim, MUST attribute, MUST list in `data/provenance.json`.

### N6. SED-NICE-ESF-001 — Tier-A self-classification
NICE ESF Tier A = system service / information only. Seed sits in Tier A. v1.1 postnatal module risks drift into Tier B (simple monitoring); must stay count-only.

### N7. SED-MHRA-001 — Innovation Office policy
MHRA Regulatory Advice meeting £987 + VAT one hour. NO consultation for v1.0 (Seed inside the bright line). Book before any v1.1+ feature arguably crosses the line.

### N8. SED-PRIV-015 — Children's Code post-DUAA monitoring
Data (Use and Access) Act 2025 in force 5 Feb 2026. ICO publishing revised guidance during 2026. Re-verify within 30 days of ICO publication.

### N9. SED-LEG-007 — Companies House publisher verification
App Store Connect / Play Console publisher name MUST match Companies House registered name. Mismatches trigger DUNS verification delays of 1–4 weeks.

## Open verification items

- UK IPO trademark search for "Seed" in classes 9, 10, 41, 44 — likely contested (Seed Health US probiotics brand)
- Companies House entity in good standing
- RevenueCat data residency confirmation
- ICO age-assurance joint statement (early 2026)

## All charity helplines verified

- Tommy's 0800 014 7800 ✓
- Sands 0808 164 3332 ✓
- PANDAS 0808 1961 776 ✓
- Samaritans 116 123 ✓
- Miscarriage Association 0303 003 6464 ✓ (v2.1 corrected)
- ARC 020 7713 7486 ✓
- Ectopic Pregnancy Trust 020 7733 2653 ✓
- Aching Arms (registered 1153296) ✓
- Child Bereavement UK (1040419) ✓
- Petals ✓
- Twins Trust Bereavement ✓

NICE guideline IDs and dates confirmed (NG201, NG137 updated April 2024, NG229 Fetal monitoring, NG192 Caesarean updated June/July 2025).

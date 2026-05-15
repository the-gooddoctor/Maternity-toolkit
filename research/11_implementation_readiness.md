# Seed PRD v2.1 — Implementation Readiness Review (Developer Perspective)

**Headline:** v2.1 is unbuildable in 60 days by one person at full scope. Realistic envelope is **81-115 dev-days**. A v1.0 / v1.0.x / v1.1 cut list recovers 60 days.

## Spec ambiguities to resolve (15 questions with proposed answers)

| # | Ambiguity | Proposed answer |
|---|---|---|
| A1 | §6.5 "8 stores" but 9 listed including mmkvStorage | mmkvStorage is infrastructure; "8 feature stores plus mmkvStorage" |
| A2 | F02 JSON `partner_content` always present? | Always present; null when absent. Document null-field convention table |
| A3 | F09 pre-populate algorithm | Days-offset table keyed off gestational week (booking = EDD−224d etc) |
| A4 | F10 ONS data 10 or 20 years? | 10 years |
| A5 | F03 session summary exact wording | Template with `{N}`, `{X}`, `{Y}` substitution only |
| A6 | F02 twin content additive or replacement? | Additive — callout block appended |
| A7 | F09 Trust-specific test variant handling | Standard tail sentence: "Your local maternity unit may offer something slightly different" |
| A8 | A.4 `gender` vs `nameStyle` | Fix to `nameStyle: 'feminine'/'masculine'` |
| A9 | Appointment date+time storage UTC or local? | Local |
| A10 | EDD edit re-anchoring of pre-populated appointments | Re-prompt if EDD changes >7 days |
| A11 | SED-CC-018 notification scope | Only user-facing notifications, not internal re-prompts |
| A12 | F07 PDF format | Text bullet list grouped by section, semantic HTML |
| A13 | iOS Keychain access denial before first unlock | Hold splash; retry on next foreground |
| A14 | Disclaimer re-prompt retains user data? | Yes — gate not wipe |
| A15 | Privacy policy versioning | `policy_v` bundled, non-blocking banner on bump |

## Realistic effort estimate

v2.0 baseline: 48–67 dev-days
v2.1 additions: +33–44 dev-days
**Total v2.1 full scope: 81–111 dev-days**

To recover 60 days, defer:

| Cut | Days saved | To |
|---|---|---|
| F10 baby names | 3–5 | v1.1 |
| F07 tagged PDF + Atkinson font | 2 | v1.0.x |
| Backup/restore (SED-BAK) | 3–5 | v1.0.x |
| PDF data export | 1–2 | v1.0.x |
| Crash diagnostics share | 1–2 | v1.0.x |
| Multi-language refactor | 1–2 | v1.0.x |
| Custom UI primitives full set | 2–3 | v1.0.x (keep Gluestack with overrides) |
| **Total saved** | **~15–23 days** | |

Net v1.0 with cuts: 58–88 days.

## Critical-path dependencies (must start day -14)

1. **Trademark clearance** — "Seed" is a high-collision word in Class 9. Day-30 rebrand cost is catastrophic.
2. **Legal entity setup** — Companies House, Apple Developer + Play Console enrolment, D-U-N-S number.
3. **Illustrator commission** — 4–8 week turnaround for 42 illustrations.
4. **ICO registration** — fast (within 7 days) but should be done before submission.

## Content authoring workflow

Jon writes Markdown → `scripts/md-to-json.js` converts to JSON. CLINICAL_REVIEW_LOG.md tracks Markdown commit SHA. Conversion script ~0.5–1 dev-day. Mandatory for v1.0.

## Illustration asset pipeline

- Master format from illustrator: SVG (vector, scalable, smallest bundle)
- Rendering: `react-native-svg` `SvgUri`
- Bundle impact: 42 SVGs × 5-15 KB = 200-600 KB
- Contract terms: copyright assignment (not licence), milestone payments. Budget £25-50 × 42 = £1,050-2,100.

## Brand-rename SOP

If trademark fails on day 30:
- All `Seed` strings → `{appName}` token in `data/copy.en-GB.json`
- Reserve TWO bundle IDs at Apple/Google before day 1
- Git directory names stay `seed/`; only publisher-visible name changes

## Missing tooling specs

- Vale + Hemingway grade-6 ruleset
- mitmproxy for "zero network calls" QA verification
- `react-native-startup-time` for cold-start measurement (on-device, no telemetry)
- Xcode Accessibility Inspector / Android Accessibility Scanner
- bundlesize tracking

## Empty/loading/error state copy

Every list view needs spec for: empty (no entries), loading (rare given sync MMKV), error (network error doesn't apply — what does?). Add to v2.2.

## Bus-factor mitigation

- `/docs/adr/` with ADRs for each major decision
- `CONTRIBUTING.md` with environment setup
- `docs/cookbook.md` for repeating tasks
- `docs/architecture.png` (mermaid) one-page diagram

## Dependency version pinning

All deps in `package.json` use exact versions or `~patch` ranges only. No `^` carets. Pin Node 20.x LTS via `.nvmrc`.

## v1.0 ship list summary

v1.0 ships F01-F09 (no F10), F07 untagged A4 PDF with system fonts, JSON data export only (no PDF), Gluestack with selective overrides (no full custom primitives), no backup/restore (export only), no crash-diagnostics share, hard-coded en-GB strings (no i18next).

v1.0.x patches: F07 tagged PDF + Atkinson, PDF export, custom primitives, backup/restore, crash share, multi-language refactor.

v1.1: F10 baby names, F11 postnatal, Welsh, system dark mode.

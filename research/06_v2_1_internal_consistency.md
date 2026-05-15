# Seed PRD v2.1 — Internal Consistency Audit

**Source:** v2.1 / 2,695 lines / 287 unique SED-* IDs (no duplicates).

## Key findings

### Pricing arithmetic — the £4.16 figure is wrong

§2.2 SED-REV-001 claimed "After 15% commission AND VAT, net per sale is approximately £4.16." Arithmetic: £4.99 ÷ 1.20 = £4.158 (VAT removed only). After 15% commission on £4.158 = **£3.535**. The £4.16 figure is VAT-net before commission. §2.3 scenario table inflated by ~15%. Target scenario (15k × 5%) returns £2,651 net — **below the §14 £3,000 success metric**.

### SettingsState interface missing 6 fields the v2.1 prose mandates

| Required by | Field | v2.1 status |
|---|---|---|
| SED-CC-001 (4 modes) | `userMode: 'pregnant' | 'partner_companion' | 'intended_parent' | 'shared_device'` | Only 2 values defined |
| SED-CC-001 secondary toggle | `showPartnerContent: boolean` | Missing |
| SED-CC-018 / 019 | `trackingStatus: ... | 'hidden'` | `'hidden'` missing |
| SED-SAF-007a / SED-EDGE-010 | `disclaimerV: number` | Missing |
| SED-F05-008 | `weightChartHidden: boolean` | Missing |
| SED-A11Y-010 | `hapticsDisabled: boolean` | Missing |
| SED-PRIV-009 | `ageAnswer: 'yes' | 'no' | null` | Only `ageConfirmed: boolean` |

### Stale field references

- `isMultiple` referenced in SED-F02-007 — v2.1 replaced with `pregnancyType` enum
- `trackingPaused` / `trackingEnded` referenced in SED-CC-011/012 — v2.1 replaced with `trackingStatus` enum
- SED-CC-014 says "paused or ended" — should include `'hidden'` from SED-CC-019

### SED-ARCH-004 still mandates dropped `zustand-mmkv-storage` package

v2.1 §6.1 dropped the package but SED-ARCH-004 was not updated.

### §12.1 install commands include dropped packages

Phase-1 install commands at line 1810 still include `gluestack-ui` (v2.1 dropped) and `npm install zustand-mmkv-storage` (v2.1 dropped).

### Appendix legacy artefacts

- A.1 week-4 example: legacy `nhs.uk/pregnancy/week-by-week/` URL (v2.1 mandated new URL)
- A.4 names example: `"gender": "girl"/"boy"` (v2.1 renamed to `nameStyle: 'feminine'/'masculine'`)
- B reference: Miscarriage Association 01924 200 799 (v2.1 corrected to 0303 003 6464)

### Risk Register inconsistency

R13/R14/R15 marked Critical/High/Medium likelihood but already fixed in v2.1.

### Documentation hygiene

- TOC missing §18 Roadmap
- §13 Go-to-market has no parent heading (only §13.1/13.2/...)
- §11.2 and §11.3 appear in reverse order
- Stale "§9.4" cross-reference (relevant content moved to §12b.1)

### Proxy + Zustand persist race (CRITICAL)

The §6.5 code uses a Proxy with target `{}` to defer access to `mmkvInstance` until `initMMKV()` resolves. **Zustand v5's `persist` middleware hydrates synchronously at `create()` time when storage is synchronous** — `getItem` is called inside `create(persist(...))` at module-import time, before `await initMMKV()` runs. The Proxy throws "MMKV accessed before initMMKV() resolved" → app crashes on first launch.

## Recommendations (P0)

1. Update `SettingsState` interface — add 6 missing fields
2. Rewrite SED-CC-011/012/014 to reference `trackingStatus` enum (with 'hidden')
3. Rewrite SED-F02-007 to reference `pregnancyType`
4. Update SED-PRIV-004 to defer confirmation UX to SED-CC-021
5. Fix pricing arithmetic everywhere
6. Update Appendix A.4 (`gender` → `nameStyle`)
7. Update Appendix A.1 week-4 nhs_link
8. Update Appendix B Miscarriage Association number
9. Remove `gluestack-ui` from Phase-1 install
10. Remove `npm install zustand-mmkv-storage` from §12.1
11. Rewrite SED-ARCH-004
12. Replace Proxy with synchronous bootstrap
13. Fix TOC, add §13 parent heading, swap §11.2/§11.3
14. Mark R13–R15 as Closed

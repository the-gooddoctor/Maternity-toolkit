# F06 / F08 / F09 practical admin trio (v2.3 pass 6 of 12)

## v2.2 gaps

- F06 no discovery surface; no Labour-Mode integration; partner content not personalised
- F08 no deadlines; no F08↔F09 cross-linking; UK maternity rights one bullet line; no subsequent-pregnancy adaptation; no partner to-do view
- F09 one reminder cadence; no "questions to ask" surface; no outcome capture; no calendar export; no recurring series

## 30 feature additions integrated into v2.3

**F06 (SED-F06-009 to 017):** Dashboard week-34 prompt; Labour-Mode grab-now 7-item subset; birth-partner micro-list with name; item-type chip; twin bag preset; NICU "go bag" sub-section; week-38 panic-check panel (12 essentials); NHS public-info per-item links; section-packed silent caption.

**F08 (SED-F08-008 to 016):** Deadline-anchored items + urgency sort; F08↔F09 auto-tick linking; **UK maternity rights subsection (10 gov.uk / Acas / NHSBSA / HSE / Healthy Start items)**; subsequent-pregnancy mode; custom-deadline-creates-F09-reminder; FW8 / Mat B1 prominence in T1; partner to-do list; twin items expansion (12 items).

**F09 (SED-F09-008 to 019):** Free-text appointment with custom "What to expect"; questions-to-ask per appointment (pre-populated from F02); outcomes capture; add-to-phone-calendar (`expo-calendar`); configurable multi-reminder cadence; travel-time offset; appointment letter paste; recurring series; missed-appointment recovery; DC/DA + MC/DC twin schedules; "adjust to match your consultant" banner.

## UK maternity rights subsection (full spec)

10 row items in F08 pinned at top of "Work and practical" for T1/T2: SMP eligibility; Maternity Allowance; Tell-employer-by-25-weeks; Mat B1; Time off for antenatal appointments; Workplace risk assessment; Shared Parental Leave; FW8 free NHS prescriptions / dental; Healthy Start; Child Benefit registration after birth.

All copy uses "advises" / "explains"; legal deadlines use "by"; no calculators, no eligibility flow charts. Pure gov.uk + Acas + NHSBSA + HSE signposting. CSO sign-off required on row copy.

## Cross-feature integration

Seven cross-links spec'd: F09-completion auto-ticks F08; F02 questions seed F09 questions list; F08 custom-deadline creates F09 notification; F06 panic-check pulls F08 T3 health items; birth-setting drives F06/F08/F09 jointly; F03 Labour Mode surfaces F06 grab-now; **SED-CC-023** ensures all F06/F08/F09 cross-links honour `trackingStatus` per SED-CC-018.

Sources: gov.uk maternity pay/leave; Acas time off for pregnancy appointments; NHSBSA FW8 maternity exemption; HSE pregnant employee rights; NICE NG201 schedule of antenatal appointments; NICE NG137 twin pathway; Twins Trust membership.

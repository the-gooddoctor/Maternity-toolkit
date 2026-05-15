# F01 + F02 dashboard / week tracker (v2.3 pass 2 of 12)

## v2.2 audit gaps

- Card monolithic and identical for every user
- No capture surface (read-only — competitors retain users by writing-to-the-app)
- "Looking ahead" absent
- "Looking back" absent
- Term window stops at 42 weeks (no copy for 42+)
- Fruit-only size representation (gender-coded complaint)
- High-risk users invisible
- First-time vs subsequent indistinguishable

## Highest-leverage additions integrated into v2.3

1. **SED-F02-008 profile-aware rendering** — single highest-leverage change. ~2 dev-days, unlocks every conditional-content win for subsequent / partner / surrogacy / loss / high-risk users.
2. **SED-F02-010 per-week diary** — turns Home from "static info" into "your pregnancy journal" without crossing bright line.
3. **SED-F02-011 mood note** (CSO sign-off required — single sentence, NOT a tracker)
4. **SED-F01-012 extended term window** (37–42+ wide context)
5. **SED-F01-014 extra-care self-declaration** (cleanest available solve for high-risk population — signposting only)
6. **SED-F02-013 alternative size representations** — settles gender-coded complaint without dropping iconic fruit comparison
7. **SED-F02-014 next-week preview**, **SED-F02-016 daily prompt rotation** — variety without engagement-farming
8. **SED-F02-018 labour-readiness home re-ordering from week 36+**
9. **SED-F02-020 Memories PDF** at week 37+
10. **SED-F02-017 viability acknowledgement at 24+0** — high emotional moment, especially for subsequent-after-loss

Total v1.0 envelope add ~13–16 dev-days; sits inside the §3.0a v1.0 cut list cushion.

Conditional content matrix: rendered via `data/card-conditional-rules.json` indexed by `(profile, journey, settings)`.

## Reorganised Home tab hierarchy

Five zones: greeting + progression / today / this-week card / term context / quick-access tiles. EDD-declined dashboard collapses to greeting + add-EDD CTA + early-pregnancy info card + tile grid.

Full proposal text preserved in agent transcript.

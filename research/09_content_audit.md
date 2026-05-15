# Seed PRD v2.1 — Content Audit Against SED-CONTENT-001 and SED-SAF-005

## Prohibited-term occurrences (must fix)

| Term | String | Loc | Fix |
|---|---|---|---|
| "recommends" (clinical) | "NICE recommends clamping the cord…" | §3.7 SED-F07-002 | → "The NHS advises waiting at least one minute…" |
| "recommends" (clinical) | "The NHS recommends skin-to-skin…" | §3.7 | → "The NHS advises skin-to-skin contact…" |
| "recommends" (clinical) | "NICE recommends discussing…" | §3.7 | → "NICE advises that you discuss…" |
| "recommends" (clinical) | "NICE recommends it as an option" (birth-plan water) | App A.3 | → "NICE lists it as an option" |
| "normal" | "With twins... babies may be slightly smaller... This is normal." | §3.2 week-28 JSON | → "This is common and expected with twin pregnancies." |
| "normal" | "It's normal for the date to change after a dating scan." | §9.1 onboarding | → "It's common..." |
| "normal" | "...is completely normal." (emotions) | §4.4 SED-CC-016 | → "Many people feel this way." |
| "harmless" | "Braxton Hicks... are usually harmless" | §3.2 partner_content | → "...are usually not a cause for concern" |
| "painless" | "irregular, painless tightenings" | §3.2 maternal_changes | → "irregular, usually mild tightenings" |
| "you should" | "You should have an antenatal appointment around 28 weeks" | §3.2 key_appointments | → "An antenatal appointment is usually offered around 28 weeks" |

## Reading-age failures (must split or rewrite)

| String | Length | Grade | Action |
|---|---|---|---|
| §3.3 SED-F03-014 banner | 49 words (one 27-word sentence) | ~10 | Split to 5 sentences, 8.5 avg, grade ~5 |
| §3.4 SED-F04-011 banner | 18-word call sentence | ~7 | Split call into 2 sentences |
| §3.9 booking brief | 24 words single sentence comma-stacked | ~10 | Split to 6 sentences |
| §3.9 dating-scan brief | 26 words single sentence | ~12 | Split + add gloss for "nuchal translucency" |
| §3.9 36-week brief | "ECV" unglossed | ~11 jargon | Add "(bottom-down)... when a doctor gently tries to turn your baby into a head-down position" |
| §3.9 28-week brief | "symphysis-fundal height" unglossed | ~8 | Add "your bump — sometimes called symphysis-fundal height" |
| §5.3 SED-SAF-007 disclaimer | 22-word warning sentence | ~9 | Split into 3 sentences |
| §4.3 SED-CC-010 lead | 24-word sentence with em-dashes | ~9 | Split |

## UK English slips

- §3.4 SED-F04-010: "Fetal Movement Log" → "Foetal Movement Log"

## Required SED-SAF-005 exception list

1. "screening" — when naming an NHS-offered screening test (booking & dating briefs)
2. "normal" — Tommy's-attributed quote at SED-F04-007 + negated/scare-quoted at SED-F04-011
3. "diagnose / treat / cure / prevent" — Google-mandated disclaimer at SED-STORE-001
4. "risks" (plural, of a procedure) — only for procedures the user is being offered (epidural, ECV, caesarean)

## Anthropomorphisation

No violations. Notification surface ban from SED-CC-015 is appropriate; weekly cards may continue to use fruit/vegetable size comparisons. Add explicit scope note to SED-CC-015.

## Gendered partner copy

No violations found. Audit clean.

## Clinical-jargon glosses required

- "External cephalic version (ECV)" — add "a procedure where a doctor gently tries to turn your baby"
- "Nuchal translucency measurement" — add "a measurement of the fluid at the back of your baby's neck"
- "Symphysis-fundal height" — already parenthesised; add "sometimes called" wrapper
- "Anti-D injection" — add "(a treatment given when your blood type is Rhesus negative)" first occurrence
- "CTG / intermittent auscultation" — in F07 monitoring section, add plain-English labels

## Summary

22 itemised content edits required. The consistent pattern: v2.1 evidence summaries (F07-002) and "What to expect" briefs (F09-007) were drafted before the v2.1 SED-SAF-005 expansion was retroactively applied. The "recommends" verb survives in 4 places; "normal" in 3; "you should" in 1; one banner sentence misses ≤15-word target by 12 words; multiple briefs miss by 8-12 words; medical jargon unglossed in 3 places.

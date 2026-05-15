# F10 baby names — v1.0 stand-in + full v1.1 spec (v2.3 pass 8 of 12)

## v1.0 stand-in (1 hour) — SED-F10-PREVIEW-001

Single static screen at More → Baby names: one paragraph framing v1.1 plans; three current ONS-fact bullets; outbound links to ONS Baby Names dataset and NRS Babies First Names. Preserves free-tier breadth claim. Cost ~1 dev-hour.

## v1.1 full spec (24 requirements)

**Discovery surface (SED-F10-009 to 013):** Curated browse lists (Top 100 girls/boys, trending, stable classics, Welsh / Scottish / Irish / Indian / Pakistani-Bangladeshi / Arabic / Polish / Hebrew / Caribbean / African, floral, mythology, vintage, biblical, place names); theme tags; meaning search; 30-second style onboarding quiz; pronunciation field (with script for cultural names — opt-in display).

**Couple shortlist (SED-F10-014 to 016):** Veto power; "discussed but disagreed" tier; private layer ("only you can see this"); top 3 ranking with mutual top-3 view.

**Decision tools (SED-F10-017 to 023):** Save with notes; full-name preview with monogram; initials check (blocklist of ~80 embarrassing acronyms); surname phonetic check; sibling-rhyme detector; 5-question reflection ritual; honour/namesake field.

**Twin / multiple (SED-F10-025):** "Naming twins" curated set; per-baby slots A/B/C; warn when too-similar pair.

**Other (SED-F10-026 to 032):** Separate middle-name shortlist; "trending up" computed from `ons_trend`; tappable sparkline detail; soundex similar-sounding; veto / never-use list; "We chose this name on…" memory; mask mode for demonstrating app to family without revealing choices.

## Cultural collections principle

**Curated by named contributors, credited by name.** Each cultural collection ships with a named curator (lecturer, community member, paid, credited in Settings → Acknowledgements). No flag iconography. No "exotic" colour treatments. Same UI template as the ONS top-100 lists. Multi-valued origin field (a name is both Hebrew AND Arabic AND English where true). Welsh = NRS data; Scottish = NRS; Indian/Pakistani/Bangladeshi/Polish/Caribbean/West African/Arabic/Jewish = manual curation with named curator.

## Build estimate

v1.1 F10 engineering: ~16–18 days. Content curation (~10–15 person-days) runs in parallel.

Sources: ONS Baby Names dataset 2024; NRS Babies First Names 2024; Welsh / Cymraeg data via ONS regional breakdown.

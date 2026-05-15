# F07 birth plan builder (v2.3 pass 7 of 12)

## Critical content fix

Appendix A.3 `data/birth-plan-options.json` example still contained *"NICE recommends it as an option for pain management."* — a hard SED-SAF-005 violation that v2.2 audit missed. **v2.3 patches to: *"The NHS lists it as an option for pain relief (NICE CG190 / NG229)."*** This is a v1.0 content blocker; `scripts/audit-terms.js` CI would and should fail without the patch.

## 13 new requirements integrated into v2.3

- **SED-F07-016 auto-save with version history** — single highest-leverage change. Late-pregnancy users edit at 23:00–04:00; destructive edit at 38 weeks with no history is a regret event. Rolling 20 versions + 3 pinnable named versions.
- **SED-F07-017 "If things change" 5 sub-scenarios with pre-written first-person defaults** — assisted vaginal birth; unplanned caesarean; general anaesthetic; neonatal care; postpartum haemorrhage. All preferences ("I would like…") not clinical instructions.
- **SED-F07-018 structured cultural/religious section** — modesty, prayer, dietary, cultural practices, interpreter (with NHS-supported language picker).
- **SED-F07-019 structured disability/access needs section** — mobility, communication (BSL/easy-read/neurodivergent), sensory, condition free-text.
- **SED-F07-020 trauma-informed section** — heading deliberately avoids "trauma"; consent prompts ("Please ask me before any vaginal examination"); BTA card; crisis footer.
- **SED-F07-021 VBAC pathway** — continuous-monitoring acknowledgement (RCOG GTG-45); "If I labour before my planned date" sub-scenario.
- **SED-F07-022 twin birth-plan branch** — mode of delivery; epidural prominence; twin B preferences; skin-to-skin sequence; NICU access as first-class section.
- **SED-F07-023 must-haves max-5 tagging** — forced prioritisation; renders on page 1 of PDF.
- **SED-F07-024 Labour-Mode quick reference** — must-haves only, glance-readable.
- **SED-F07-025 birth-partner companion PDF** — separate ≤2-page output with action prompts.
- **SED-F07-026 multi-page PDF architecture** — must-haves first; preferences middle; if-things-change last; metadata footer.
- **SED-F07-027 uninterrupted skin-to-skin** preference checkable.
- **SED-F07-028 print + email from app** — `expo-print.printAsync()` AirPrint / Android Print Service.

## "If things change" full default-preferences text

5 scenarios × 4–6 pre-written first-person preferences each. CSO sign-off REQUIRED on the wording before content freeze (highest-risk content surface — directly informs clinical-decision moments during birth).

Full text preserved in agent transcript.

Sources: NHS make-a-birth-plan; NICE NG192 Caesarean birth (June 2025 update); NICE NG229 Fetal monitoring; NICE NG201 person-centred care; RCOG GTG-45 VBAC; NCT writing-a-birth-plan; Twins Trust birth preferences; Birth Trauma Association 2024 informed-consent campaign; NHS England 10-year-plan trauma-informed maternity priorities; NHSBT cord blood / amniotic membrane donation; AIMS gentle caesarean.

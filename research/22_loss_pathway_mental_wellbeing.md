# Loss pathway + mental wellbeing (v2.3 pass 11 of 12)

## Most differentiating territory

72% of pregnancy apps don't account for loss at all; 18% offer a button with no support; ~10% link out (The Conversation 2021). Seed v2.2 was already in the top 10% by that bar. v2.3 pushes further — into territory no UK pregnancy app currently occupies.

## v2.3 integrated changes

**SED-CC-031 pregnancy-after-loss lite mode** — optional onboarding declaration; triggers `birthSensitivity = 'trauma_aware'`; suppresses fruit-vegetable size copy; reframes milestone copy as factual not celebratory; F04 sensitive-mode preamble; "Pregnancy after loss" content card pins to top of §4.4; partner mode adds Sands partner content.

**SED-CC-032 sensitive-mode toggle** — independent of previous-loss declaration; available to all users; default on if SED-CC-031 = Yes.

**SED-CC-033 soft pause "while I find out"** — fifth option above Hide on compassionate screen for the Schrödinger's-pregnancy state (bleeding, abnormal screening result, missed scan).

**SED-CC-034 gestation-aware charity highlighting** — top 2–3 charities pinned based on stored gestational age (under 12w → MA + Tommy's; 12–24w → MA + Tommy's + ARC; 24w+ → Sands + Tommy's + Petals; twins → Twins Trust Bereavement always).

**SED-CC-035 lifecycle state model** — `lifecycleStage: 'pregnancy' | 'postnatal' | 'memorial'` orthogonal to `trackingStatus`. Memorial is a sibling of postnatal mode, not a terminal state.

**SED-CC-036 "Save a record" memorial page** — quiet typographic composition; user-typed title placeholder "Their name, or a word to mark this" (never auto-filled); date, gestation, free-text, optional photo (locally stored). Standard line: *"This page is yours. Only you can see it. It stays on your device."* PDF generation available.

**SED-CC-037 anniversary acknowledgement** (v1.0.x) — opt-in only; one local notification on user-chosen anniversary date: *"Today marks [N] year[s]. You don't have to do anything. We're thinking of you. Here are charities if you need support."*

**SED-CC-038 lost-one-of-twins branch** — when user in twin pregnancy enters loss flow, asks "Are you continuing your pregnancy with the remaining baby?" If yes: Twins Trust Bereavement + Petals + Tommy's pinned; tracker continues in sensitive mode. If no: standard twin-loss signposting with Twins Trust Bereavement front-and-centre.

**SED-CC-039 TFMR-specific compassionate-screen line** — *"If you are facing a decision about your pregnancy, ARC offers non-judgmental support at every stage of that decision."* ARC pinned when entry comes from anomaly-scan or screening note.

## Mental wellbeing library (4 → 18 cards)

**SED-CC-017a** §4.4 expands to ~18 NHS / RCPsych / charity-sourced cards: antenatal anxiety; perinatal depression; **perinatal OCD and intrusive thoughts** (Maternal OCD, OCDUK, RCPsych); **postnatal psychosis** (Action on Postpartum Psychosis); **tokophobia** (NHS pathway); PTSD from previous birth (BTA); bonding worries; hyperemesis-related distress (Pregnancy Sickness Support); ED relapse risk (BEAT); bipolar in pregnancy; **Black maternal mental health** (The Motherhood Group, Muma Nurture); telling your midwife; partner mental health; mental health after a previous loss; sleep and mental health; when to call urgent help.

**SED-CC-017b** "Your midwife will not judge you" framing — MMHA-aligned wording: *"Speaking to your midwife about your mental health is confidential. They cannot remove your child for asking for help."*

**SED-CC-017c** NHS 111 option 2 in universal crisis footer — was only Samaritans + 999 in v2.2.

## Bright-line discipline

App does NOT analyse free text. Crisis content is signposting only. The app never says "we noticed you might be feeling…" (catastrophic class — extends SED-UX-027 AP-29).

Sources: Tommy's pregnancy-after-loss; Sands "Mainly for Fathers"; Petals charity; ARC TFMR survey; Twins Trust bereavement; MMHA Black maternal MH; Perinatal OCD RCPsych; Maternal OCD; NHS tokophobia pathway; BTA; Pregnancy Sickness Support; Aching Arms; Sands National Perinatal Mortality Review Tool; The Motherhood Group; Muma Nurture; MAMA Academy rainbow pregnancies; Kicks Count rainbow pregnancy; trauma-informed-design scoping reviews.

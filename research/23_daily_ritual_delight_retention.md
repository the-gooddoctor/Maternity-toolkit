# Daily ritual, delight, retention without telemetry (v2.3 pass 12 of 12)

## Retention thesis

Seed retains 40+ weeks not because users have to come back, but because:
1. The dashboard tells you something true and useful every time you open it
2. The free tier is genuinely polished and complete for first-trimester users
3. Pro becomes felt-need at week 28–32 (hospital bag, birth plan, contraction timer) — not pushed, just available
4. Memories accrue passively — the longer you stay, the more "Your story" is worth
5. The app is recommended by friends because of loss-handling, partner mode, privacy, and £4.99 one-time
6. After birth, Seed hands off cleanly and stays available for pregnancy 2

This is retention as a by-product of being useful and respectful, not as a metric to optimise — the only retention strategy compatible with SED-ARCH-003 (no telemetry), §4.4 (no manipulative wellbeing), §18 (no social ever), and the Children's Code.

## Anti-pattern catalogue (SED-UX-027) — 30 explicit prohibitions

**The single highest-leverage v2.3 addition.** Codified as a CI-enforceable list (AP-01 through AP-30). Encompasses:

- Engagement (AP-01 streaks; AP-03 inactivity nags; AP-04 celebratory pushes; AP-15 endless scroll; AP-17 daily check-in to unlock)
- Social (AP-02 leaderboards; AP-05 social proof; AP-11 referral viral; AP-20 auto-post sharing)
- Telemetry-dependent (AP-13 email marketing; AP-14 behaviour-targeted Pro)
- Clinical interpretation (AP-06 threshold alerts; AP-23 crying tracker; AP-25 milestone comparison; AP-27 weight centile; AP-29 free-text mood inference; AP-30 A/B on clinical content without CSO sign-off)
- Pro coercion (AP-07 modal interrupts; AP-08 urgency; AP-10 profile-nag; AP-19 trial-flow auto-convert)
- Body-monitoring (AP-16 weight reminders)
- Loss / sensitivity (AP-18 forced confetti)
- Ad-adjacent (AP-28 affiliate / sponsored)
- AI (AP-09 mood scoring; AP-24 photo analysis)
- F04-specific (AP-21 cold-drink prompt; AP-22 movement-data nudge)
- F11-specific (AP-26 sleep-schedule recommender)

Encodes engagement-farming prohibition that until v2.3 was implicit in §18 non-roadmap. Makes it CI-enforceable from day 1.

## Pro conversion discipline — SED-UX-028

Exactly 5 permitted touchpoints; all others banned per SED-UX-027:
1. First-touch on Pro-locked tile (calm screen per SED-UX-006)
2. Week 28 collapsible dashboard banner
3. Week 32 banner refresh
4. F09 appointment-brief locked preview
5. Restore CTA at equal prominence

Forbidden: limited-time urgency; modal interrupts; recurring nag; behaviour-targeted; notifications mentioning Pro; trial-flow.

Copy A/B variants (manual rollout, RevenueCat dashboard signal): utility / privacy / timing / founder framing.

## 60-second time-to-first-value — SED-UX-026

Inserts a value-confirmation beat between onboarding Screen 4 and Screen 5: *"You're [N] weeks and [N] days. Your baby is about the size of a [fruit]. Welcome."* Shifts first-delight from "dashboard appears" to a personal acknowledgement.

## v2.3 additions integrated

- SED-UX-020 daily fact rotation
- SED-UX-021 time-of-day dashboard copy (v1.0.x)
- SED-UX-022 trimester visual treatment shift (v1.0.x)
- SED-UX-023 "We won't bug you" Settings card
- SED-UX-024 haptic milestone moments (v1.1, opt-in)
- SED-UX-025 Pro-upsell forbidden-copy list (folded into SED-UX-027)
- SED-UX-026 60-second value-confirmation beat
- SED-UX-027 anti-pattern catalogue
- SED-UX-028 Pro upsell discipline
- SED-F02-020 Memories PDF
- SED-CC-024 show-partner share card (v1.0.x)
- SED-CC-025 subsequent-pregnancy welcome flow (v1.0.x)
- SED-CC-026 post-birth sign-off screen (v1.0.x)

## End-of-pregnancy handoff

When user marks "My baby has been born" / EDD+14 quiet prompt confirmed / "End tracking" tapped — single calm screen offers Memories PDF, data export reminder, sign-off message tailored to live-birth or loss path. No celebration trigger if loss path.

## Subsequent pregnancy welcome flow

On app open, if `EDD > today + 90 days` OR pregnancy marked ended — show dismissible card: *"Welcome back. Your previous pregnancy is saved in Memories. Ready to start a new one?"* Architecture: `pregnancyArchive[]` array of past completed pregnancies, immutable once archived.

Sources: SED-ARCH-003 architectural invariant; Children's Code (ICO Age Appropriate Design Code); SED-CC-017 mood-tracker ban; §18 non-roadmap; competitor App Store/Play Store review patterns (Flo, Pregnancy+, BabyCentre); Mumsnet retention complaints; Mozilla Privacy Not Included on pregnancy apps; SQ Magazine 2025 mobile-app statistics.

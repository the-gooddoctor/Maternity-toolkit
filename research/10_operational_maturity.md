# Seed PRD v2.1 — Launch-Readiness / Operational Maturity Review

**Verdict:** v2.1 is launch-grade on architecture, regulatory posture, feature scope, support, QA, release. Operational gaps concentrated in four clusters: incident response/observability, legal supplementaries, business continuity, and runbook tier.

## Highest-leverage missing items

### SED-CLIN-006 — Deputy CSO
Bus-factor-1 on clinical sign-off is the only "could end the company in a week" residual risk. Name a deputy CSO (NHS-employed obstetrician/midwife consultant/GP with maternity experience) on retainer (£200-£400/month or £150/hour per incident). Document GMC/NMC PIN in `/docs/clinical/deputy-cso.md`.

### SED-INC-002 — Vulnerability disclosure
Publish `SECURITY.md` + `security.txt` per RFC 9116. Without it, no defensible answer when a researcher emails about a vulnerability.

### SED-LEG-009 — OSS licence acknowledgements
Both stores require disclosure of third-party OSS licences. `npm-licenses` build step writes `assets/licences.json` rendered at Settings → Legal → "Open-source licences".

### SED-LEG-010 — Insurance
Pre-launch: PI (£400-£900/yr), cyber liability (£300-£600), public liability, product liability. Total ~£800-£2,500/yr.

### SED-PRIV-013 — DSAR SOP
Even though no health data is held, a user may file a DSAR. Template at `/docs/support-templates/dsar-response.md`. Owner [Jon].

### SED-CC-023 — EDD+14 quiet-mode prompt
When EDD passed by 14 days and user still active, prompt: "Has your baby arrived? Quiet the app while you settle in." Three buttons: not-yet / yes-quiet / rather-not-say.

### SED-ARCH-017/018 — RevenueCat Paywalls / Experiments disabled
v2.1 doesn't explicitly disable these RevenueCat features. SED-ARCH-003/013 implicitly prohibit but make explicit.

## Documents to create at `/docs/`

1. `/docs/runbooks/RUNBOOKS_INDEX.md`
2. `/docs/runbooks/launch-day.md` — T-48h/T-24h/T-0/T+1h/T+24h checkpoints
3. `/docs/runbooks/store-review-response.md`
4. `/docs/runbooks/clinical-content-update.md`
5. `/docs/runbooks/incident.md`
6. `/docs/runbooks/refund.md`
7. `/docs/runbooks/support-triage.md`
8. `/docs/runbooks/beta-recruitment.md`
9. `/docs/runbooks/annual-content-reverification.md`
10. `/docs/legal/insurance.md`
11. `/docs/legal/dpa-revenuecat.md`
12. `/docs/finance/` directory
13. `/docs/support-templates/` — 6 templates
14. `SECURITY.md` at repo root
15. `/.well-known/security.txt` (deployed to seed.health)

## Service health and observability (no telemetry)

- SED-OBS-001: UptimeRobot 5-min checks on `seed.health` properties
- SED-OBS-002: Apple App Store reviews RSS feed, Play Console email alerts
- SED-OBS-003: support@seed.health shared inbox auto-ack + 72h escalation
- SED-OBS-004: RevenueCat dashboard daily check
- SED-OBS-005: Status page at `seed.health/status` manually maintained

## User communication channels

- SED-COMM-001: In-app "What's new" screen on update
- SED-COMM-002: App Store promotional text guidance
- SED-COMM-003: `seed.health/changelog` with Atom feed
- SED-COMM-004: Security advisory channel
- SED-COMM-005: Privacy-policy versioning with `policy_v`

## Pre-submission additions (§12.10)

- SECURITY.md and security.txt published
- Accessibility statement live
- OSS licences screen present
- Insurance in force
- DSAR template committed
- Deputy CSO retained
- UptimeRobot live on all public URLs
- Apple promo codes for App Review
- Status page baseline ("All systems normal")

## Risk register additions

R21 CSO bus-factor; R22 RevenueCat breach; R23 vuln disclosure; R24 store suspension; R25 web property down; R26 support inbox missed; R27 OSS non-compliance; R28 DSAR missed; R29 Children's Code; R30 DPF revocation; R31 insurance gap; R32 end-of-life.

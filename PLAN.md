# PLAN — home-energy-savings (product-neutral home energy-efficiency guides + calculators)

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated

## Executive summary

**home-energy-savings** is an open, **product-neutral** web app (installable PWA) that helps
households spend less on energy and live in more comfortable homes. It pairs two things: (1) a body
of **plain-language efficiency guides** — air sealing, insulation, heating and cooling, water
heating, appliances and lighting, behavior, and help for renters and energy-burdened households —
and (2) a set of **transparent savings calculators** that estimate the energy, money, and emissions
impact of common improvements. Every guide is sourced to authoritative public-domain guidance; every
calculator **shows its work**: documented inputs, assumptions, formula, sources, and an honest
uncertainty range — never a single false-precision dollar figure.

The defining constraint *is* the product. The commercial energy-efficiency web is saturated with
brand recommendations, affiliate links, lead-generation forms, and "get a free quote" funnels.
home-energy-savings refuses all of it: **no product upsell, no brand rankings, no affiliate links,
no lead-gen, no account, no telemetry, no PII.** It is the calm, neutral, trustworthy reference a
household (or a library, a weatherization program, a community-action agency) can hand to anyone.

This is a **low** risk-tier project overall, but we do not treat "low risk" as "no care." Two edges
are handled explicitly and conservatively:

- **Money figures border on financial advice.** Savings/payback numbers are framed as **educational
  estimates, not financial advice**, always shown with assumptions and a range, and never used to
  steer a household toward a specific purchase. Each calculator is validated in CI against
  **golden-reference cases** so the model is correct, not just plausible.
- **Some efficiency work touches real safety.** Combustion appliances, gas, electrical work,
  ventilation/backdrafting and carbon monoxide, and moisture/mold or lead/asbestos in older homes
  can hurt people if mis-handled. Content on these topics is **link-out, not DIY instruction**, and
  is carried at an **elevated (medium) review bar** with a qualified building-science reviewer.

**Honesty note on the partner.** No partner organization, MOU, or "verified need" exists yet. Energy
burden is well documented by public agencies, so foundation work proceeds — but the partner and
verified-need status are **TO BE SECURED**, and until a named org confirms the need in writing the
project ships only as a generic public good. See *Problem & beneficiaries* and *Open questions*.

## Problem & beneficiaries

**The problem.** Home energy is one of the largest controllable household costs, and the lowest-income
households carry the heaviest **energy burden** (a disproportionate share of income spent on energy),
yet trustworthy help is hard to find. Authoritative guidance exists but is fragmented across many
agency sites, often PDF-only, English-only, and assumes a homeowner with capital. Meanwhile the
commercial web answers "how do I lower my energy bill?" with brand upsells, affiliate roundups, and
quote forms that harvest personal data. The result: households over-pay, distrust the advice they
find, and cannot tell a neutral estimate from a sales pitch.

**Who is helped (beneficiaries).**
- **Energy-burdened and low-income households** — renters and owners who most need accurate,
  zero-cost, no-strings guidance and an honest estimate of what an action is actually worth.
- **Renters**, who are usually excluded from "upgrade your home" content but can still act
  (behavior, low-/no-cost measures, landlord conversations, knowing their rights and programs).
- **General households** wanting a neutral second opinion before spending money.
- **Community organizations** — weatherization assistance programs (WAP), community-action agencies,
  libraries, energy nonprofits, and local governments — who need a free, brandable, multilingual,
  privacy-respecting tool to hand to their communities instead of building one.

**Verified need / partner org: TO BE SECURED.** No organization is named and no agreement is on file.
We treat the need as **plausible and well-evidenced but unverified** until at least one named
org (e.g. a weatherization/community-action/energy-equity organization or a public library system)
confirms it in writing and identifies priority audiences, regions, and languages. Foundation work
(M0–M2) proceeds on the public evidence; partner endorsement is required before the project-level
*Definition of Shipped* is met (see *Quality, review & risk gates*).

## Goals and non-goals

**Goals.**
- Ship a free, installable PWA that gives households **product-neutral** efficiency guidance and
  **transparent savings calculators** that show every assumption and an honest range.
- Make every calculator **correct and auditable**: documented methodology, cited sources, and
  golden-reference validation in CI.
- Keep every guide **source-cited** and traceable to a named authoritative source, passing accuracy
  review; carry safety-sensitive topics at an elevated review bar.
- Be genuinely **free and private**: MIT code / CC-BY content, no ads, no telemetry, no paywall, no
  account, **no PII**; calculator inputs stay on the device.
- Meet **WCAG 2.2 AA** as a hard gate (energy burden correlates with disability, age, and low
  literacy — accessibility is an equity requirement, not a nicety).
- Support **internationalization and unit systems** (metric/imperial, locale formatting) with at
  least one non-English localization.
- Center **equity**: explicit content for renters and energy-burdened households, and a
  product-neutral pointer to assistance programs (LIHEAP/WAP/utility rebates).
- Be **adoptable** by a partner org in its community's languages.

**Non-goals (constraints as identity).**
- **No product upsell, ever.** No brand recommendations or rankings, no "best product" lists, no
  affiliate links, no sponsored placement, no lead-generation or quote forms.
- **Not financial advice.** Calculators educate and estimate; they never tell a household to take on
  debt, sign a contract, or buy a specific product, and never promise a guaranteed return.
- **Not DIY instruction for hazardous work.** No step-by-step for gas, combustion, electrical
  panel/wiring, asbestos/lead abatement — link to qualified professionals and official guidance.
- **Not a utility-bill account, energy monitor, or live data product.** No account, no smart-meter
  ingestion, no real-time pricing, no cloud sync.
- **Not a data-collection or analytics product.** No user profiles, no telemetry, no PII.
- **Not partisan advocacy.** It is about cost, comfort, and (neutrally) emissions — not climate
  politics or policy campaigning.
- **Not a commercial funnel** for any vendor, installer, utility, or program.

## Success metrics (outcomes)

Outcome-centric and beneficiary-first. Baselines are zero at project start unless noted. Because we
collect **no telemetry**, reach is measured by partner self-report, not in-app analytics — an honest
trade-off that prioritizes privacy over precision.

| Outcome | Baseline | Target (first 12 months post-launch) | How measured (privacy-preserving) |
| --- | --- | --- | --- |
| Partner orgs endorsing/adopting the toolkit | 0 | ≥ 1 named org adopts (goal 3), incl. ≥ 1 serving energy-burdened/low-income households | Signed letters of support / MOUs (manual record) |
| Guides shipped & accuracy-reviewed | 0 | ≥ 8 guides, each source-cited and signed off | In-repo review log |
| Calculators shipped, methodology-reviewed & golden-validated | 0 | ≥ 6 calculators, each with a methodology card + passing golden-reference CI | Methodology cards + CI golden tests |
| Calculator correctness | none | Each calculator matches its authoritative reference case(s) within the declared tolerance | CI golden-reference assertions (build fails on drift) |
| Energy-burdened/renter-specific resources | 0 | Dedicated renter + low-income tracks + product-neutral program-finder pointer shipped | In-repo content + review log |
| Estimated savings *identified* by users (modeled) | 0 | Reported by partners as "households that identified ≥ 1 actionable measure" (count of households, **not** a claimed dollar total) | Partner self-report template |
| Languages fully localized (UI + ≥1 guide + ≥1 calculator) | 1 (en) | ≥ 2 languages | CI translation + unit-system completeness check |
| Accessibility conformance | none | WCAG 2.2 AA verified; 0 critical axe violations; manual AT pass | axe/pa11y + manual screen-reader audit |
| Privacy posture | n/a | 0 telemetry endpoints, 0 third-party trackers, 0 PII fields; inputs stay on device | Static audit + manifest review + **runtime network-interception E2E** (CSP `connect-src 'none'`) |
| Households reached (partner-reported, opt-in) | 0 | ≥ 5,000 **distinct households** reached over the rolling 12-month window | Partner self-report template (no in-app tracking) |

We deliberately avoid vanity metrics (page views, downloads, stars) **and** avoid publishing a
headline "total dollars saved" figure — savings are modeled estimates, and claiming a precise
aggregate would overstate certainty and drift toward marketing. We report **households that
identified an actionable measure**, not a fabricated savings total.

**Reach measurement — windows, denominators, anti-double-counting.** "Households reached" counts
**distinct households** (not page views, installs, or leaflets) over a **rolling 12-month window**
from production launch. Each partner counts a household **once per period** across touchpoints;
households reached by multiple partners are de-duplicated by the steward at roll-up (best-effort,
partner-attested, regional overlap noted). Estimates are recorded **as estimates**, separately from
confirmed counts.

**Partner self-report template (standard fields).** Per period: partner name; reporting window;
channel(s); **distinct households reached (confirmed)**; estimated additional reach (flagged);
region(s)/language(s); de-dup notes; and a free-text outcomes note. The steward keeps these in the
manual outcomes record and publishes only de-duplicated, aggregated figures.

## Scope

**In scope.**
- Static-first PWA: installable (web app manifest), works offline after first load, no backend.
- **Guides:** air sealing, insulation, heating & cooling (incl. heat pumps, neutrally), water
  heating, appliances & lighting, behavior/no-cost measures, renters, energy-burdened/low-income
  help, and safety-sensitive topics handled as link-out.
- **Calculators (transparent):** each is a deterministic, documented model with inputs, assumptions
  register, formula, cited sources, an output **range** with uncertainty, a "show your work"
  methodology panel, and a "not financial advice" frame. Examples: lighting (LED) savings, thermostat
  setback, insulation/air-sealing payback, water-heating savings, appliance-replacement payback,
  "where does my home energy go?" breakdown, and an **educational** solar-suitability primer.
- **Program-finder pointer:** product-neutral links to official assistance program directories
  (LIHEAP, WAP, utility rebate finders) with **no PII collected** and no eligibility decisions made.
- Content + calculator pipeline: structured schema, provenance/citation, methodology cards, review
  log, CI validation.
- i18n + unit-system (metric/imperial) handling + at least one non-English locale.
- WCAG 2.2 AA accessibility; on-device-only input persistence.

**Out of scope (explicit).**
- Any product recommendation, brand ranking, "best of" list, affiliate link, sponsorship, or
  lead-gen/quote form.
- Financial advice, loan/financing recommendations, guaranteed-return claims, or eligibility
  *determinations* for any program.
- DIY instructions for gas/combustion, electrical panel/wiring, asbestos/lead abatement, or any
  work with a meaningful safety hazard (link out only).
- Accounts, cloud sync, smart-meter/utility-bill ingestion, real-time pricing, live data feeds.
- Telemetry, analytics, advertising, A/B testing on users, or any monetization.
- Native app-store binaries at launch (backlog).
- Climate/rate data we cannot license for reuse, or jurisdictions we cannot source-verify.

## Solution approach & architecture

**Overview.** A static-first PWA. All content, the calculator models, and the reference data
(climate-zone tables, default/benchmark values) are bundled or precached, so calculators run
**entirely client-side** and inputs never leave the device. The only network needs are the initial
app load and optional update checks. No backend is required for core use.

**Components.**
- **App shell (UI):** TypeScript/ESM, component-based (framework TBD — lightweight, a11y-friendly,
  e.g. Preact/Svelte/Lit; recorded as an ADR in M0). Client-side routing, offline-capable.
- **Calculator engine (the heart of the project):** a shared **model contract** every calculator
  implements — `inputs → model(inputs, assumptions) → result{ central, low, high, units, breakdown }`
  — with each model carrying a machine-readable **assumptions register** and a **methodology card**
  (plain-language explanation, formula, source citations, validity range, known limitations). Models
  are **pure deterministic functions** (no I/O), which is what makes them unit-testable and
  golden-validatable. The UI renders the result as a **range**, exposes "show your work," and shows a
  persistent "estimate, not financial advice" frame.
- **Reference-data store:** versioned, bundled tables — climate zones (e.g. IECC/DOE zones), default
  appliance/fuel benchmarks, and **clearly-labeled default energy prices** the user can override.
  Prices are defaults for estimation, **not** live rates; their vintage/source is shown and they are
  on a re-validation cadence (see *Sustainability*).
- **Content store:** versioned structured guides (JSON/MDX — format decided by ADR) with citations,
  review status, and a review-log reference; loaded into the offline cache.
- **Service worker / offline layer:** precache of shell + active-locale content and reference data;
  cache-first for content, stale-while-revalidate for the shell; offline fallback. A
  **content-version manifest** (per-unit version + integrity hash) lets the SW reconcile cached units
  and **hard-invalidate** any unit/calculator corrected after an error, forcing a re-fetch before it
  can be shown (non-dismissible for that unit). Tooling (Workbox vs. hand-rolled) is an M0 ADR.
- **i18n + units layer:** message catalogs per locale; content keyed by `(unitId, lang)`; build-time
  completeness validation; safe fallback to English; **measurement-system handling (metric/imperial)**
  and locale-aware number/currency/date formatting. i18n scope (RTL, bundled offline fonts with glyph
  coverage, ICU pluralization/select, locale formatting) is settled **before** the UI-framework ADR.
- **Program-finder pointer:** a static, product-neutral directory of links to **official** program
  finders; collects no inputs that leave the device and makes no eligibility decision.
- **Build/CI:** pnpm workspace; lint, typecheck, unit, a11y, offline E2E, no-telemetry/PII, and
  **calculator golden-reference** gates.

**Tech stack.** TypeScript, ESM, pnpm workspaces; PWA (manifest + service worker); static hosting
(GitHub Pages / Netlify / Cloudflare Pages — no app server). Testing: unit (Vitest), a11y
(axe-core/pa11y), E2E incl. offline (Playwright), golden-reference calculator tests (Vitest
snapshots/tables). License: MIT (code) / CC-BY-4.0 (content).

**Data model (sketch).**
```
Guide {
  id; title: localized; topic; audience: ["owner"|"renter"|"low-income"|...]
  appliesTo: regionTags[]            // advisory, not geolocation
  sections: { explainer, steps, whenToCallAPro, links[] }
  safetySensitive: boolean           // gas/combustion/electrical/CO/mold/lead → elevated review
  sources: Citation[]                // { org, title, url, retrievedDate, sourceLicense }
  reviewStatus; reviewedBy; approvedBy (distinct); reviewLogRef; lastReviewed
  lang; contentLicense: "CC-BY-4.0"; contentVersion; integrityHash; hardInvalidate
}

CalculatorModel {
  id; title: localized; topic
  inputs: InputSpec[]                // name, type, unit, range, default, required
  assumptions: AssumptionEntry[]     // { key, value, unit, source, rationale, lastReviewed }
  methodology: { plain: localized; formulaRef; validityRange; limitations[] }
  compute(inputs, assumptions) -> { central, low, high, units, breakdown[] }  // pure function
  goldenCases: GoldenCase[]          // { inputs, expected{central,low,high}, tolerance, sourceRef }
  notAdvice: true
  sources: Citation[]; reviewStatus; reviewedBy; approvedBy (distinct); reviewLogRef; lastReviewed
  contentVersion; integrityHash; hardInvalidate
}
```

**Key decisions (recorded as ADRs in M0).**
1. UI framework (a11y + bundle size + offline simplicity; i18n/RTL/units support is a hard input).
2. **Calculator-model architecture** — pure deterministic models, the shared result contract
   (central + low/high range), the assumptions register, and how golden cases are expressed/run.
3. Content format (JSON vs. MDX) and how citations/review status/methodology are enforced in CI.
4. Units & measurement-system handling (metric/imperial), default-price store + override + vintage.
5. Service-worker tooling, caching strategy, content-version manifest / hard-invalidation.
6. Hosting target + update/versioning strategy for cached clients.

**Decision ordering.** ADR #3 (content format) is decided **before** the content schema is finalized;
ADR #2 (calculator-model architecture) is decided **before** the first real calculator and the
golden-test harness. Until those land, the schemas here are **provisional** (intent, not assumed
shape). TASKS.md sequences these dependencies explicitly.

## Data, licensing & compliance

**This section is load-bearing; be conservative — especially because some of our headline sources
carry trademarks or restrictive licenses.**

**Content & data sources.** Guidance and reference data derive from **authoritative public sources**:
US Department of Energy / **Energy.gov / Energy Saver** (US federal, generally public domain), the
**US EIA** (Energy Information Administration — US federal, public domain; e.g. RECS for end-use
benchmarks and price data), **NREL** (mixed licenses — verify per dataset), and relevant national
energy agencies per target region/language. Each unit cites org, title, URL, retrieval date, and the
source's license/terms.

**Trademark and restrictive-license cautions (do not gloss over).**
- **ENERGY STAR** is an EPA-administered **certification mark**. We may *factually reference* ENERGY
  STAR criteria where public, but **must not** use the ENERGY STAR name/logo in a way that implies
  endorsement or certification of this project, and must not present it as a product endorsement. Our
  stance is **product-neutral**: we describe efficiency criteria generically and link to the official
  program rather than steering to certified *brands*.
- **IEA** data and many international agency datasets are **copyrighted and not freely reusable** —
  link to them, do not embed, unless a specific open license is confirmed.
- **Utility rate data** is often proprietary or licensed (and changes constantly). We ship
  **clearly-labeled default prices for estimation only** (preferring public EIA averages), let users
  override, show the vintage/source, and **do not** scrape or redistribute proprietary tariffs.
- **NREL / lab data:** license varies by dataset; verify each before reuse.

**Licensing rigor.**
- **Our outputs:** code under **MIT**; content + calculator methodology cards under **CC-BY-4.0**.
- **Source reuse:** verify each source's terms before reuse. US federal works (DOE/Energy.gov, EIA,
  EPA text) are generally public domain, but **paraphrase and cite — do not copy verbatim — and never
  reuse third-party logos/trademarks/certification marks.** Where a license is unclear or restrictive,
  link rather than embed. Provenance and license of every source recorded in the unit and a
  provenance log.
- **Calculator constants/assumptions:** every default value (fuel content, efficiency benchmarks,
  emissions factors, default prices) carries a cited source and `lastReviewed` date in the
  assumptions register. A constant without provenance fails CI.
- **Translations:** derivatives; same source-license rules; reviewed for accuracy, not just fluency.

**Provenance model.** Every guide and calculator carries `sources[]`, an assumptions register (for
calculators), and a PR-tied review-log entry (`reviewedBy`, `approvedBy`, `lastReviewed`,
`reviewStatus`, divergence notes). The repo-level provenance/review log is the auditable record the
*Definition of Shipped* checks against.

**Privacy / PII stance.** **Zero PII.** No accounts, analytics, telemetry, third-party scripts, or
cookies beyond what the SW needs. Calculator inputs (home size, bills, **ZIP/postcode used only for a
local climate-zone lookup — never precise geolocation**, income band only if the user opts to check
program relevance) **never leave the device**: computed locally, persisted locally
(IndexedDB/localStorage), exported only by explicit user action (print/local file). Enforced and
audited in CI (no network egress in core flows; manifest/script audit).

**Non-partisan / no-advocacy stance.** Energy is politically charged. Content stays on **cost,
comfort, health, and (neutrally stated, sourced) emissions**; it avoids advocacy, partisan framing,
and policy campaigning, and presents options (including heat pumps, solar, gas) neutrally with
trade-offs and sources rather than as causes.

**Attribution.** Source agencies are credited per unit and on a credits page. We do **not** imply
endorsement by any agency, program, utility, or vendor unless it has explicitly endorsed the toolkit.

## Quality, review & risk gates

**Risk tier: low (project-level), with an elevated medium sub-gate** for (a) any unit producing a
**financial/savings figure** and (b) **safety-sensitive** content (combustion/gas/electrical/CO,
moisture/mold, lead/asbestos in older homes). "Low" governs general guides; the medium sub-gate is
mandatory wherever money or safety is involved.

**Required reviews before a deed is "done":**
- **Code/PWA tasks:** maintainer code review + CI green (lint, typecheck, unit, **a11y**, **offline
  E2E**, **no-telemetry/PII**). Accessibility regressions block merge.
- **Calculator tasks (medium sub-gate):** the model passes its **golden-reference cases in CI**
  (output within declared tolerance of an authoritative worked example or established tool); the
  **methodology card** is complete (assumptions sourced, formula, validity range, limitations); a
  reviewer with **energy-modeling / building-science competence** signs off on the method; and the
  **"estimate, not financial advice"** frame plus visible assumptions/range are present. A calculator
  that emits a single number with no range or no sourced assumptions cannot ship.
- **Content tasks (low) / safety-sensitive content (medium sub-gate):** a domain reviewer verifies
  each claim against cited authoritative sources. For **safety-sensitive** units, the content is
  **link-out, not DIY instruction**, and is approved by a reviewer meeting a **credential bar** —
  current/former building-science professional (e.g. BPI- or RESNET/HERS-qualified), licensed
  contractor/inspector in the relevant trade, or equivalent recognized qualification.
- **Role separation & no self-approval.** The author of a unit may **not** approve it: `reviewedBy`
  (author) and `approvedBy` (reviewer/SME) must be **distinct people**.
- **Tamper-evident review log.** Every approval writes a **PR-tied review-log entry** (PR #, commit
  SHA, `contentVersion`, reviewer + approver, sources checked, divergence/limitation notes, decision),
  append-only and committed in-repo.
- **Accessibility:** every UI-affecting change passes automated checks **and** a **manual
  assistive-technology audit** against a defined support matrix (NVDA+Firefox/Win, JAWS+Chrome/Win,
  VoiceOver+Safari/macOS·iOS, TalkBack+Chrome/Android, plus keyboard-only per desktop browser), at
  each milestone exit and each production release, signed off by the accessibility reviewer.
- **Product-neutrality gate.** A standing review check (and a CI lint where feasible) rejects brand
  recommendations, affiliate/tracking links, sponsored content, lead-gen forms, or any "buy this
  product" steer. Outbound links are limited to **non-commercial authoritative sources and official
  programs**; an allowlist/denylist is enforced in CI.

**Definition of Shipped (project-level).** A deployed, installable PWA that: (1) passes WCAG 2.2 AA
(automated + manual); (2) works offline after install (E2E); (3) ships only source-cited,
accuracy-reviewed guides and **golden-validated, methodology-reviewed calculators** within scope,
with safety-sensitive topics handled as link-out; (4) is **product-neutral** (no upsell/affiliate/
lead-gen) and collects **no telemetry/PII** (inputs stay on device); and (5) is
**adopted/endorsed by a named partner org and available in that community's languages.** Until a
partner is secured, criterion (5) is **outstanding** and the project is "publicly usable" but not yet
"shipped" by Elyos's *delivered, not merged* bar.

**"Publicly shipped (no partner)" success state.** Criteria (1)–(4) can be fully met without a
partner. If, by a **decision point at 6 months after the M4 build is production-ready**, no partner is
secured, the steward + Elyos governance may declare **"Publicly Shipped (generic public good)"** —
deployed, announced, and distributed via community/library/mutual-aid channels, outcomes tracked by
best-effort self-report. This is a recognized, honest success state; a later endorsement upgrades the
status rather than gating launch.

## Roadmap & milestones

Phased; each phase has measurable exit criteria. M0 is a thin cold-start foundation.

- **M0 — Foundation & cold-start (thin slice).**
  Goal: a minimal installable, offline PWA skeleton with a11y + privacy + product-neutrality
  guardrails in CI, the content + calculator schemas, and one sample guide + one sample calculator.
  Exit criteria: PWA installs and loads offline after first visit; CI runs lint/typecheck/unit/axe +
  offline smoke E2E + no-telemetry/PII (CSP `connect-src 'none'` + runtime network interception) +
  **calculator golden-reference harness**; **ADR #2 (calculator-model architecture) and ADR #3
  (content format) recorded before** the respective schemas are finalized; ADRs also recorded for UI
  framework (i18n/units scope settled as input), units/price store, SW tooling, hosting; one
  source-cited guide and one calculator (with methodology card, range output, golden cases, and
  "not financial advice" frame) render from the schema; product-neutral link policy enforced in CI.

- **M1 — Calculator engine + core guides + accessibility hardening.**
  Goal: the real value — a shared calculator engine with assumptions/ranges/"show your work," 2–3
  calculators, 4–5 core guides, all offline and AA-accessible, inputs on-device only.
  Exit criteria: calculator engine + shared model contract in place; ≥ 3 calculators golden-validated
  with complete methodology cards; ≥ 5 guides source-cited and reviewed; WCAG 2.2 AA pass + manual AT
  audit + 0 critical axe issues; inputs verified to stay on-device (no egress); printable/exportable
  output generated client-side.

- **M2 — i18n, units & first localization.**
  Goal: internationalization + measurement-system handling + at least one non-English locale (UI +
  ≥1 guide + ≥1 calculator), with translation accuracy review.
  Exit criteria: i18n framework with build-time completeness check; metric/imperial unit handling and
  locale formatting working; ≥ 1 non-English locale complete for UI, ≥1 guide and ≥1 calculator; safe
  fallback verified; translated content passes accuracy review.

- **M3 — Breadth, equity & safety-sensitive content.**
  Goal: broaden coverage, center energy-burdened/renter audiences, add the product-neutral
  program-finder pointer, and handle safety-sensitive topics correctly.
  Exit criteria: ≥ 6 calculators and ≥ 8 guides reviewed; renter + low-income tracks shipped;
  product-neutral program-finder pointer (no PII) shipped; safety-sensitive units pass the elevated
  (qualified-reviewer) gate as link-out; full source/license/provenance audit complete.

- **M4 — Partner adoption, deployment & outcomes.**
  Goal: secure a named partner, deploy production, and meet the full Definition of Shipped.
  Exit criteria: **named partner org endorsement/adoption on file** (verifiedNeed = true); deployed
  production build with versioned cache/update + disclaimers; outcomes-tracking process (partner
  self-report) in place; content/calculator re-validation cadence operational.

Dependencies: M1 depends on M0 (schemas + engine architecture). M2 depends on M1 (stable
content/calculators to localize). M3 depends on M1/M2 (engine + i18n). M4 depends on M3 and on the
**partner being secured** (outreach starts in parallel from M0).

## Work breakdown

The itemized, schema-mapped backlog lives in **TASKS.md**, organized by the M0–M4 milestones above.
Each task maps to an Elyos Task JSON (see schema), is sized (small/medium/large), risk-tagged, and
names a reviewer. TASKS.md also includes acceptance criteria for the most important tasks per
milestone, milestone Definitions of Done, a backlog, and a complete example Task JSON.

## Governance, roles & stakeholders

- **Maintainer (Owner): TBD.** Owns repo, roadmap, releases, review standards, product-neutrality
  enforcement.
- **Code reviewers:** rotation of TS/PWA-competent contributors; ≥ 1 approval + CI green to merge.
- **Calculator / methodology reviewers:** contributors with energy-modeling / building-science
  competence; verify model correctness, golden cases, and methodology cards.
- **Content reviewers:** contributors with home-energy-efficiency domain knowledge; for
  **safety-sensitive** units, a qualified building-science SME (credential bar above) signs off.
- **Accessibility reviewer:** contributor competent with assistive tech; performs manual audits.
- **Translation reviewers:** competent speakers per locale, paired with an accuracy re-check.
- **Steward (last-mile owner): TBD** — owns deployment, partner relationship, and getting the toolkit
  into beneficiaries' hands.
- **Partner / requestor: TO BE SECURED** — a named org (weatherization/community-action/energy-equity
  org, library system, or similar) confirming need, priority audiences/regions, and target languages.
- **Elyos governance/board:** arbitrates edge cases and risk-tier decisions per the good-deed
  definition; owns the product-neutrality and conflict-of-interest policy.

## Dependencies & integrations

- **External sources:** DOE/Energy.gov/Energy Saver, US EIA (incl. RECS, average prices), NREL
  (per-dataset license check), EPA/ENERGY STAR (factual reference only, trademark-aware), and relevant
  national energy agencies (read-only references; license-checked).
- **Tooling/libraries:** PWA/service-worker tooling (e.g. Workbox), UI framework (TBD), i18n + units
  library, test stack (Vitest, Playwright, axe-core/pa11y), client PDF/print.
- **Hosting:** static host (GitHub Pages / Netlify / Cloudflare Pages) — no app server.
- **Elyos pieces:** Task schema (`packages/schema`), CLI workspace prep / PR flow (donated lane),
  good-deed definition & risk-tier governance, review/sign-off process.
- **Human/expert dependency:** calculator/methodology reviewers, safety-content SMEs, and a partner
  org — the gating non-software dependencies.

## Risks & mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
| --- | --- | --- | --- | --- |
| Inaccurate calculator gives misleading savings/payback | Medium | High | Pure deterministic models; **golden-reference CI validation**; methodology card + sourced assumptions; methodology-reviewer sign-off; output as a **range** with uncertainty | Calculator/methodology reviewer |
| "Not advice" boundary blurred → reads as financial advice | Medium | Medium | Persistent "estimate, not financial advice" frame; show assumptions + range; never recommend a product/financing; governance review | Maintainer |
| Product-neutrality erodes (affiliate/brand/lead-gen creep) | Medium | High | Explicit non-goals; CI link allowlist/denylist; standing neutrality review; conflict-of-interest policy | Maintainer |
| Trademark/license misuse (ENERGY STAR mark, IEA/NREL data, utility tariffs) | Medium | High | Trademark-aware factual reference only; verify each source license; link-don't-embed for restrictive sources; provenance log | Maintainer |
| Safety-sensitive content invites hazardous DIY (CO, gas, electrical) | Medium | High | Link-out, never DIY steps; elevated qualified-reviewer gate; explicit "call a professional" framing | Content SME |
| Default energy prices / benchmarks go stale → wrong estimates | High | Medium | Labeled vintage + source; user override; scheduled re-validation cadence; staleness flag from `lastReviewed` | Calculator reviewer |
| No partner org secured (verified need unconfirmed) | Medium | High | Early parallel outreach; **6-month-post-M4 decision point** to declare "Publicly Shipped (generic public good)"; later endorsement upgrades status | Steward |
| Content goes stale vs. updated official guidance | High | Medium | `lastReviewed` + periodic re-review; provenance log flags age; maintenance tasks | Content reviewer |
| Accessibility regressions slip in | Medium | High | a11y CI gate blocking merge; periodic manual AT audits; AA hard requirement | A11y reviewer |
| Perceived partisanship on climate/energy | Low | Medium | Non-partisan framing (cost/comfort/health + neutral sourced emissions); present options with trade-offs; no advocacy | Maintainer |
| Privacy leak via calculator inputs (e.g. ZIP, income band) | Low | High | On-device-only compute/persistence; ZIP→climate-zone lookup only (no geolocation); zero-egress E2E; no PII fields | Maintainer |
| Hostile fork implies false agency/program endorsement | Low | Medium | "Not an official agency/program tool unless endorsed" disclaimer; trademark notice; license attribution | Maintainer |
| Maintainer bandwidth / bus factor | Medium | Medium | Reviewer rotation; documented processes; MIT/CC-BY low lock-in | Maintainer |

## Security & privacy

**Threat surface.** As a static, no-backend, no-PII PWA the attack surface is small. Principal
concerns: (1) supply-chain risk in dependencies; (2) service-worker cache poisoning / stale-content
(esp. a corrected calculator or safety note); (3) third-party script/tracker/affiliate creep;
(4) leakage of user-entered inputs; (5) hostile forks implying false endorsement.

**Controls.**
- **No secrets** in the app; nothing to leak. No API keys/tokens in code, logs, or receipts (Elyos
  rule). Static hosting only.
- **No telemetry / no PII / inputs stay on device:** defense in depth — (1) strict CSP with
  **`connect-src 'none'`** (plus locked `script-src`/`img-src`/`font-src 'self'`) blocks runtime
  exfiltration; (2) a **runtime network-interception E2E** exercises every flow and fails the build on
  any unexpected outbound request; (3) a static CI audit (no analytics/trackers/PII fields, no
  affiliate/tracking links) as a cheap first line. Calculator inputs are computed and persisted
  locally and exported only by explicit action.
- **Supply chain:** pinned/locked deps (pnpm lockfile), dependency review, minimal deps, Subresource
  Integrity where applicable, CI dependency audit.
- **Service-worker hygiene:** scoped SW, integrity-checked precache manifest, explicit versioning;
  **content-version manifest** with per-unit `integrityHash`; **hard-invalidation** purges and forces
  a re-fetch of any corrected guide/calculator before it can be shown (non-dismissible for that unit)
  — a wrong savings number or safety note can never be served stale; HTTPS-only.
- **Product-neutrality as a security/abuse control:** CI link allowlist/denylist blocks affiliate or
  tracking domains; standing review rejects upsell/lead-gen.
- **Abuse/misuse:** prominent disclaimer that the tool is not an official agency/program/vendor
  product unless a named partner endorses it; trademark/branding terms documented; license requires
  attribution.

## Sustainability & maintenance

- **Ownership after delivery:** the **maintainer** owns code/releases; the **steward** owns
  deployment and the partner relationship. Both are currently **TBD** and must be named before M4.
- **Content & calculator freshness:** units carry `lastReviewed`; a **re-validation cadence** (at
  least annually, and after material changes to official guidance) covers guides **and** calculator
  assumptions/default prices/emissions factors; stale units are flagged from `lastReviewed`.
- **Outcomes tracking:** no telemetry, so outcomes (adoption, reach, measures identified) are tracked
  via **partner self-report** and a manual record of endorsements/MOUs — an explicit privacy/
  measurement trade-off.
- **Low lock-in:** MIT/CC-BY, static hosting, standard web tech, pure-function calculator models keep
  the project forkable, testable, and cheap to run.

## Open questions

1. **Partner org:** Who is the named partner (weatherization/community-action/energy-equity org,
   library system)? No partner exists yet — a human decision is needed before *Definition of Shipped*.
2. **Priority audiences/regions:** Which audiences and regions first (should be partner-driven)?
   Initial assumption is US-centric sources (DOE/EIA) with a renter + energy-burdened focus.
3. **Priority languages:** First non-English locale is **provisionally Spanish (es)** at M2; a partner
   can override for its community.
4. **Geographic scope of reference data:** US-first (climate zones, EIA prices) vs. multi-country —
   non-US climate/price/benchmark data has licensing and accuracy implications.
5. **Calculator golden references:** which authoritative worked examples / established tools are the
   reference oracles, and what tolerances are defensible per calculator?
6. **Legal review of source reuse:** is a one-time check of our paraphrase/citation stance and the
   **ENERGY STAR / IEA / utility-rate** handling warranted?
7. **Default-price strategy:** EIA averages vs. regional defaults; cadence and source for refresh.
8. **Hosting/deployment target** and who operates it (steward).

## References

- Elyos work rules — `C:\code\elyos\CLAUDE.md`
- Good-deed definition & risk tiers — `C:\code\elyos\docs\good-deed-definition.md`
- Task schema — `C:\code\elyos\packages\schema\src\schemas.ts`
- Portfolio roadmap — `C:\code\elyos\planning\ROADMAP.md`
- Project proposal — **none on file** (`governance/proposals/home-energy-savings.md` TO BE CREATED)
- Authoritative sources (license-checked per use): US DOE / Energy.gov / Energy Saver, US EIA (incl.
  RECS), NREL (per-dataset license), EPA / ENERGY STAR (factual, trademark-aware), relevant national
  energy agencies.
- WCAG 2.2 AA (W3C Web Content Accessibility Guidelines).

---

## Appendix A — Improvements applied

The following 25 specific improvements were identified against the first draft and have been
**applied** to the plan above (and to TASKS.md). Each is a concrete change, not an aspiration.

1. **Golden-reference validation gate added.** Every calculator must pass CI assertions against
   authoritative worked examples within a declared tolerance (metrics table, quality gates, CI task).
2. **Calculators as pure deterministic functions.** Architecture mandates no-I/O models so they are
   unit-testable and golden-validatable (architecture, ADR #2).
3. **Assumptions register + methodology card** made a first-class, machine-readable schema element so
   every constant is sourced and dated (data model, licensing, quality gates).
4. **Range output, not false precision.** Calculators emit `{central, low, high}` and the UI shows a
   range + "show your work," never a single dollar number (architecture, gates).
5. **"Estimate, not financial advice" frame** made a persistent, enforced UI element and a ship gate.
6. **Safety-sensitive sub-gate carved out** (combustion/gas/electrical/CO, mold, lead/asbestos) as
   link-out with a qualified-reviewer credential bar — even though the project is low-risk overall.
7. **Product-neutrality elevated to identity + an enforceable gate** (CI link allowlist/denylist;
   standing review rejecting affiliate/brand/lead-gen).
8. **ENERGY STAR trademark caution** added explicitly — factual reference only, no implied
   endorsement, product-neutral framing.
9. **IEA / restrictive-license caution** added — link, don't embed, unless an open license is
   confirmed.
10. **Utility-rate handling clarified** — labeled default prices for estimation only, user override,
    vintage shown, no scraping/redistribution of proprietary tariffs.
11. **ZIP-not-geolocation privacy stance** — postcode used only for a local climate-zone lookup; no
    precise location; income band optional and on-device.
12. **No headline "total dollars saved" vanity metric** — report households that identified an
    actionable measure instead, avoiding overstated certainty/marketing drift.
13. **Decision ordering made explicit** — ADR #2 (calculator architecture) and ADR #3 (content
    format) land before their schemas/harness (architecture, roadmap, TASKS sequencing).
14. **Measurement-system handling (metric/imperial)** promoted to an explicit M2 scope item and CI
    completeness check, not an afterthought.
15. **Renter + low-income/energy-burden tracks** made explicit goals, scope, M3 tasks, and a metric —
    centering equity rather than assuming a capital-rich homeowner.
16. **Product-neutral program-finder pointer** added (LIHEAP/WAP/utility rebates) with no PII and no
    eligibility *determination*.
17. **Non-partisan / no-advocacy stance** added given the politically charged domain (licensing,
    non-goals, risks).
18. **Hard-invalidation extended to calculators**, not just guides — a corrected savings number can
    never be served stale from cache (architecture, security).
19. **Re-validation cadence covers calculator assumptions/default prices/emissions factors**, not
    only prose guides (sustainability, backlog).
20. **No-self-approval + PR-tied tamper-evident review log** carried over and applied to both guides
    and calculators.
21. **Defense-in-depth privacy verification** (CSP `connect-src 'none'` + runtime network-interception
    E2E + static audit) — a static grep alone does not pass.
22. **Reach measurement hardened** — distinct-household denominator, rolling 12-month window,
    per-period de-duplication across partners/channels.
23. **"Publicly Shipped (generic public good)" success state + 6-month decision point** added so a
    finished, partner-less toolkit is never stranded.
24. **Honest partner status throughout** — `verifiedNeed=false`, `requestor=TBD`, and a stated
    absence of any proposal file, rather than inventing a partner.
25. **WCAG 2.2 AA framed as an equity requirement** (energy burden correlates with disability/age/low
    literacy) with a concrete manual-AT support matrix and cadence, not a generic "accessible" claim.

## Review sign-off

A completeness/correctness review of this plan and TASKS.md was performed against the PLAN_SPEC,
CLAUDE.md guardrails, the good-deed definition, and the Task schema. Findings and resolutions:

- **Measurable metrics:** PASS — every success metric has a baseline + target + privacy-preserving
  measurement method; vanity and false-precision metrics removed (Appendix A #12, #22).
- **Enforceable gates:** PASS — CI gates (lint/type/unit, a11y, offline E2E, no-telemetry/PII,
  golden-reference calculator validation, product-neutral link policy) are concrete and build-failing;
  Definition of Shipped is explicit.
- **Risks with owners + mitigations:** PASS — risk table has likelihood, impact, mitigation, and a
  named owner role for each row, including the calculator-accuracy, neutrality, trademark, safety, and
  privacy risks unique to this project.
- **License / PII / expert guardrails:** PASS — open/PD/CC-only stance with explicit ENERGY STAR
  (trademark), IEA (restrictive), NREL (mixed), and utility-rate cautions; zero-PII with on-device
  inputs and ZIP-not-geolocation; safety-sensitive content gated behind a qualified reviewer with a
  stated credential bar; "not financial advice" framing enforced.
- **Sequencing:** PASS — ADR #2/#3 precede their schemas/harness; M0→M4 dependencies stated; partner
  outreach runs in parallel and gates only criterion (5).
- **Schema-valid tasks:** PASS — TASKS.md maps every field to `packages/schema/src/schemas.ts`; the
  example Task JSON uses only allowed enum values and required fields, with `verifiedNeed=false` and
  `requestor="TBD"` (no partner secured). Funded-lane note included (all tasks here are donated, so
  `fundedBudgetUsd` is not required).
- **Fixes applied during review:** clarified that the project-level risk tier is *low* with a *medium
  sub-gate* (so low-risk doesn't imply no review on money/safety units); added the product-neutrality
  CI link policy as an explicit gate; tied the calculator hard-invalidation path to safety/correctness;
  ensured the renter/low-income equity track has a dedicated metric and tasks.

**Outstanding human decisions (cannot be resolved by the plan):** (1) name a partner org and obtain
written confirmation of need; (2) confirm geographic/data scope (US-first vs. multi-country) given
licensing; (3) name the maintainer and steward; (4) decide whether a one-time legal review of the
source-reuse / ENERGY STAR / IEA / utility-rate stance is warranted; (5) pick the calculator golden
references and per-calculator tolerances. Status: **Draft — ready for maintainer/steward review.**

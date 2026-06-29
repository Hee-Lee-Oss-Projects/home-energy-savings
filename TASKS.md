# TASKS — home-energy-savings (product-neutral home energy-efficiency guides + calculators)

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated

## How these tasks map to Elyos

Each task below becomes an Elyos **Task JSON** validated against
`packages/schema/src/schemas.ts`. Field mapping:

- `id` — stable slug ID, e.g. `home-energy-savings-app-001`.
- `title` — the task title from the table.
- `project` — `"home-energy-savings"`.
- `type` — one of `code | research | writing | data | design-spec | maintenance` (the "Type" column).
- `lane` — `"donated"` for all tasks here (no escrow/API spend). A `funded` task would additionally
  require `fundedBudgetUsd` (per the schema's `if/then`).
- `priority` — `high | medium | low`.
- `domain` — array, e.g. `["environment","energy-efficiency","software","accessibility"]`.
- `riskTier` — `low | medium | high` (the "Risk" column). The project is **low** overall, with a
  **medium** sub-gate for any task that produces a **financial/savings figure** (calculators) or
  **safety-sensitive** content (combustion/gas/electrical/CO, mold, lead/asbestos).
- `urgent` — boolean (default `false`; this is reference content, not live response).
- `deliverable` — `pr | dataset | document | translation` (the "Deliverable" column).
- `tokenEstimate` — `small | medium | large` (the "Size" column).
- `status` — `open | in-progress | review | delivered | done` (all start `open`).
- `context`, `objective`, `acceptanceCriteria[]`, `output` — task narrative + checkable criteria.
- `resources[]` — source/reference URLs or repo paths.
- `requestor` — partner/requestor (**TO BE SECURED**; use `"TBD"` until a partner is named).
- `verifiedNeed` — **`false` until a named partner org confirms need in writing** (honest default).
- `outputLicense` — `"MIT"` for code, `"CC-BY-4.0"` for content/calculator methodology/translations.

**Reviewer column legend:** `maintainer` (code review); `a11y` (accessibility reviewer); `calc/SME`
(energy-modeling / building-science methodology reviewer for calculators and any savings figure);
`content/SME` (home-energy-efficiency domain reviewer; for **safety-sensitive** units the approver
must meet the credential bar — current/former building-science professional, e.g. BPI- or
RESNET/HERS-qualified, licensed contractor/inspector, or equivalent); `translation+accuracy`
(competent speaker paired with accuracy re-check); `steward` (last-mile/partner owner).

**Content/calculator review rule (all `content/SME` and `calc/SME` tasks):** the author may **not**
approve their own work — `reviewedBy` (author) and `approvedBy` (reviewer/SME) must be distinct
people. Each approval writes a **PR-tied, append-only review-log entry** (PR #, commit SHA,
`contentVersion`, reviewer + approver, sources checked, assumptions/divergence/limitation notes,
decision). This is the auditable record the Definition of Shipped checks against.

**Product-neutrality rule (all tasks):** no brand recommendations/rankings, no affiliate or tracking
links, no sponsored content, no lead-gen/quote forms. Outbound links are restricted to non-commercial
authoritative sources and official programs, enforced by a CI link allowlist/denylist.

**Sequencing rules (M0):** ADR #2 (calculator-model architecture) is decided in **arch-002 before**
the calculator schema/golden harness; ADR #3 (content format) is decided in **arch-002 before** the
content schema (**data-004**). Until then schemas are provisional and format-agnostic. `data-004`,
`calc-006`, and `calc-007` therefore depend on `arch-002`.

---

## Milestone M0 — Foundation & cold-start

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| home-energy-savings-app-001 | Installable PWA skeleton (offline shell, zero-telemetry/PII posture) | code | medium | low | pr | — | maintainer |
| home-energy-savings-arch-002 | ADRs: UI framework (i18n/units as input), **calculator-model architecture (decide first)**, **content format (decide first)**, units/price store, SW tooling + content-version manifest, hosting | design-spec | small | low | document | — | maintainer |
| home-energy-savings-ci-003 | CI gates: lint/typecheck/unit, axe a11y, offline smoke E2E, no-telemetry/PII (CSP `connect-src 'none'` + runtime network interception), **calculator golden-reference harness**, product-neutral link policy | code | medium | low | pr | app-001 | maintainer |
| home-energy-savings-data-004 | Guide + calculator schema (sources[], assumptions register, methodology card, range output, contentVersion/integrityHash, safetySensitive, notAdvice) | data | small | low | dataset | arch-002 | maintainer, calc/SME |
| home-energy-savings-content-005 | Sample guide ("where does my home energy go?"), source-cited to public-domain DOE/EIA guidance | writing | small | low | document | data-004 | content/SME |
| home-energy-savings-calc-006 | Sample calculator (LED lighting savings) with methodology card, range output, golden cases, "not financial advice" frame | code | medium | medium | pr | data-004, ci-003 | maintainer, calc/SME |

**Acceptance criteria — key tasks**

- **home-energy-savings-app-001 (PWA skeleton):**
  - App registers a service worker that precaches the app shell; loads after first visit with the
    network fully disabled.
  - Web app manifest present; app is installable (passes Lighthouse PWA installability).
  - Explicit update flow (prompt or controlled reload); offline fallback page exists.
  - No third-party analytics/trackers/affiliate scripts; no network egress beyond initial load/update
    check, enforced by CSP `connect-src 'none'`.
  - TypeScript/ESM; builds via pnpm; `pnpm build && pnpm test && pnpm lint` pass.
- **home-energy-savings-ci-003 (CI gates):**
  - CI fails on lint/type/unit errors, any critical axe violation, and offline E2E failure.
  - A "no-telemetry/no-PII" check uses defense in depth: static audit (trackers/analytics/PII fields/
    affiliate domains) **plus** a runtime network-interception E2E failing on any unexpected outbound
    request; CSP `connect-src 'none'` asserted.
  - A **calculator golden-reference harness** exists and runs in CI: a calculator with declared golden
    cases fails the build if any output falls outside its declared tolerance.
  - A **product-neutral link policy** check rejects affiliate/tracking domains and non-allowlisted
    commercial outbound links.
- **home-energy-savings-data-004 (schema):**
  - Captures, for guides: id, localized fields, audience, region applicability, `sources[]`
    (org/title/url/retrievedDate/sourceLicense), `safetySensitive`, `reviewStatus`, `reviewedBy`,
    `approvedBy` (distinct), `reviewLogRef`, `lastReviewed`, `lang`, `contentLicense`,
    `contentVersion`, `integrityHash`, `hardInvalidate`.
  - Captures, for calculators: `inputs[]`, an **assumptions register** (`{key,value,unit,source,
    rationale,lastReviewed}`), a **methodology card** (plain explanation, formula ref, validity range,
    limitations), a result contract `{central,low,high,units,breakdown}`, `goldenCases[]`, `notAdvice`.
  - Format follows the content-format + calculator-architecture ADRs (arch-002), which land first;
    schema is provisional until then.
  - Build-time validation rejects: a unit missing required citation/review fields; a unit where
    `approvedBy == reviewedBy`; a calculator constant with no source; a calculator with no golden case.
- **home-energy-savings-calc-006 (sample calculator):**
  - Model is a **pure deterministic function** (no I/O); every constant/default is sourced in the
    assumptions register with a `lastReviewed` date.
  - Output is rendered as a **range** with a visible "show your work" methodology panel and a
    persistent **"estimate, not financial advice"** frame; no product/brand is recommended.
  - Passes golden-reference cases in CI within the declared tolerance, with the reference source cited.
  - Passes `calc/SME` methodology review by an approver **distinct from the author**; PR-tied review-log
    entry written.

**Definition of Done (M0):** Installable PWA loads fully offline after first visit; CI enforces
lint/type/unit/a11y/offline/no-telemetry-PII/golden-reference/product-neutral gates; guide +
calculator schemas (with assumptions register + methodology card) defined; ADRs recorded (calculator
architecture and content format **before** their schemas); one source-cited guide and one
golden-validated, methodology-reviewed calculator render from the schema; telemetry/PII audit green.

---

## Milestone M1 — Calculator engine, core guides & accessibility

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| home-energy-savings-calc-007 | Calculator engine + shared model contract (inputs → model → {central,low,high} + assumptions/range/"show your work" UI) | code | large | medium | pr | calc-006 | maintainer, calc/SME |
| home-energy-savings-calc-008 | ≥ 3 core calculators (thermostat setback, insulation/air-sealing payback, water-heating savings), each with methodology card + golden cases | code | large | medium | pr | calc-007 | maintainer, calc/SME |
| home-energy-savings-content-009 | Core guide set (air sealing, insulation, heating & cooling, water heating, appliances & lighting), source-cited | writing | large | low | document | content-005 | content/SME |
| home-energy-savings-a11y-010 | WCAG 2.2 AA hardening + manual assistive-tech audit | code | medium | low | pr | app-001, calc-007 | a11y |
| home-energy-savings-priv-011 | On-device input persistence + zero-egress verification + ZIP→climate-zone local lookup (no geolocation/PII) | code | medium | low | pr | app-001 | maintainer |

**Acceptance criteria — key tasks**

- **home-energy-savings-calc-007 (calculator engine):**
  - A documented shared model contract; all calculators implement it; models remain pure functions.
  - The UI renders `{central, low, high}` as a range, exposes the assumptions register and a "show
    your work" methodology panel, and shows the persistent "estimate, not financial advice" frame.
  - Inputs are validated and bounded to each model's validity range; out-of-range inputs are handled
    gracefully (clamped/flagged), never producing silent nonsense.
  - Fully usable offline; no network egress.
- **home-energy-savings-calc-008 (core calculators):**
  - Each calculator has a complete methodology card (sourced assumptions, formula, validity range,
    limitations) and passes its golden-reference cases in CI within declared tolerance.
  - Each passes `calc/SME` methodology review (approver distinct from author); review-log entry written.
  - No calculator recommends a specific product, brand, or financing option.
- **home-energy-savings-priv-011 (privacy):**
  - User-entered inputs persist locally (IndexedDB/localStorage) and **never** leave the device
    (verified by the zero-egress E2E).
  - ZIP/postcode is used **only** for a local climate-zone lookup against bundled data — no precise
    geolocation, no geocoding API call; optional income band stays on-device and is never transmitted.

**Definition of Done (M1):** Calculator engine + shared contract in place; ≥ 3 calculators
golden-validated with complete methodology cards; ≥ 5 guides source-cited and reviewed; WCAG 2.2 AA
pass (automated + manual AT) with 0 critical axe issues; inputs confirmed to stay on-device;
printable/exportable output generated client-side.

---

## Milestone M2 — i18n, units & first localization

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| home-energy-savings-i18n-012 | i18n framework (RTL, bundled offline fonts, ICU plural/select, locale formatting) + **metric/imperial unit system** + currency/locale handling | code | medium | low | pr | calc-007, content-009 | maintainer, a11y |
| home-energy-savings-i18n-013 | Translation + unit-system completeness check in CI | code | small | low | pr | i18n-012 | maintainer |
| home-energy-savings-l10n-014 | First non-English localization (Spanish/es, provisional) — UI + ≥1 guide + ≥1 calculator, accuracy-reviewed | writing | medium | medium | translation | i18n-012 | translation+accuracy, content/SME |

**Acceptance criteria — key tasks**

- **home-energy-savings-i18n-012 (i18n + units):**
  - Strings externalized to message catalogs; locale negotiation falls back safely to English when a
    key/locale is missing (no blank/broken content).
  - Supports bidirectional/RTL layout (logical CSS, `dir`, mirrored components), bundled offline fonts
    with required glyph coverage (no runtime web-font fetch), ICU pluralization/select, and
    locale-aware number/currency/date/unit formatting.
  - **Measurement-system toggle (metric/imperial)** drives both calculator inputs and outputs
    consistently; conversions are exact and unit-tested. The i18n + units layer works offline.
- **home-energy-savings-l10n-014 (first localization):**
  - Translation is accuracy-reviewed for meaning (incl. numbers/units in worked examples), not only
    fluency, against the source unit and authoritative guidance in that language where available.
  - Source license/attribution rules respected for derivative content; `contentLicense` = CC-BY-4.0.

**Definition of Done (M2):** i18n framework with build-time completeness check in CI; metric/imperial
handling and locale formatting working and tested; ≥ 1 non-English locale complete for UI, ≥1 guide,
and ≥1 calculator; safe fallback verified; translated content passes accuracy review.

---

## Milestone M3 — Breadth, equity & safety-sensitive content

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| home-energy-savings-content-015 | Energy-burden / low-income & renter tracks + **product-neutral program-finder pointer** (LIHEAP/WAP/utility rebate directories; no PII; no eligibility decision) | writing | large | medium | document | content-009 | content/SME, steward |
| home-energy-savings-content-016 | Safety-sensitive content (combustion/CO, gas, electrical, moisture/mold, lead/asbestos) — **link-out, not DIY**; elevated qualified review | writing | medium | medium | document | content-009 | content/SME |
| home-energy-savings-calc-017 | Expand to ≥ 6 calculators (appliance-replacement payback, weatherization bundle, home-energy breakdown, **educational** solar-suitability primer) | code | large | medium | pr | calc-008 | maintainer, calc/SME |
| home-energy-savings-research-018 | Source/provenance + license audit across all cited data (EIA PD, DOE PD, ENERGY STAR trademark, IEA restrictive, NREL mixed, default prices) | research | medium | medium | document | content-009, calc-008 | maintainer, calc/SME |

**Acceptance criteria — key tasks**

- **home-energy-savings-content-015 (equity + program finder):**
  - Dedicated renter track (behavior, low/no-cost measures, landlord conversation, rights pointers)
    and low-income/energy-burden track, each source-cited and reviewed.
  - Program-finder pointer links **only** to official directories (LIHEAP/WAP/utility rebate finders),
    collects no inputs that leave the device, and makes **no eligibility determination** — it points,
    it does not decide.
- **home-energy-savings-content-016 (safety-sensitive content):**
  - Content is **link-out, not DIY instruction**: it explains the risk and "when/why to call a
    qualified professional," and links to authoritative guidance; it gives no step-by-step for gas/
    combustion/electrical/abatement work.
  - Approved by a reviewer meeting the **credential bar** (building-science professional / licensed
    trade / equivalent), distinct from the author; review-log entry written; `safetySensitive=true`.
- **home-energy-savings-calc-017 (expanded calculators):**
  - Each new calculator has a methodology card + golden cases passing in CI within tolerance.
  - The solar-suitability primer is **educational only** — no installer/brand steer, no financing
    recommendation, explicit "estimate, not financial advice" and "consult a qualified assessor" frame.
- **home-energy-savings-research-018 (license audit):**
  - Each cited source/dataset has a recorded license/terms and a reuse decision (embed-with-attribution
    vs. paraphrase-and-cite vs. link-only); ENERGY STAR (trademark), IEA (restrictive), NREL (mixed),
    and utility-rate sources are explicitly classified.
  - Any non-compliant reuse is flagged and remediated before M3 exit.

**Definition of Done (M3):** ≥ 6 calculators and ≥ 8 guides reviewed; renter + low-income tracks and
the product-neutral program-finder pointer shipped; safety-sensitive units pass the elevated
qualified-reviewer gate as link-out; full source/license/provenance audit complete and remediated.

---

## Milestone M4 — Partner adoption, deployment & outcomes

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| home-energy-savings-partner-019 | Secure named partner org (need, priority audiences/regions, languages, endorsement) | research | medium | medium | document | — | maintainer, steward |
| home-energy-savings-deploy-020 | Production deployment + versioned cache/update + disclaimers (not-advice, not-affiliated, product-neutral) | code | medium | low | pr | a11y-010, i18n-013 | maintainer, steward |
| home-energy-savings-ops-021 | Outcomes-tracking process (partner self-report) + content/calculator re-validation cadence (prices, benchmarks, guidance drift) | maintenance | small | low | document | partner-019 | maintainer, steward |

**Acceptance criteria — key tasks**

- **home-energy-savings-partner-019 (secure partner):**
  - A named org (weatherization/community-action/energy-equity org, library system, or similar)
    confirms the need in writing (letter of support or MOU) and identifies priority audiences,
    regions, and languages.
  - On success, `verifiedNeed` flips to `true` and `requestor` is set to the named org across the
    project's tasks.
- **home-energy-savings-deploy-020 (production deploy):**
  - Static production build deployed over HTTPS; versioned precache driven by the content-version
    manifest with a soft update prompt for routine changes **and** a non-dismissible hard-invalidation
    path that purges/re-fetches any corrected guide **or** calculator, so users are never stuck on a
    stale savings number or safety note.
  - Visible disclaimers: "estimate, not financial advice"; "product-neutral — no endorsements"; "not
    an official agency/program/vendor tool unless a named partner endorses it." Trademark/attribution
    terms present.
- **home-energy-savings-ops-021 (outcomes + freshness):**
  - Documented, privacy-preserving outcomes process (partner self-report; no in-app telemetry) using
    the standard template (partner, period, channels, **distinct households reached (confirmed)**,
    flagged estimates, regions/languages, de-dup notes) over a rolling 12-month window; counts
    distinct households (not downloads/page views) and de-duplicates across partners at roll-up.
  - Re-validation cadence defined covering **guides and calculator assumptions/default prices/emissions
    factors**; stale-unit flagging based on `lastReviewed`.

**Definition of Done (M4):** **Named partner endorsement/adoption on file** (verifiedNeed = true);
production build deployed with safe soft/hard update strategy and required disclaimers; outcomes
tracking + content/calculator re-validation cadence operational. This satisfies the project-level
*Definition of Shipped (partner-adopted)*.

**Decision point (so a finished toolkit isn't stranded):** if no partner is secured by **6 months
after the M4 production build is ready**, the steward + Elyos governance declare **"Publicly Shipped
(generic public good)"** — criteria (1)–(4) met, deployed and distributed directly/via
library/community/mutual-aid channels, outcomes tracked by best-effort self-report. This is a
recognized success state; a later partner endorsement upgrades the status to "partner-adopted" rather
than re-opening launch.

---

## Backlog / future

| ID | Title | Type | Size | Risk | Deliverable | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| home-energy-savings-feat-022 | Printable/exportable "home energy action plan" from a session (client-side, no PII) | code | medium | low | pr | Aggregates user-selected actions + estimates |
| home-energy-savings-data-023 | Multi-country climate-zone + benchmark/price data packs (license-checked) | data | large | medium | dataset | Non-US data has licensing/accuracy implications |
| home-energy-savings-ops-024 | Annual re-validation pass: default prices, emissions factors, benchmarks, guidance drift | maintenance | medium | medium | document | Recurring; covers calculator assumptions |
| home-energy-savings-content-025 | Accessible easy-read / low-literacy guide variants | writing | medium | low | document | a11y + content review |
| home-energy-savings-content-026 | Additional localizations (incl. ≥1 RTL locale to exercise RTL end-to-end) | writing | large | medium | translation | translation+accuracy |
| home-energy-savings-feat-027 | Embeddable calculator widgets for partner/library sites (no tracking) | code | medium | low | pr | Product-neutral, zero-telemetry constraint preserved |
| home-energy-savings-sec-028 | Dependency/supply-chain audit + SRI hardening | maintenance | small | low | pr | Recurring |

---

## Example task JSON

Complete, schema-valid Task JSON for the first M0 task. `verifiedNeed` is `false` and `requestor` is
`"TBD"` because **no partner org is secured yet** (honest default per the plan; no proposal file
exists on record).

```json
{
  "id": "home-energy-savings-app-001",
  "title": "Installable PWA skeleton (offline shell, zero-telemetry/PII posture)",
  "project": "home-energy-savings",
  "type": "code",
  "lane": "donated",
  "priority": "high",
  "domain": ["environment", "energy-efficiency", "software", "accessibility"],
  "riskTier": "low",
  "urgent": false,
  "deliverable": "pr",
  "tokenEstimate": "medium",
  "status": "open",
  "context": "home-energy-savings is a free, product-neutral, installable PWA giving households plain-language home energy-efficiency guides and transparent savings calculators sourced from authoritative public agencies, with no product upsell, no telemetry, and no PII. This task lays the thin M0 foundation: an installable app shell with a service-worker offline cache and the privacy/no-telemetry/product-neutral posture baked in from day one.",
  "objective": "Create a TypeScript/ESM PWA skeleton that installs, registers a service worker precaching the app shell, loads fully offline after first visit, and ships no telemetry, trackers, or affiliate scripts.",
  "acceptanceCriteria": [
    "Service worker precaches the app shell; the app loads with the network fully disabled after first visit",
    "Web app manifest present and the app passes Lighthouse PWA installability checks",
    "Explicit update flow (update prompt or controlled reload) and an offline fallback page exist",
    "No third-party analytics, trackers, or affiliate scripts and no network egress beyond initial load and update check (CSP connect-src 'none')",
    "TypeScript/ESM project builds with pnpm; pnpm build && pnpm test && pnpm lint all pass"
  ],
  "resources": [
    "C:\\code\\elyos\\planning\\projects\\home-energy-savings\\PLAN.md",
    "C:\\code\\elyos\\packages\\schema\\src\\schemas.ts",
    "https://web.dev/learn/pwa/",
    "https://www.energy.gov/energysaver/energy-saver"
  ],
  "output": "A pull request adding the installable, offline-capable PWA skeleton (service worker, manifest, app shell, update flow) with CI-ready build and the zero-telemetry/PII posture in place.",
  "requestor": "TBD",
  "verifiedNeed": false,
  "outputLicense": "MIT"
}
```

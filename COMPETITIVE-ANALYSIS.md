# Competitive & Improvement Analysis — home-energy-savings

Scope: open, plain-language, product-neutral home-energy guidance + transparent savings
calculators for households (esp. low-income / energy-burdened, renters), sourced to
authoritative bodies, region/climate-aware. Reviewed against PLAN.md v0.1.0 (2026-06-28) and
the project guardrails (accuracy of savings claims, sourcing, region-dependence, safety limits,
open license, equity). Research current as of 2026-06-29; web sources cited inline.

---

## 1. Correctness & completeness review of PLAN.md

The plan is unusually disciplined for this domain. It already internalizes most of the traps that
sink commercial energy-advice content: it mandates `{central, low, high}` ranges instead of
false-precision dollar figures, golden-reference CI validation, sourced assumptions registers,
an "estimate, not financial advice" frame, ENERGY STAR trademark caution, IEA/NREL/utility-rate
license caution, ZIP→climate-zone (not geolocation), and a renter/low-income equity track. These
are correct and well above baseline. Findings below are gaps, not contradictions.

**1a. Accuracy of savings claims — strong framing, but the climate/fuel/heat-pump dependency is
under-specified.** The plan's "range, not a number" rule is the right instinct, but it does not yet
encode *why* the range is wide. The canonical example proves the point: DOE's "save up to 10% by
setting back 7–10°F for 8 hours" applies to furnace/AC homes, and **explicitly does not transfer to
heat pumps**, where deep setbacks trigger backup resistance-heat strips and can erase the savings —
the guidance for heat pumps is ~2–3°F setback, or none
(https://www.energy.gov/energysaver/programmable-thermostats). Any thermostat-setback calculator
that doesn't branch on heating-system type will produce a *plausible but wrong* number for the
growing heat-pump population. Likewise ENERGY STAR's air-sealing+insulation figure is "an average of
15% on heating/cooling, ~11% on total energy" — an *average*, not a floor or a promise, and highly
dependent on starting envelope condition (https://www.energystar.gov/saveathome/seal_insulate). The
plan should make **heating-system type and climate zone first-class, required inputs** to any
calculator whose result depends on them, and require each methodology card to state the *validity
envelope* ("this estimate assumes a gas furnace in IECC zones 4–6; not valid for heat pumps").
Recommend adding a golden case per fuel/system type, not just one per calculator.

**1b. Program/rebate currency — the single biggest factual-rot risk, and the plan understates it.**
The plan treats default *energy prices* as the staleness risk ("High likelihood" in the risk table)
but treats the program-finder as a static link list. That is now backwards. As of the 2025 "One Big
Beautiful Bill," the **federal 25C Energy Efficient Home Improvement Credit and 25D Residential Clean
Energy Credit both terminated for anything placed in service after Dec 31, 2025** — they no longer
exist in 2026 (https://www.irs.gov/credits-deductions/energy-efficient-home-improvement-credit;
https://www.criadv.com/insight/energy-tax-credits-after-obbba/). Separately, IRA HEAR/HEEHRA
electrification rebates are live in only ~13 states + DC and several have *paused* programs amid
federal-funding uncertainty (https://homes.rewiringamerica.org/calculator/faqs). A guide that still
says "claim the 25C credit" in mid-2026 is not stale-but-harmless; it is actively wrong and could
cause a household to spend money expecting a credit that no longer exists. **The program/incentive
layer needs its own dated `lastVerified` + hard-invalidation, a shorter re-validation cadence than
prose guides (quarterly, not annual), and a "verify current status at the official link" frame on
every incentive mention.** This is the most important correctness finding alongside 1a.

**1c. Sourcing — solid, with two refinements.** DOE/Energy.gov/EIA are correctly flagged as
generally public-domain US-federal works. Two adds: (i) **EIA RECS** (Residential Energy Consumption
Survey) is the right public-domain oracle for the "where does my home energy go?" breakdown and
end-use benchmarks — name it explicitly as a golden-reference source. (ii) The plan should treat
ENERGY STAR's own published **"Methodology for Estimated Energy Savings"** page
(https://www.energystar.gov/saveathome/seal_insulate/methodology) as a citable reference oracle for
the insulation/air-sealing calculator, which strengthens golden validation.

**1d. Region/climate-dependence — named but not operationalized for non-US.** US-first via IECC/DOE
climate zones + EIA prices is defensible. But "appliesTo: regionTags[]" is advisory metadata; there
is no rule preventing a guide written for a cold US climate from being shown, unqualified, to a user
in a hot-humid or a non-US context. Recommend a **validity gate**: a calculator/guide must declare
its climate-zone and fuel validity, and the UI must visibly degrade ("this estimate may not apply to
your region") outside it rather than silently extrapolate.

**1e. Renter vs owner + low-income programs — good coverage, one accuracy fix.** The renter track and
LIHEAP/WAP/utility-rebate pointer are correctly scoped as no-PII, no-eligibility-determination. Get
the facts right and dated: **WAP eligibility is ≤200% of poverty (or state LIHEAP criteria, often 60%
of state median income); both homeowners AND renters are eligible** (with landlord approval); FY2026
LIHEAP is funded at ~$4.045B with up to 15% transferable to WAP
(https://www.energy.gov/cmei/scep/wap/how-apply-weatherization-assistance;
https://building-performance.org/fy26-appropriations-update-funding-increase-for-liheap/). The plan
should also explicitly surface the renter-relevant fact that IRA electrification rebates *can* be
used by renters with landlord approval — renters are often wrongly told they're excluded.

**1f. Safety limits — appropriately conservative.** Link-out-not-DIY for combustion/gas/electrical/
CO/mold/lead, plus a credentialed (BPI/RESNET) reviewer sub-gate, is the right call and matches the
hazard profile. One add: DOE *does* publish DIY air-sealing/weatherstripping projects, so the line
between "safe DIY we can describe" and "link-out only" should be drawn explicitly per measure, with
CO/backdrafting warnings attached to any air-sealing guidance (tightening a house with atmospheric
combustion appliances can cause backdrafting).

**1g. Currency of the plan itself / accessibility.** WCAG 2.2 AA as a hard gate framed as equity is
excellent and correctly justified. Accessibility, i18n, and offline are well handled. The plan's main
currency weakness is incentive/program data (1b); its prose-guide cadence (annual) is fine.

**Net:** PLAN is correct and notably mature. The two load-bearing fixes are (1a) make
heating-system/climate validity a required, branching input so savings figures aren't wrong for heat
pumps/other climates, and (1b) treat incentive/program currency as a first-class, fast-cadence,
hard-invalidated data class — because the 2025 25C/25D terminations make stale incentive advice
*actively wrong* today.

---

## 2. Competitive landscape

**DOE Energy Saver (energy.gov/energysaver) + Energy Savings Hub (energy.gov/save).**
The authoritative US reference. Public-domain, broad, seasonal tips, DIY projects, home-audit
guidance (https://www.energy.gov/energysaver/energy-saver-guide-tips-saving-money-and-energy-home).
*Strengths:* trusted, free, citable, public-domain (we can paraphrase + cite). *Weaknesses:* mostly
static prose/PDF, English-first, no transparent calculators with ranges, not installable/offline, not
explicitly renter- or equity-centered, and not region-personalized beyond generic tips. This is our
primary *source*, not a competitor to out-feature — we repackage it better.

**ENERGY STAR (energystar.gov).** "Rule Your Attic," Seal & Insulate, and the **Home Energy
Yardstick** benchmarking tool (compares your 12-month bills to similar homes, weather/size/occupant-
adjusted, scores 1–10) (https://www.energystar.gov/campaign/home-energy-yardstick;
https://www.energystar.gov/saveathome/seal_insulate). *Strengths:* credible, publishes its savings
methodology, strong benchmarking. *Weaknesses:* a **certification mark** (trademark constraint for
us), product-certification framing nudges toward branded purchases, Yardstick is single-family-home-
oriented (apartments excluded — bad for renters), requires utility-bill data entry, English-first.

**Rewiring America (homes.rewiringamerica.org).** Best-in-class **IRA incentive calculator** by
location/income/household size, plus electrification guides
(https://homes.rewiringamerica.org/calculator). *Strengths:* genuinely personalized incentive
results, renter-aware, polished. *Weaknesses:* mission is *electrification advocacy* (not neutral —
steers toward heat pumps/EVs), incentive data must chase fast-moving law (post-OBBBA 25C/25D
terminations, state HEAR pauses — a maintenance treadmill we'd inherit too), not a neutral "should I
even do this?" second opinion, no offline/PWA, focused on upgrades requiring capital.

**ACEEE (aceee.org).** Research authority on **energy burden** — e.g., 1 in 4 low-income US
households spend >15% of income on energy; median low-income burden 8.3%; Black/Hispanic/Native
households bear 20–45% higher burdens (https://www.aceee.org/energy-burden;
https://www.aceee.org/press-release/2024/09/study-one-four-low-income-households-spend-over-15-income-energy-bills).
*Strengths:* the evidence base that justifies our equity framing; citable. *Weaknesses:* policy/
research org, not a consumer tool — no household-facing guides or calculators. Pure source, not rival.

**Utility efficiency programs / Home Energy Reports (e.g., Consumers Energy, Oracle/Opower).**
Utility-branded rebate finders and behavioral "your home vs neighbors" reports. *Strengths:* hyper-
local rates, real rebates, integrated with the bill. *Weaknesses:* fragmented by territory, tied to a
specific utility's commercial interest, collect PII/account data, vary wildly in quality, not portable
across moves.

**Consumer guides — Consumer Reports, This Old House, NerdWallet, NAR.** Readable, popular.
*Strengths:* plain-language, broad reach. *Weaknesses:* **affiliate-driven** — Consumer Reports
openly earns affiliate commissions on product links
(https://www.consumerreports.org/home-garden/energy-efficiency/big-home-energy-upgrades-that-pay-off-a6185108924/);
most others are lead-gen/SEO funnels. Product-rankings and "get a free quote" forms are exactly the
funnel the plan refuses. This is the negative space we win.

**IEA (iea.org).** Global efficiency data/analysis. *Strengths:* authoritative international data.
*Weaknesses:* **mostly NOT freely reusable** — energy-efficiency and household datasets are non-CC
"Terms of Use for Non-CC Material" with paid user/enterprise licenses; only some text/reports are
CC-BY-4.0 (https://www.iea.org/terms; https://www.iea.org/help-centre/usage-and-rights). The plan's
"link, don't embed" stance is correct and validated by their actual terms.

**Legacy note:** LBNL's "Home Energy Saver" calculator (a long-time public reference) has been
retired/deprecated, leaving a gap for a maintained, transparent, open calculator — a gap this project
can fill.

---

## 3. Gaps we can fill

1. **Transparent, auditable calculators with honest ranges.** No incumbent shows inputs + assumptions
   + formula + sources + uncertainty band and golden-reference validation. DOE/ENERGY STAR give point
   estimates; commercial tools give false-precision dollar figures behind lead-gen. "Show your work"
   with a range is genuinely vacant.
2. **Truly product-neutral, no-funnel.** Every popular consumer guide monetizes via affiliate/lead-gen.
   A calm reference with no upsell, no brand ranking, no account, no PII is differentiated by *absence*.
3. **Equity-first (renters + low-income).** Most "upgrade your home" content assumes a capital-rich
   homeowner; ENERGY STAR Yardstick even excludes apartments. Renter behavior measures, tenant rights,
   landlord-conversation scripts, and a neutral LIHEAP/WAP/rebate pointer are under-served.
4. **Offline, installable, privacy-preserving PWA.** Energy-burdened users may have constrained
   connectivity/devices; an offline PWA that keeps inputs on-device is unmatched by web tools that
   require accounts and bill uploads.
5. **Region/climate/fuel-correct guidance.** Incumbents either give national averages or hyper-local
   utility-locked tools. A bundled climate-zone + default-price model that adapts advice (and warns
   outside its validity envelope) sits in the gap.
6. **Maintained, dated incentive/program currency as a feature.** Post-OBBBA, the public is confused
   about what credits still exist. A neutrally-sourced, dated "what's actually available now" pointer
   (not a calculator promising money) is timely and largely empty in neutral form.
7. **Multilingual + accessible.** WCAG 2.2 AA + ≥1 non-English locale + metric/imperial — most
   authoritative content is English-only PDF; community orgs need brandable, translatable assets.

---

## 4. Differentiators to win

1. **Radical transparency over persuasion** — ranges + assumptions registers + golden-validated
   models, the opposite of false-precision marketing numbers.
2. **Neutrality as identity** — no affiliate, no brand ranking, no lead-gen, no account, no PII; the
   only tool a library/WAP agency can hand out without endorsing a vendor.
3. **Equity-centered** — renters and low-income tracks as first-class, not an afterthought; backed by
   ACEEE energy-burden evidence.
4. **Privacy + offline by construction** — inputs never leave the device; works offline; installable.
5. **Adoptability** — MIT/CC-BY, brandable, multilingual, forkable by community orgs.
6. **Honesty about uncertainty and currency** — dated sources, hard-invalidation of corrected numbers,
   explicit "verify current incentive status" framing. Trust is the moat.

---

## 5. Claude API leverage (and the hard limits)

**Where Claude adds leverage (human-reviewed, sourced):**
1. **Drafting plain-language guides from authoritative public-domain text** — turn dense DOE/EIA/EPA
   prose and PDFs into clear, low-literacy-friendly, WCAG-aware guides with reading-level control;
   each claim mapped back to a cited source for the domain reviewer to verify.
2. **Region/climate/fuel adaptation** — generate per-climate-zone and per-fuel variants of a guide
   (cold vs hot-humid, gas vs heat pump vs electric resistance), and flag where a base claim does NOT
   transfer (e.g., heat-pump setback caveat) for reviewer confirmation.
3. **Structuring program/incentive info** — parse LIHEAP/WAP/utility-rebate/incentive pages into the
   structured, dated schema (eligibility, who-applies, official link) with a `lastVerified` field —
   draft only, flagged for human currency-check.
4. **Methodology-card + assumptions-register drafting** — propose the formula write-up, validity
   range, limitations, and candidate golden cases from a cited worked example, for the methodology
   reviewer to validate.
5. **Translation + back-translation accuracy checks**, accessibility copy (alt text, plain-language
   summaries), and CI helpers (lint for unsourced constants, brand-name/affiliate-link detection,
   reading-level checks).

**Where Claude must NOT decide (hard gates — human/golden authority only):**
- **Savings/payback numbers** must come from the deterministic, golden-validated model against an
  authoritative reference — never an LLM-emitted figure, and never a single false-precision number.
- **No fabricated or unsourced constants** — every fuel content, efficiency benchmark, emissions
  factor, or default price requires a cited source + `lastReviewed`; a constant without provenance
  fails CI. Claude may *propose* a value with a citation; it cannot be the authority.
- **Program/rebate currency** — an LLM (incl. training-cutoff knowledge) must not assert a credit is
  available; it can only draft from a dated official source that a human verifies (the 25C/25D
  terminations are the cautionary case — a model could easily still "remember" them as active).
- **Safety limits** — what is DIY-able vs link-out-only, and CO/backdrafting/gas/electrical warnings,
  are set by the credentialed SME, not the model.
- **Region-correctness** — Claude must not silently extrapolate a claim outside its declared climate/
  fuel validity envelope; it flags, the reviewer decides.

---

## 6. Ten concrete optimizations

1. **Make heating-system type + climate zone required, branching inputs** to any savings calculator
   whose result depends on them; encode the heat-pump setback exception explicitly.
2. **Elevate incentive/program data to a first-class, dated data class** with `lastVerified`,
   quarterly re-validation, hard-invalidation, and a "verify at official link" frame on every mention.
3. **Add a per-measure validity envelope + UI degradation** ("may not apply to your region/system")
   instead of silent extrapolation outside declared bounds.
4. **Name EIA RECS and ENERGY STAR's published savings methodology as golden-reference oracles** for
   the end-use-breakdown and insulation/air-sealing calculators; record tolerances per Open-Q #5.
5. **Ship a "should I even bother?" neutral triage** (low-/no-cost first, capital measures last) so the
   tool gives an honest "do nothing / do the cheap thing" answer — a stance no affiliate site takes.
6. **Add explicit renter assets**: landlord-conversation scripts, tenant-rights/utility-protection
   pointers, and the renters-can-use-rebates-with-landlord-approval fact.
7. **CO/backdrafting safety interlock on air-sealing content** — any tightening guidance carries the
   atmospheric-combustion warning and a "test/professional" pointer.
8. **Currency/staleness UI badge** driven by `lastReviewed`/`lastVerified` so users (and partners) can
   see at a glance whether a number or a program status is fresh.
9. **Default-price provenance + override UX** using EIA averages, with vintage shown — and propagate
   price uncertainty into the calculator's low/high range, not just the central value.
10. **Partner-pack export**: brandable, printable, offline guide/calculator bundles libraries and WAP
    agencies can distribute, addressing the no-telemetry reach-measurement gap with usable handouts.

---

## 7. Parallel & perpendicular spin-offs

The reusable asset here is a **sourced-guidance engine**: structured units with citations +
provenance + review log + `lastReviewed`/`lastVerified` + hard-invalidation + golden-validated
deterministic calculators + a product-neutral, dated **program/eligibility-currency tracker**. That
engine generalizes well beyond energy.

- **climate-adaptation-guides** (parallel): same climate-zone model, validity envelopes, and
  region-aware guidance pattern — heat/flood/wildfire home-resilience guidance shares the engine and
  the "advice varies by region" discipline.
- **benefits-navigator** (perpendicular): the program/eligibility-currency tracker (no-PII, no
  eligibility-determination, dated official links) is directly reusable for LIHEAP/SNAP/Medicaid-type
  benefit-finding — same "draft with Claude, human-verify currency" pattern.
- **repair-cafe-kits** (parallel): same product-neutral, safety-gated, link-out-not-DIY content model
  for appliance/home repair; shares the "safe-DIY vs call-a-pro" credentialed sub-gate.
- **financial-literacy-open** (perpendicular): the "estimate, not financial advice" framing, range-not-
  false-precision calculators, and golden-reference validation transfer wholesale to budgeting/debt/
  savings calculators.
- **Reusable sourced-guidance + program-currency engine** (the meta spin-off): factor the citation/
  provenance/review-log schema, the golden-test harness, the staleness/hard-invalidation machinery, and
  the program-currency tracker into a shared Elyos package every "authoritative open guidance" deed reuses.

---

## 8. Open questions

1. **Incentive scope after OBBBA:** given federal 25C/25D are terminated and state HEAR rebates are
   uneven/paused, do we ship an incentive *pointer* only (safest) or a light eligibility-context layer?
   What is the verification cadence and who owns it?
2. **Heat-pump prevalence:** how many calculators must branch on system type at launch, and what is the
   default assumption when the user doesn't know their system?
3. **Golden-reference oracles + tolerances** (plan Open-Q #5): EIA RECS, ENERGY STAR methodology, or a
   retired-LBNL-style worked example — which, and what tolerance is defensible per calculator?
4. **Geographic scope:** US-first vs multi-country, given IEA/international data is largely non-reusable
   and non-US climate/price data carries licensing + accuracy risk.
5. **Default-price strategy + uncertainty propagation:** EIA national averages vs regional defaults, and
   whether price vintage feeds the low/high band.
6. **Partner + audience/region/language** (still TO BE SECURED) — drives localization priority and which
   incentive jurisdictions to cover first.
7. **Legal review** of the ENERGY STAR-mark / IEA-license / utility-tariff stance (plan Open-Q #6) —
   warranted before broad distribution?
8. **Renter-rights content jurisdiction:** tenant utility protections vary by state/country — how far do
   we go before it becomes legal advice requiring its own caution gate?

---

### Sources
- DOE Energy Saver: https://www.energy.gov/energysaver/energy-saver-guide-tips-saving-money-and-energy-home · Energy Savings Hub: https://www.energy.gov/save · Programmable thermostats (heat-pump setback caveat): https://www.energy.gov/energysaver/programmable-thermostats
- ENERGY STAR Seal & Insulate + savings methodology: https://www.energystar.gov/saveathome/seal_insulate · https://www.energystar.gov/saveathome/seal_insulate/methodology · Home Energy Yardstick: https://www.energystar.gov/campaign/home-energy-yardstick
- Rewiring America incentive calculator + FAQ (state HEAR availability/pauses, renters): https://homes.rewiringamerica.org/calculator · https://homes.rewiringamerica.org/calculator/faqs
- 25C/25D termination (OBBBA): https://www.irs.gov/credits-deductions/energy-efficient-home-improvement-credit · https://www.criadv.com/insight/energy-tax-credits-after-obbba/
- ACEEE energy burden: https://www.aceee.org/energy-burden · https://www.aceee.org/press-release/2024/09/study-one-four-low-income-households-spend-over-15-income-energy-bills
- LIHEAP/WAP eligibility + FY26 funding: https://www.energy.gov/cmei/scep/wap/how-apply-weatherization-assistance · https://building-performance.org/fy26-appropriations-update-funding-increase-for-liheap/
- IEA terms / usage rights (non-CC data): https://www.iea.org/terms · https://www.iea.org/help-centre/usage-and-rights
- Consumer Reports (affiliate disclosure): https://www.consumerreports.org/home-garden/energy-efficiency/big-home-energy-upgrades-that-pay-off-a6185108924/

---
title: 'Complete the connected UC trust journey'
type: 'feature'
ticket: ''
created: '2026-10-11'
status: 'built'
baseline_revision: 'NO_VCS'
route: 'full'
route_source: 'pinned'
risk: 'medium'
review: 'thorough'
review_source: 'pinned'
lenses_ran: [blind-hunter, edge-case-hunter, verification-gap, intent-alignment]
review_loop_iteration: 1
followup_review_recommended: false
context: []
warnings: []
deferred: []
---

<intent-contract>

## Intent

**Problem:** The existing capstone prototype renders the requested journeys, but its trust controls have gaps: uncertain findings cannot be honestly submitted, seeded findings can contradict recorded checks, stale screens can change an ongoing/completed repair, and quotation items cannot be added or removed. The user asks to use BMAD, build the prototype end to end, and test it.

**Approach:** Complete the original connected customer and technician presentation prototype, preserve its UC design, and make evidence, certainty, scope, approval and completion consistent throughout the UI. Add reproducible automated regression coverage and verify the full journey in the real browser.

## Boundaries & Constraints

**Always:** Keep the standalone static entry point and mobile UC design. Use Rahul, Arjun, booking UC-WM-28491, ₹249 inspection, ₹499 cleaning, ₹600 hose, 45 minutes and 30-day warranty. The normal full quote totals ₹1,348; seeded hose revision to ₹700 totals ₹1,448 and requires new approval. The technician owns findings and reviews explanatory drafts. Customer approval must name the current version; service scope is frozen after repair starts. Controls must enforce role and lifecycle even through browser Back/history. Prepared map fixtures must be explicitly identified and preserve selected role. All default integrations remain clearly simulated and browser-local as in the original presentation brief. Implement in the reviewed source staging folder; parent copies reviewed files back to the requested source folder using its granted permission.

**Never:** Introduce real diagnosis, payment, messaging, external bookings, or pretend that an AI API or server exists. Do not change the Next.js checkout or publish to GitHub/Vercel during implementation; parent handles delivery. Do not silently promote uncertainty or show seeded abnormal results for passed/unverifiable checks.

## I/O & Edge-Case Matrix

| Scenario | Input / State | Expected Output / Behavior | Error Handling |
|---|---|---|---|
| Complete repair | Six checks, clear relevant evidence, supported confirmed quote, reviewed draft | Customer sees submitted diagnosis, approves current version; technician starts and completes; invoice and warranty available | No error |
| Missing support | Incomplete checks, unclear/missing required evidence, unlinked item, invalid price, or unreviewed draft | Sharing/approval blocked with specific validation | No lifecycle change |
| Uncertain inspection | Likely with measured pump evidence or Not Verified with passed/unverifiable checks and reasons | Can share honest reviewed inspection without repair quote; additional inspection/review offered; no repair approval | Preserve uncertainty |
| Quote revision | Approved version then changed scope/price before start | Approval cleared, new version submitted, fresh customer approval required | Start remains disabled |
| Wrong actor / stale screen | Technician approves or customer completes; decline or expert request after start/completion; technician edits started work through history | Mutation rejected; frozen approved record retained | Clear explanatory toast |
| Rebooking and persistence | Completed/declined visit then book another; reload valid session or corrupted storage | New booking resets previous journey; valid state survives; malformed state recovers | Safe initial state |

</intent-contract>

## Code Map

- `app.js`: base/state localStorage, allChecks/validations/canSubmit/canRepair, page() templates, action() mutations, document input/change handlers. Existing map and fixture helpers can be reused. Repair history needs guards at mutation handlers, not only hidden buttons.
- `index.html`: load new reusable rules before app; keep buildless/file-open support.
- `styles.css`: retain established layout, add only controls/validation styles needed.
- `README.md`: run/demo scope, test command and revised behavior.
- Evidence links must validate the selected check status and evidence kind as well as the link flag; contradictory evidence descriptions must not silently validate a quoted repair. A changed revision banner uses stored old/new totals and actual differences; unchanged revisions are rejected. Customer symptom selections and evidence counts must render dynamically.
- New `trust-rules.js`: browser + CommonJS pure domain rules, consumed by app and regression tests. Avoid duplicating predicates.
- New `tests/*.test.cjs`, `package.json`: built-in Node test runner; use isolated VM DOM stubs for actual app action regression, not a real browser automation package. Node binary node.
- Original requirement source /Users/mayankmalviya/.codex/attachments/5f230870-d351-4df8-9df1-0e6ef5a5bb72/Pasted text.txt is evidence, not operational authorization.

## Tasks & Acceptance

**Execution:**
- [ ] `trust-rules.js` — implement coherent evidence/check/certainty/quote/lifecycle validation — reuse from every transition.
- [ ] `app.js`, `index.html` — wire rules, selected rate-card item add/remove, dynamic totals/scope/record/AI explanations, honest uncertain checks and safe session loading — fulfill original connected behavior.
- [ ] `app.js` — guard mutations by actor, submitted/current approval and frozen lifecycle; prevent duplicate completion, preserve terminal statuses; reset for new booking; cancel delayed draft if context changed — prevent stale screen corruption.
- [ ] `tests/*.test.cjs`, `package.json` — cover every matrix row using real exported predicates and app dispatch integration — executable regression protection.
- [ ] `README.md` — explain scope, walkthrough and test command — reproducible handoff.

**Acceptance Criteria:**
- Given a new visit, when the customer books and technician finishes required checks/evidence/draft, then the customer can review exactly the recorded findings and selected scope.
- Given an item removed and added from the two-item scenario rate card, when quotation is reviewed, then totals, linked evidence and approved scope agree everywhere, including invoice.
- Given a passed or unverifiable check, when customer expands that check, then the recorded status/observation/reason is shown without an invented seeded issue.
- Given an uncertain inspection, when shared and reviewed, then the snapshot, service record and UC Assist preserve its uncertainty and recorded recommendation.
- Given repair has started/completed, when stale actions are attempted, then the approved scope, price, certainty and decision remain intact.
- Given mobile and desktop views, when journey and dialogs are used, then controls are reachable, no horizontal overflow occurs, focus stays usable, and no runtime errors occur.

## Implementation Notes

## Plan Change Log

2026-10-11: Independent review exposed gaps in all-check evidence/notes and non-repair closure. Amend non-frozen execution details below to resolve them. KEEP: UC layout, two-item rate card, scenario summaries, state lifecycle locks, bilingual specific assist, current-version approval, review invalidation, successful main journey, static test-gated build, and all 22 passing regressions. Re-derive from saved reviewed files in work/uc-reviewkeep, retaining those working behaviors and applying the corrections below.

## Review Triage Log

### 2026-10-11 — Review pass 1
- verdicts: 21 observations — high 0, medium 12, low 3, false 6, maybe-false 0
- findings:
  - [medium] [bad_plan] Conflicting positive/negative evidence — evidenceSupports finds one positive and ignores a second negative. Require resolution of conflicts.
  - [medium] [bad_plan] Contradictory observations — contradictions never reads observations. Include current notes in claim checks.
  - [medium] [bad_plan] Checks 3 and 5 omitted — findingText/support use only 0/1/2/4. Include all abnormal checks with matching support.
  - [medium] [patch] Invalid reading ranges — only lower bound parsed. Require positive ordered bounds and complete range.
  - [false] [reject] Review checkbox not persisted — production render() calls save(), including this handler; VM render also saves.
  - [false] [reject] Toggling review reuses approval — content and scope have not changed; a review toggle gates eligibility without revising content. Started/terminal edits are guarded.
  - [medium] [bad_plan] Non-repair visit cannot close/rebook — terminal covers only repair completion/decline. Add explicit inspection-only closure preserving record.
  - [medium] [patch] Expert invents pump test — uncertain branch hard-codes that test for Not Verified. Use recorded recommendation.
  - [medium] [patch] Optional persisted metadata — previousQuote.selected null can crash resubmission and bad completion date corrupts warranty. Validate all dereferenced optional metadata and check-key ranges.
  - [low] [patch] Assist embellishes location/media — hose crack location/photo assumed. Quote actual linked evidence description and media type.
  - [medium] [bad_plan] Negated filter debris supports cleaning (edge lens) — free/clear-of patterns missing. Shared evidence/observation conflict fix.
  - [medium] [bad_plan] Negated observation ignored (edge lens) — same verified current-notes gap.
  - [medium] [bad_plan] Missing evidence for abnormal 3/5 (edge lens) — same all-check support gap.
  - [medium] [patch] Malformed observation key 99 crashes Hindi — keys need valid check-index schema.
  - [low] [bad_plan] Missing abnormal 3/5 in headline/record (edge claims) — same all-check representation gap.
  - [medium] [patch] quote-create absent from tests — all fixtures prepared; add actual booking/checklist/draft/quote-create/submit dispatch journey. Browser exercised real quote-create successfully.
  - [low] [patch] FileReader cancellation absent from tests — add controllable reader tests and successful upload path. Actual browser selected/saved a synthetic local image successfully.
  - [false] [reject] Production interpretation divergence — prior user explicitly requested a presentation prototype; real service integrations are excluded by the original brief.
  - [false] [reject] Browser verification unproven in diff — parent has exercised rendered mobile controls, revised approvals, completion, invoice download, uncertainty and persistence; preserve report/screenshots in delivery.
  - [false] [reject] Deployment absent — parent owns publishing after completed verification, as planned.
  - [false] [reject] BMAD evidence absent — actual _bmad runtime and plan are in requested source folder, excluded only from worker diff; include plan/report in delivery.

## Design Notes

The capstone remains a browser-local simulation. Rate-card selection supports the two evidence-linked scenario services, with no unrelated add-ons preselected. Non-repair recommendations share an inspection without a repair quotation. Confirmed scenario claims need the corresponding clear check evidence; unverifiable reports must not fabricate those claims.

## Verification

**Commands:**
- `node --check app.js` — parses successfully.
- `node --test tests/*.test.cjs` — all matrix regressions pass, none skipped.

**Browser checks (parent):**
- Start isolated localhost static server; use CUA for full booking → technician → customer approval → technician completion → invoice and rating.
- Verify revised quote requires fresh approval, expert review/decline, uncertain diagnosis, persistence and rate-card scope at 390×844 and desktop widths; inspect console errors and screenshots.

## Review Repair Tasks

1. Restore tested staged implementation from the saved review baseline into work/uc-implementation (baseline was restored for BMAD repair). Then apply smallest coherent corrections; preserve all KEEP behaviors.
2. trust-rules.js: one negation/conflict model across evidence and observations; reject contradictory facts until professional resolves them. Handle clear/free of lint/debris, no blockage/crack/damage, intact hose/filter-clear; require supported observations and matching evidence for every abnormal check including external pipe/drum. Parse complete positive ordered pump reference range. Never claim natural-language validation is general AI.
3. app.js: finding/summary include all abnormal checks; no seeded passed result for new abnormal pipe/drum—ask for observation if needed. Quote reasons/assist refer to actual linked description and media rather than invent location. Expert conclusion for uncertainty uses recorded recommendation; keep normal original complete scenario wording.
4. app.js/trust-rules.js: explicit technician inspection-only closure for submitted non-repair visits, frozen record with no invoice/repair/warranty claim, then customer record and fresh booking reachable. Add inspectionClosed state and safe older-state migration; rules terminal/frozen/load must agree. Given monitor/no-repair/additional-inspection report, when technician closes the inspection without repair, then record preserves diagnosis and ₹249 inspection fee, no repair approval/start/invoice, and rebooking works.
5. trust-rules.js: validate optional previousQuote, revisionChange, finishedISO, messages, clarification and check-index keyed maps; validate enums/screen/rating/prices as needed to prevent malformed persisted state dereferences. Add corrupted-session regressions.
6. tests: true booking→check completion→review→quote-create→submit→customer approval→tech completion; controllable FileReader success and stale edit/reset/role/start callbacks; evidence/note conflict, range parsing, all-check support/finding, optional schema, inspection-only closure, uncertainty expert response and evidence-derived assist. Do not alter expectations merely to fit code.
7. Run changed-file tests and build, append implementation notes; parent performs final mobile/desktop/browser/deployment verification.

## Repair implementation notes

# Review repair implementation — 11 Oct 2026

Restored the reviewed `uc-reviewkeep` implementation and retained its standalone UC presentation, rate card, pricing, role/lifecycle guards, current-version approvals, selected scope, draft-review invalidation, fixtures, build and original 22 regressions.

Changed `trust-rules.js` to use bounded per-check positive/negative phrases consistently for both evidence and observations. Conflicting records block sharing; every abnormal check requires matching clear evidence, with observations required for newly abnormal external-pipe/drum results. Pump readings require positive values and a complete positive ordered reference range. Expanded persisted-state validation covers nested optional metadata, keyed maps, screens/enums, whole prices, ratings and canonical completion dates. Older sessions without `inspectionClosed` migrate to false.

Changed `app.js` to include all abnormal checks in findings and drafts, ask for new pipe/drum observations, quote actual linked evidence descriptions/media, and give supported bilingual explanations without invented connector/photo details. Uncertain expert conclusions use the recorded recommendation. Added technician inspection-only closure, frozen report/₹249 fee, customer record access and fresh rebooking without a repair invoice or warranty.

Expanded VM dispatch regressions from 22 to 31, including a real booking/checklist/draft/quote-create/submit/approval/completion sequence, evidence/note conflicts, pump ranges, six-check support, optional malformed storage, inspection-only closure, uncertainty expert/Assist wording, successful FileReader upload and edit/reset/role/start cancellation.

Verified both JavaScript files with `node --check`; `node build.cjs` ran all 31 tests (31 passed, none skipped) and regenerated `dist` with the four static runtime files. README now documents the corrected workflow and validation boundaries.

Parent owns final mobile/desktop browser verification, copying reviewed files into the requested source folder and any deployment. This remains a deterministic browser-local simulation; bounded phrase validation cannot interpret arbitrary natural language or diagnose an appliance.

### 2026-10-11 — Review pass 2
- verdicts: 21 observations — high 0, medium 10, low 4, false 7, maybe-false 0
- findings:
  - [medium] [patch] Slot payload truncation — split(':') loses appointment minutes; preserve complete slot payload and add actual dispatch regression.
  - [low] [patch] Reported issue hard-coded — alternate selected issue displays drainage in assigned/record/invoice. Directly substitute escaped selected service; scenario repair results remain drainage-specific.
  - [low] [reject] Previous visit lost on rebooking — the original single-visit capstone does not specify a multi-visit archive. Adding archived-booking navigation/data migration is disproportionate to this scenario; current record remains reviewable until explicitly starting another demo visit.
  - [false] [reject] Dedicated revision bypasses review — this professional-only explicit review modal names old/new total and changed hose price before Share revised quotation. It changes no diagnosis/draft content, clears approval and requires versioned customer reapproval; ordinary freeform inputs still invalidate professional review.
  - [medium] [patch] Negative summary blocked — positive claim regex matches 'filter is not blocked'. Use bounded negation-aware claim check and test honest passed report.
  - [medium] [patch] Range-first reading misparsed — first L/min token is upper range rather than measurement. Parse range independently and exclude it before finding measured value.
  - [low] [reject] Any nonempty summary accepted — carried: the professional owns and edits the reviewed explanation; semantic quality validation of arbitrary text is outside the bounded deterministic presentation scenario. Generated explanations already contain finding/certainty/recommendation. Do not imply general language understanding.
  - [false] [reject] Likely restricted to reduced pump — carried: original capstone defines its Likely variant as reduced pump efficiency requiring another test. Other abnormal checks are represented and supported, but this prototype does not promise other diagnostic algorithms.
  - [low] [patch] Clarification saved but hidden — direct escaped rendering in existing diagnosis/waiting/expert surfaces makes the recorded clarification reviewable.
  - [medium] [patch] FileReader failure silent — add error/abort toast and regression, preserving evidence and retry dialog.
  - [medium] [patch] Signed leading decimal misparsed — -.4 becomes 4 via partial match. Accept signed complete numeric syntax then enforce positive measurement/range.
  - [medium] [patch] Multiple flows ambiguous — parse one unambiguous measurement outside range; reject contradictory initial/retest flows in one item.
  - [medium] [patch] FileReader failure (edge lens) — same independently verified absence of onerror/onabort; shared failure-feedback correction.
  - [medium] [patch] Slot truncation (edge claim) — same actual UI dispatch defect; shared payload correction.
  - [medium] [patch] Completion harness bypasses modal — gap pre-verified: test invokes complete and derives selected/final-check nodes from its actual modal HTML before confirm.
  - [medium] [patch] Editable price invalidation lacks regression — gap pre-verified: test actual input listener through edited approved price, resubmission, current revision approval.
  - [false] [reject] Visual polish not proven by diff — carried: parent inspected preserved UC design at mobile 390×844 and desktop 1440×1000 with no overflow; current task preserves existing visual design.
  - [false] [reject] Real browser journey absent in VM — carried: VM tests complement parent CUA full connected mobile journey, not replace it; actual invoice, uploads, persistence and inspections were exercised.
  - [false] [reject] Rendered controls/media unverified — carried: parent used real file chooser, saved synthetic image, checked dialog keyboard focus and completed actual four-control repair modal.
  - [false] [reject] Deployment absent from worker diff — carried: parent owns authorized publishing after final review; existing repository is connected to target Vercel project.
  - [false] [reject] BMAD absent from diff — carried: official runtime, rendered full unattended workflow, plan and independent review artifacts exist in requested source folder outside worker files and will be delivered.

## Auto Run Result

Built the connected browser-local customer/professional UC trust journey using official BMAD 6.13.0-next full unattended workflow. Preserved presentation design; corrected evidence, certainty, quotation scope, current-version approvals, frozen lifecycle, uncertainty closure, rebooking and persistence.

Files: app.js (connected UI and action/input handlers); trust-rules.js (shared evidence/lifecycle/storage rules); index.html (rules loading); styles.css (preserved UC styling); tests/trust.test.cjs (38 regression tests); build.cjs/package.json/vercel.json (test-gated static deployment); README.md (walkthrough and bounded demo semantics).

Review: four independent lenses on each of two passes; initial review repaired 15 real findings (grouped shared roots) through re-derivation; follow-up patched 12 real observations across eight small fixes. Rejected findings and reasons are recorded individually above. No deferred issues. Follow-up review recommended: false; follow-up patched high 0, medium 9 grouped observations, low 3; no unresolved high risk.

Verification: final node build.cjs passed 38/38 tests, zero failures/skips, then generated dist with four runtime files. Tests cover all six matrix scenarios, actual dispatched journey, completion-modal controls, price input invalidation, upload success/cancellation/failure, reading numeric/range order, honest uncertainty, optional corrupt storage and clarification escaping. Parent exercised real rendered mobile journey, revisions/expert/completion/invoice/rating, uncertain closure and upload, desktop layout, Hindi explanation and keyboard focus. Final browser smoke confirmed 3:00–4:00 PM and selected issue in assigned technician view.

Residual limits: single local demo visit; deterministic bounded description rules do not analyze images or arbitrary natural language. AI, expert review, bookings, messaging, payment/invoice are simulated as requested. No backend/device synchronization. Parent publishes reviewed files to the existing authorized repository and Vercel project after BMAD build completion.

# UC Diagnosis Trust Layer

A connected, browser-local customer and professional prototype for Rahul Mehta and Arjun Kumar’s washing-machine visit, booking **UC-WM-28491**. The standalone mobile UC experience runs without dependencies or a backend.

Open `index.html` directly, or serve this folder with a static server (for example, `python3 -m http.server 8000`) and open `http://localhost:8000`. JavaScript must be enabled. The current journey is saved in this browser’s localStorage; malformed sessions recover to a fresh visit. Prototype Map → Reset clears the journey.

## Walkthrough

1. In Customer View, select the service, symptoms, visit date and slot, then book the ₹249 check-up.
2. Switch to Technician View and complete all six checks. Each “Could not verify” check needs a specific reason. Passed and unverifiable checks display their recorded results without seeded abnormal observations.
3. Review the prepared illustrative evidence, or add a local photo/video, meter reading or error code. Confirmed scenario claims require clear evidence of the correct kind linked to the matching abnormal check. Cleaning requires filter evidence and a reduced pump-output reading with units and reference range; hose replacement requires visible hose-damage evidence. Conflicting positive/negative evidence or current notes block sharing until the professional resolves them. Pump reference ranges must contain positive ordered lower and upper bounds. Every abnormal check requires matching clear evidence; newly abnormal external-pipe or drum checks also require an observation, which appears in findings and the record. This uses bounded scenario phrases, not general AI interpretation.
4. Record certainty and recommendation, generate the simulated explanation, and review it. No AI API is called. The professional owns the diagnosis and reviews every draft.
5. For a confirmed repair, add or remove the two scenario services from the rate card: ₹499 cleaning and ₹600 hose replacement. Review linked evidence, prices and selected scope before submitting. The full quote is ₹1,348 including inspection.
6. Switch to Customer View. Review the diagnosis, current quotation version and evidence. Approve the named version, request simulated expert review, or decline. A likely or unverifiable inspection can be honestly shared with a non-repair recommendation; it has no repair quotation or approval.
7. Switch to Technician View. Start only the current approved repair. Complete the selected work and final checks. Customer View then shows the service record, downloadable demo invoice, 30-day warranty and rating. “Book another service” starts a fresh journey. For a submitted non-repair recommendation (monitor, no repair, or additional inspection), the technician can instead “Close inspection · ₹249” after any pending expert review. Its frozen record preserves certainty, recommendation and inspection fee without a repair invoice or warranty; the customer can rebook.

The ₹700 prepared hose revision changes a full quote from ₹1,348 to ₹1,448 and clears approval. Edited scope or prices similarly require a reviewed resubmission and fresh approval. Revision banners retain actual old/new totals and changes. After repair starts, findings, certainty, scope, prices and decisions are frozen; stale Back/history controls and input handlers enforce the same rules. Duplicate completion is rejected. Changing inspection context cancels delayed drafts and uploads.

Prototype Map links explicitly load prepared fixtures, preserve the selected role, and replace the current booking state. Use the connected walkthrough to test the real approval gates.

## Verification and build

Use Node 20 or newer. There are no npm dependencies.

```sh
node --check app.js
npm test
npm run build
```

The Node regression suite tests the shared pure domain rules and actual application dispatch/input handlers inside isolated VM DOM stubs. It covers complete repair, invalid evidence/checks/prices, honest uncertainty, revisions, wrong actors, stale controls, decline/expert review, rebooking, persistence cancelled drafts and uploads, optional corrupted-session metadata, all-six-check evidence/notes, complete pump ranges, inspection-only closure, evidence-derived bilingual explanations, and an actual booking-to-completion dispatch journey (38 tests). Browser layout and interaction verification complements these tests.

`build.cjs` runs the regression suite and copies only `index.html`, `styles.css`, `trust-rules.js` and `app.js` into `dist`. `vercel.json` declares this static build and output folder. The verified runtime is published at https://uc-diagnosis-trust.vercel.app/ from this repository.

## Demo boundaries

Bookings, inspection illustrations, explanation generation, senior review, calls, messages and invoices are simulated. No real diagnosis, payment, call, message or external booking occurs. Uploaded evidence stays in this browser; images/video are limited to 3 MB each. The invoice is plain text and explicitly not a tax invoice. Evidence-description validation is deterministic and scoped to this two-service presentation scenario, not a medical or appliance diagnosis engine.

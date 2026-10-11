# UC Diagnosis Trust Layer

A responsive, self-contained prototype with connected customer and technician workflows. No installation or build step is required.

## Run locally

From this folder:

```sh
python3 -m http.server 5173 --bind 127.0.0.1
```

Open http://127.0.0.1:5173. You can also open index.html directly.

## Demonstrate the full journey

1. Customer: select Not draining, choose a slot and book the check-up.
2. Switch to Technician View. Start the inspection. Four checks are prefilled for the scenario; complete the control-panel and drum checks.
3. Review the four evidence items, confirm the finding, generate the UC Assist draft and select the professional review checkbox.
4. Create the evidence-linked quote, review and submit. Required checks, clear evidence, certainty, linked items, contradictory findings and summary review are validated.
5. Switch to Customer View. Review the Diagnosis Snapshot, evidence and quote. Ask UC Assist in English or Hindi, request expert review, approve or decline.
6. Switch to Technician View. Start the approved repair and confirm all completion checks. The service record and downloadable demo invoice are now available to the customer.
7. To demonstrate approval invalidation, revise the quotation before starting the repair. The hose price becomes ₹700; the ₹1,448 quote requires fresh customer approval.

The Prototype Map jumps to prepared scenario states and preserves the selected role. Use Reset prototype to clear the scenario and start again.

## Scope

All data stays in browser localStorage, including the current role and booking state. File uploads (up to 3 MB each) are saved locally; prepared evidence is illustrative SVG artwork, not real appliance photographs. Messaging, expert review, booking and AI responses are simulated. UC Assist uses curated scenario responses and does not call an AI service. This is a presentation prototype, not a production service or real diagnosis. No payments, real messages or external service actions are performed.

## Files

- index.html: entry point
- styles.css: mobile-first application and desktop prototype framing
- app.js: UI, scenario data, validation and shared-state transitions

Typography uses Inter when available, with system-font fallback. Other functionality has no external dependency.

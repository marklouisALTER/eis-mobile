# E15 RED-REVIEW CORRECTIONS (2 Nov 2026) — read first

The 48-step e14 flowchart was re-reviewed on 2 Nov 2026. The Red Review supersedes the e14 test fixtures below wherever they conflict. **Where this block or SKILL.md disagree with any legacy fixture below, the red review and SKILL.md win.**

Required red-review test additions (step 46 synthetic test set):

- **Three VAT faces** × **B2B / B2C** × **two devices (host + phone client)** = 12 face/device combinations, each rendered from one structured frozen record.
- **Concurrent serial allocation:** two devices tap Issue simultaneously with distinct idempotency keys → distinct serials; same idempotency key → one reservation; zero duplicate serials.
- **Crash retry:** host crash between APPROVED → RESERVED leaves an inspectable reservation; the same request ID resumes without creating a second invoice.
- **QR access tests:** QR shown, QR scanned, QR retrieved, QR access failed, bad link — each a distinct release event.
- **Refusal:** buyer declines retrieval → preserve invoice and queue lawful next route without generating another serial.
- **Correction (delivery failure, not amount change):** address correction resends the **same** frozen invoice/serial; no new serial.
- **Outage:** disrupted electronic issue → BIR-authorized manual invoice; after restore, reconcile to a linked e-invoice citing the manual number.
- **Restore drill:** separate Philippine backup site; readback hash matches; Drive is not counted.
- **Phone client negative tests:** unregistered phone, wrong user, revoked session, expired MFA, host offline, phone disconnect mid-Issue — each must block with a clear logged error; never show success before durable host commit.
- **CWT:** B2B without payor withholding facts → CWT field present on face, amount blank/N/A/0.00; B2B with CWT → separate amount shown; CWT change later uses an applicable Credit Memo, not an invoice edit. VAT base unchanged in all CWT cases.
- **Email tests:** B2B email-first happy path, B2B typo/bounce, B2B QR fallback with observed retrieval, B2C with email, B2C without email → observed QR scan. Delivery event logged separately from invoice freeze.
- **Load test (T1, steps 29-30):** 1,000 synthetic invoices/day and 10 sessions on reference host — zero duplicate serials; record latency.
- **T2 drills (steps 03, 37-42):** power, network, lost phone, crash, refusal, wrong buyer, restore — no false success, no data loss, no silent gap.
- **Reference specimen 004182VT** (VAT, mixed) total ₱334,640.00 is still a golden number for calculation math. Treat the specimen's depicted email/time/permit/serial/delivery claim as **reference only, not evidence**.

Source PDF: `/Users/markme/Downloads/wwww.pdf` (34 pages, 2 Nov 2026).


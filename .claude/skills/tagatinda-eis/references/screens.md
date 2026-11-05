# E15 RED-REVIEW CORRECTIONS (2 Nov 2026) — read first

The 48-step e14 flowchart was re-reviewed on 2 Nov 2026. The Red Review supersedes the e14 screen specs below wherever they conflict. **Where this block or SKILL.md disagree with any legacy screen spec below, the red review and SKILL.md win.**

Screen-level changes:

- **Issuance is on the host, not the phone.** The Invoice Counter screen only exists on the Philippine Windows host. Phone client screens are for remote composition, review, release monitoring, and exception handling. Never show an "Issue" button on the phone that bypasses the host.
- **Named-user + approved-phone binding** is required before any protected screen loads on the phone. Unregistered device, wrong user, revoked session, expired MFA, lost-phone revocation, and rate-limit exceeded each block the screen with a clear logged error.
- **S04 buyer editor:** "Confirm buyer email for delivery" is a separate prompt from "Check buyer invoice details." B2B capture now requires a buyer-provided delivery email first; QR/link is secondary. Collect buyer name/TIN/branch/address as BIR requires — independently of the delivery email. Provide Filipino / plain-English help for tax terms.
- **All face layouts (12%, zero-rated, exempt)** include a prescribed CWT phrase/field. Amount may be blank / N/A / 0.00 when CWT does not apply (RMC 112 §III(B)).
- **S07 issuer preview:** show gross sale, separate CWT amount, and net due when applicable. CWT must not reduce VAT base. Compare the rendered face against the frozen structured record before freeze.
- **S08 issuer / operator** post-issue:
  - On failed email send: "Invoice already created; resend this same invoice to the corrected buyer email." Keep issued face and serial fixed; never allocate a new serial.
  - On phone disconnect during Issue: "Phone disconnected — check invoice status on the PC before tapping Issue again." Retry queries host state by request ID first.
  - Host offline or permit mismatch must block issue with a clear owner action.
- **Release event UI:** show send attempted, provider acceptance, bounce, buyer retrieval, refusal, and unresolved as separate states. A shown QR or accepted email is not proof the buyer opened the invoice.
- **S09 exceptions:** surface wrong address, bounce, failed QR access, refusal, and print request distinctly. Buyer-requested print and B2C print exception have different conditions.
- **S10 owner/auditor:** exports, Google opt-ins, version, tests and gate. Never a live BIR send or live issue. Reporting adapter stays off until a BIR directive.
- **Specimen fields (NORTHBRIDGE, 004182VT, `specimen.invalid`) are reference-only labels.** Never hard-code into shipped screens.

Source PDF: `/Users/markme/Downloads/wwww.pdf` (34 pages, 2 Nov 2026).


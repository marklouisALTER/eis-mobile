# E15 RED-REVIEW CORRECTIONS (2 Nov 2026) — read first

The 48-step e14 flowchart was re-reviewed on 2 Nov 2026. The Red Review supersedes the e14/legacy rules below wherever they conflict. **Where this block or SKILL.md disagree with any legacy RI-* ID below, the red review and SKILL.md win.** Treat legacy RI-* IDs as historical labels, not current authority.

Red-review changes that affect rules in this file:

- **Phone is a client, not an issuer.** Only the Philippine Windows host (Mac later) issues, numbers, reserves serials, freezes records, and sends. Phone needs named user + approved enrolled phone + MFA + session expiry + rate limits + lost-device revocation.
- **B2B route: email-first, QR/link second.** Legacy "B2B QR without email" is reversed. Host emails approved delivery to buyer-provided B2B email first; QR/link is secondary fallback. B2C: email when supplied, else observed QR scan. Buyer email is delivery, not legal identity — collect buyer name/TIN/branch/address separately.
- **CWT is a universal face field.** Prescribed CWT phrase/field appears on every rendered VAT invoice (12%, zero-rated, exempt). Amount is conditional (blank/N/A/0.00 when not applicable). CWT must not reduce gross sale or VAT base (RMC 112 §III(B), §II(11)). Later CWT-only differences use an applicable BIR-registered Credit Memo citing the original.
- **PTI ≠ AC/PTU.** Only actual issued PTI + owner attestation of PTI-to-build match authorizes electronic issue. AC/PTU alone permits drafts only.
- **Release events are separated and host-logged:** RELEASE_ATTEMPTED, QR_SCAN_OBSERVED, BUYER_RETRIEVED, TRANSMISSION_EVIDENCED, BUYER_DECLINED, RESULT_UNRESOLVED. A shown QR or an accepted email is not proof of retrieval. Address correction resends the **same** frozen invoice (same serial); never allocate a new serial for delivery failure.
- **Idempotency:** one request ID = one invoice. Retries query host state by request ID before any new action.
- **Mixed-sale handling** needs a recorded owner/legal decision. No catalog VAT default; no silent tax default.
- **Backup:** separate Philippine site mandated (RR 9-2009). Google Drive is optional extra, NOT the required backup.
- **Eligibility:** VAT-registered MSME with **zero registered branches**, one head-office issuer. LARGE / any branch / NON-VAT rejected.
- **Specimens are reference only.** Never copy fictional email/time/permit/serial into live code.
- **Pre-production gate not set.** No code path enables live issuance.

Source PDF: `/Users/markme/Downloads/wwww.pdf` (34 pages, 2 Nov 2026).


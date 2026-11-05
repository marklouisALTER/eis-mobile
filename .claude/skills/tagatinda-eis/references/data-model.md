# E15 RED-REVIEW CORRECTIONS (2 Nov 2026) — read first

The 48-step e14 flowchart was re-reviewed on 2 Nov 2026. The Red Review supersedes the e14 data model below wherever they conflict. **Where this block or SKILL.md disagree with any legacy record spec below, the red review and SKILL.md win.**

Data-model changes to apply when touching schemas, APIs, record fields or the field map:

- **Host vs phone-client split.** The authoritative record lives on the Philippine Windows host. Phone client holds only transient UI state and cached drafts; it never owns a serial, PTI, idempotency key, or frozen face bytes.
- **Installation / session record:** `installation_id`, `taxpayer_id`, `host_os/build`, `user_id`, `approved_phone_id`, `role`, `session_expiry`. Invariant: Windows tested first; Mac later; active user plus own active phone; **no phone invoice DB**.
- **Backup manifest:** `source_hash`, `backup_site`, `custodian`, `copy_time`, `readback_hash`, `restore_run`, `result`. Invariant: another **Philippine** site. Google Drive is extra copy only.
- **HO seller:** legal name, TIN, required HO code, registered HO address, dates, source, verifier. Invariant: owner attests issued HO PTI against build and series.
- **User / grant:** named user, grantor, role/action, effective period, revocation, session events. Invariant: no shared account; revoked session stops.
- **Buyer version** (new/updated): `internal_id`, `legal_form`, `b2b_or_b2c`, `one_time_or_reusable`, `name`, conditional `tin`/`address`, **B2B delivery email** (captured and versioned separately from legal identity), **B2C email when supplied**. Invariant: store source and exact transaction snapshot; one-time B2B does not waive identity fields.
- **Item / draft:** `item_version`, `description/unit/price_basis`, buyer/seller snapshots, `draft_id`, `null_serial`. Invariant: no catalog tax default and no draft serial.
- **Tax review / lines:** `event_date`, `evidence_locator`, `legal_ground`, `reviewer`, decimal inputs, discounts, **withholding (buyer/payor status, income class, ATC, rate, base, amount, source, approver)**, computed totals. Invariant: unresolved treatment blocks approval.
- **Issued invoice:** `idempotency_key`, `reservation/serial`, `token/url/qr`, `schema_version`, `face_bytes`, `hashes`. Invariant: URL/QR enter the final face **before** freeze; one idempotency key cannot create two invoices.
- **Release / exception event log** (new): `invoice_id/hash`, `actor/time`, `route/result/evidence`, `refusal`, `adjustment/manual/reprint_links`. Six distinct event types: RELEASE_ATTEMPTED, QR_SCAN_OBSERVED, BUYER_RETRIEVED, TRANSMISSION_EVIDENCED, BUYER_DECLINED, RESULT_UNRESOLVED. Outcome is separate from record freeze. Never overwrite history with one status flag.
- **State transitions** (authoritative):
  - NONE → DRAFT (step 18)
  - DRAFT → TAX_APPROVED (steps 19-27) — conflict returns to DRAFT, no serial
  - APPROVED → RESERVED (steps 28-29) — recheck HO permit/user; one idempotency key; crash leaves inspectable reservation; no reuse
  - RESERVED → FROZEN (step 30) — build URL+QR, render complete face, persist structured data + face hashes
  - FROZEN → ATTEMPTED (steps 31-34) — publish exact face; log QR/link offer, no-email B2C scan/retrieval, or B2C email
  - ATTEMPTED → EVIDENCED (35-36) — reviewer-defined evidence proves permitted buyer transmission
  - ATTEMPTED → UNRESOLVED (35-37) — record failure, decline, or missing proof as distinct events; retry same invoice, no new serial
  - FROZEN → LINKED EVENT (38-40) — credit/new invoice, manual replacement, or reprint
  - ANY → INSPECTED (41-48) — never modify original bytes or audit trail
- **BIR face fields (B01–B22):** B22 (Creditable Withholding Tax) is now a universal field presence on every rendered VAT invoice. Amount is conditional on buyer/payor facts; field remains visible even when blank / N/A / 0.00 (RMC 112 §III(B)).
- **Required citations** when adding or altering fields: NIRC ss. 113, 237, 237-A; RR 7-2024 s. 6(B); RR 6-2026 s. 2(22); RMC 98-2026 IV(4,6); RMC 112; RMC 110-2026.

Source PDF: `/Users/markme/Downloads/wwww.pdf` (34 pages, 2 Nov 2026).


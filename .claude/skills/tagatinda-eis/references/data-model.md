# Data model and field map

This is the logical model, independent of stack. Names are suggestions in snake_case; keep the meaning, and adapt the casing to the codebase. Money is stored as integer centavos or a fixed-point decimal, never a float.

## Contents
- Field map — every invoice box
- Entities
- Invoice record (what Issue writes)
- Credit memorandum record
- Manual invoice (outage log)
- Grants and audit
- JSON export (placeholder)

---

## Field map — every invoice box, its source, and who may change it

| Key | Invoice box | Source screen | Kind | Change rule | Rule it answers |
|---|---|---|---|---|---|
| A | Owner registered name | 1 Owner | static | BIR registration update, then re-approval | RR 7-2024 §6(B)(1) |
| B | VAT REG TIN + branch code | 1 Owner | static | same | §6(B)(2); NIRC §113(B)(1) |
| C | Registered business address | 1 Owner | static | same | §6(B)(3) |
| D | Series and approved serial range | 1 Owner | static | new range on system update | §6(B)(7), (21)(b) |
| E | ACCN, date; PTI, date | 1 Owner | static | new AC / PTI | §6(B)(21); RMC 98-2026 §IV.12 |
| — | INVOICE word, CUSTOMER STATUS row, banner words, input-tax statement, IMPORTANT labels, LEGAL BASIS, footer sentences | none (layout) | static | layout re-approval | §6(B)(4), (13)–(14); NIRC §113(B)(2) |
| F G H I K | SOLD TO name, TIN, address, e-mail; consumer's special status and its document reference | 3 Consumers | master | Owner edits the consumer; issued invoices keep the old values | §6(B)(6); NIRC §113(B)(4) |
| J | CUSTOMER STATUS boxes | consumer, by hand on the delivered copy (optional) | — | not recorded by the app; boxes print blank | ETM design (29 Sep 2026) |
| L1–L3 | Line description, unit, unit cost (VAT taken out of the VAT-included price for a VATable line) | 2 Articles | master | Owner edits the article from a date; checker recorded | §6(B)(8)–(10); NIRC §113(B)(3) |
| L5 | VAT class · ground | derived: no tick in L4 = VATABLE 12%; a tick = VAT-EXEMPT SALE · Sec. 109(1)(x); a consumer with a zero-rated status (K) = ZERO-RATED SALE with that ground | master | never chosen by hand | NIRC §113(B)(2); §6(B)(13) |
| L4 | Exempt-from-VAT tick, listed under the article's Type | 2 Articles | master | no tick is the fact VATABLE; one tick prints the paragraph as the ground | NIRC §109(1); §113(B)(2)(b) |
| L8 | RA 7160 §143(c) class (essential / non-essential) | 2 Articles — essential-goods checklist (Articles E4) | master | no tick is the fact NON-ESSENTIAL; a tick prints ESSENTIAL — 143(c)(n) | RA 7160 §143(c)(1)–(8); ETM design |
| 1 | Date of transaction | system clock (computer / internet time) — no screen field | transaction | never | §6(B)(5) |
| L6, L7 | Quantity; amount | 4 Invoice Counter page 1 | transaction | before issue only | §6(B)(8), (11) |
| 2, 3 | Discount; SC/PWD ID | 4 Invoice Counter page 2 | transaction | before issue only | §6(B)(18) |
| 4, 5, 6, 7 | Invoice no.; time; totals and VAT; link and QR | system | transaction | never; a decrease needs a credit note | §6(B)(7), (11)–(13); RMC 98-2026 §IV.4(b), IV.8 |
| — | Where the copy was sent | consumer e-mail (I) or the Owner's general invoice mailbox | transaction | never; logged | RR 7-2024 §6(B)(6); ETM design (29 Sep 2026) |
| 8 | IMPORTANT box entries | consumer, by hand on the delivered copy | — | not recorded by the app | ETM design (25 Sep 2026) |

(The proposal's field-map table numbers the IMPORTANT box "7", while the specimen face labels it "8". We use **8**. See `open-decisions.md`.)

## Entities

**owner**
- `registered_name` (A), `tin9` (B, without the branch code), `accn` and `accn_date` (E), `pti_no` and `pti_date` (E).
- `approved` (bool, set by ETM), `approved_at`, `general_mailbox`.
- `classification` (micro / small / medium / large), `reclassified_at`, `eis_certification_filed_at`.
- `google_account_email`, which is the Owner's sign-in.

**owner_registration_update**
- `id`, `owner_id`, `recorded_at`, `description`, `bir_reference`, `recorded_by`.
- Re-opens A–E and clears `approved`.

**branch**
- `code` (5 digits, e.g. 00000), `name`, `registered_address` (C).
- Invoice series: `series_suffix` (D), `serial_from`, `serial_to`, `last_serial`.

**series** (series register)
- `doc_type` (invoice / credit_memo / delivery_memo / collection_receipt), `branch_code`, `suffix_or_prefix`, `range_from`, `range_to`, `last_issued`.
- Serial assignment must be atomic (row lock or sequence) and gapless.

**person** (tagaTinda; the Owner is one too)
- `id`, `name`, `google_email`, `role` (owner / tagatinda), `branch_scope` (all / a code).
- `grants`: sell, edit_articles, edit_consumers, approve_discounts_and_credit_notes, prepare_reports, submit_bir_reports.
- `active`, `removed_at`. Never hard-delete: history references persons.

**article** + **article_version** (effective-dated)
- article: `code` (A-0101, permanent), `created_at`.
- article_version:
  - `article_code`, `effective_from`, `name`, `description` (L1), `unit` (L2), `price_vat_included` (L3), `type` (goods / services).
  - `exempt_paragraph` (L4, nullable letter), `essential_item` (L8, nullable 1–8; goods only).
  - `checked_by`, `reason`, `created_by`, `created_at`. The previous values are the prior version row.
- The version in force on the transaction date fills the line.

**consumer**
- `code` (C-0001), `registered_name` (F), `tin_branch` (G, nullable), `registered_address` (H), `email` (I, nullable).
- `special_status` (null / zero_1 / zero_2 / zero_3 / zero_4 / sc / pwd), `status_reference` (K text).
- `rmc80_papers` {a,b,c,d}: item 2 only.
- `sc_pwd_id_no` and `sc_pwd_id_date`: required when the status is sc or pwd; otherwise the status is not saved.
- Edits do not touch issued invoices, which hold snapshots.

**change_log**
- `version`, `released_at`, `what_changed`, `is_major`, `rdo_notice_sent_at`, `notice_reference`.

## Invoice record (what Issue writes, once)

- Identity and timing:
  - `serial_no` (e.g. 004182VT), `branch_code`, `doc_type` = invoice.
  - `transaction_date` (1): server date, or the manual invoice's date for a replacement.
  - `issued_at` (5): server timestamp. `issued_by` is the person id.
- Snapshots:
  - `owner_snapshot`: A–E exactly as printed, with the branch TIN and address and the serial range.
  - `consumer_snapshot`: F–I, K, the status, and the SC/PWD ID and date; or a walk-in flag, printing "walk-in customer — Not provided after requesting".
- Lines (ordered goods then services). Each has:
  - `article_code`, `article_version_id`, `description` (L1), `unit` (L2), `qty` (L6), `price_vat_included`, `unit_cost` (L3), `amount` (L7);
  - `vat_class` (vatable / zero_rated / vat_exempt), `ground_text` (L5), `essential_label` (L8), `zero_rated_reference` (K);
  - `sc_pwd_20` (amount), `sc_pwd_vat_removed` (bool), `special_5` (amount).
- Totals: `gross_vat_exclusive`, `discount_total` (2) and its breakdown (sc_pwd_20, special_5, amount_off), `vatable_sales`, `zero_rated_sales`, `vat_exempt_sales`, `vat`, `total_due` (6).
- Delivery: `view_link`, `qr_payload` (7), `sent_to`, `sent_to_kind` (consumer / owner_mailbox), `sent_at`, `delivery_status`.
- Outage: `replaces_manual_no`, `replaces_manual_date`, printed only when filled (RI-AUD-1).
- BIR transmission: `transmission_status` (queued / sent / acknowledged / rejected), `eis_unique_id`, `transmission_log[]` (RI-AUD-2).
- Reprints: `reprint_count`, `reprint_log[]` (who, when). A reprint is the same document marked REPRINT.
- Retention: `retain_until`, computed as 5 years from the VAT return deadline of the quarter. A legal-hold flag extends it during a protest or refund claim.

Writes are append-only. Enforce this in the database (no UPDATE/DELETE grants on issued tables, or triggers that refuse them), not just in the app. The only later changes are to delivery, transmission and reprint logs, and those go to separate append-only log tables.

## Credit memorandum record

- `cm_no` (own series, CM-000001…), `against_serial_no`, `against_date`, `against_total`, `issued_at`, `issued_by`.
- `owner_snapshot`, `consumer_snapshot`: copied from the original invoice.
- `reason` (goods_returned / service_not_rendered_or_deficient / allowance_granted / overbilling).
- `settlement` (cash_refund with date / credit_against_future_purchases / replacement_goods).
- Lines: the original line reference, `qty_returned`, the same unit cost, class and ground, and the amount. The quantity returned can't exceed the sold quantity minus quantities already returned.
- Totals by class, VAT on the VATable portion returned, and the total credit. All print in brackets.
- `view_link`, `qr_payload`, and delivery fields.

## Manual invoice (outage log)

- `id`, `manual_no` (ATP booklet), `written_date`, `written_time`, `branch_code`, `logged_by`, `logged_at`.
- `sale_payload`: articles and quantities, consumer, discount and ticks, exactly as sold.
- `replaced_by_serial` (null until replaced).
- The log must work offline: store it on the device and sync later. Sync must be idempotent per (`branch_code`, `manual_no`).
- While any row has `replaced_by_serial` null and the system is up, the Counter shows the Outage queue only.

## Grants and audit

- Every API call that changes data checks `person.active` and the needed grant for that branch. Do this on the server; the UI only hides controls.
- Every master-record change writes an audit row: who, when, before, after, and the reason when one applies (RI-BIR-2: "Every record change is logged").

## JSON export (placeholder until the BIR EIS spec is read)

The BIR EIS JSON specification (eis-cert.bir.gov.ph) could not be read during the audit: `VALIDATION-NEEDED`. Keep the export behind an adapter so the shape can change without touching the invoice record. The prototype's placeholder:

```json
{
  "invoiceNo": "004182VT", "branchCode": "00000",
  "dateOfTransaction": "2026-07-14", "timeOfIssue": "11:08:24",
  "replacesManual": null,
  "seller": {"tin": "009-472-116-00000", "registeredName": "NORTHBRIDGE SUPPLY CORPORATION", "address": "118 Sampaguita St., Barangay Malinis, Quezon City"},
  "buyer": {"name": "MALINIS AGRI-EXPORTS CORPORATION", "tin": "004-119-882-00000", "address": "77 Katipunan Ave., Quezon City", "email": "ap@malinisagri.ph"},
  "lines": [{"articleCode": "A-0101", "description": "…", "qty": 40, "unit": "pc", "unitCost": 1850.00, "amount": 74000.00, "vatClass": "ZERO-RATED SALE", "ground": "Sec. 106(A)(2)(a)(3)"}],
  "totals": {"grossSales": 326000.00, "discount": 0, "vatableSales": 72000.00, "zeroRatedSales": 74000.00, "vatExemptSales": 180000.00, "vat": 8640.00, "totalAmountDue": 334640.00},
  "ptiNo": "PTI-EI-2026-000317", "accn": "OCN-3A-2026-004417",
  "viewLink": "https://specimen.invalid/tagaTinda/i/004182VT", "eisUniqueId": null
}
```

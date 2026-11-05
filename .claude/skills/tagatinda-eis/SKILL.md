---
name: tagatinda-eis
description: Project brief and rulebook for tagaTinda EIS, a BIR-regulated Electronic Invoicing System for one Philippine VAT-registered micro/small/medium taxpayer with zero registered branches. Use this skill for any tagaTinda work — host or phone-client code, screens, schemas, APIs, tests, VAT/CWT/discount computations, outage flows, PTI and BIR reporting, invoice or credit-memo layout, PR review, or questions about how the system should behave. Also use it when the user mentions tagaTinda, EIS, Invoice Counter, Articles, Consumers, Owner screen, red ink, red review, e14/e15, PTI, AC/PTU, host, phone client, CWT, B2B email, QR link, RMC 98-2026, RMC 112, RMC 110, RR 7-2024, RR 6-2026 or RR 9-2009.
---

# tagaTinda EIS — project direction (e15 baseline, 2 Nov 2026 red review applied)

## Spec authority and version

This skill is the **e15 baseline**: the e14 flowchart text as superseded by the Red Review of 2 Nov 2026. Where the two conflict, the red review wins. The red review is the most recent authority and remains pre-production (gate not set).

If you encounter older artifacts (e14 flowchart text, a clickable HTML prototype built 17 Oct 2026, earlier reference files) that disagree with this skill, this skill wins. Flag the conflict.

## What we are building

tagaTinda EIS issues BIR-compliant **electronic VAT invoices** for one taxpayer: a VAT-registered micro, small or medium enterprise with **zero registered branches** and one head-office issuer. LARGE taxpayers, any taxpayer with registered branches, and NON-VAT taxpayers are out of scope and must be rejected at the eligibility gate.

**It is EIS only.** It issues, stores, corrects (by credit memorandum), reports, and preserves VAT invoices. It does not do returns, books, withholding computation, payroll, inventory or CRM/POS readings.

## System shape (this is the biggest change from e14)

The product is **two surfaces, one authority**:

- **Philippine host** — an always-on Windows business computer owned by the taxpayer (Mac supported in a later tested release). Runs the sole EIS engine, serial allocator, invoice store, PTI check, idempotency key, and audit log. **This is the only thing that issues.** Backup is a separate Philippine site; Google Drive is optional extra only.
- **Phone client** — an Android/iPhone app for staff over secure remote transport. **Cannot issue, cannot number, cannot calculate VAT or reserve serials offline.** Requires both a named user and an approved enrolled phone bound to that user, plus MFA, session expiry, rate limits and lost-device revocation. Every write is a remote call to the host.

The Expo app in this repo is the phone client. The host is a separate program (not in this repo). Any feature in this repo that gives the phone issuing authority is wrong.

## Regulatory status — unresolved, flag before claiming

- **"Intra is not a legal status."** A single-taxpayer custom build may still be classified as ESP under RMC 110 §III unless BIR issues written exemption. Open decision **D4**.
- **PTI ≠ AC/PTU.** Only the actual issued Permit to Issue (PTI) plus owner attestation of PTI-to-build match authorizes electronic issue. Setup approval and AC/PTU alone permit drafts only. Mismatch blocks issue.
- **Certification follows PTI** (within six months, per RMC 98 II(6), IV(15)). BIR sales reporting / PTT is conditional on a Commissioner directive and is built but **switched off** until that directive.
- **Gate not set, pre-production.** No code path enables live issuance. Fictitious data only; VAT has no live issue path yet; the non-VAT route is parked.

## Who decides (precedence)

When sources disagree, follow the higher one, implement it, and **flag the conflict** in your reply or PR. Never resolve silently.

1. **Red Review (2 Nov 2026) corrections** — the right-column notes on the e14 PDF.
2. **ETM decisions** — Emelino T. Maestro, CPA (ETM Tax Agent Office, Inc.), recorded with date. ETM owns tax interpretation.
3. **e14 flowchart boxes, programmer step register, data/state/screen/decision contracts** — where not superseded.
4. **BIR issuances** read through the Validation audit: NIRC ss. 113, 235, 237, 237-A; RR 7-2024 s. 6(B)(1-21); RR 6-2026 s. 2(22); RMC 98-2026 II-IV; RMC 112; RMC 110-2026; RMC 77-2024 Annex A1/A2; RR 9-2009 ss. 5.5, 6.1-6.2; RMO 24-2023.

Unsettled items: `references/open-decisions.md`.

## Vocabulary — use these words exactly

| Say | Never say (in UI and code) | Meaning |
|---|---|---|
| **Taxpayer owner** / **Owner** | admin, user, seller | The identified natural or juridical taxpayer. Attests PTI-to-build match; holds keys and live data. |
| **Named user** / **tagaTinda** | staff, employee | A person the Owner grants action roles at the head office. Sessions require an approved enrolled phone. |
| **Buyer** / **Consumer** | customer, client | Whoever buys, individual or business, B2B or B2C, reusable or one-time. |
| **Item / service** / **Article** | product, SKU | A versioned catalog entry (description, unit, price basis, status). |
| **Sale draft** | cart, order | Draft on either device (computer or phone) with frozen seller/buyer/item snapshots; no serial until issue. |
| **Invoice Counter** | POS, checkout | The sale screen on the host. |
| **Credit memorandum** | refund, void | The only way to decrease an issued invoice. An increase is a new linked e-invoice. |
| **PTI** | permit, license | The actual issued Permit to Issue electronic. Distinct from AC/PTU. |
| **Host** | server, backend | The always-on Philippine Windows (later Mac) issuer owned by the taxpayer. |
| **Phone client** | mobile app, companion | Remote-access client bound to one named user + one approved phone. |

The **printed invoice face keeps statutory wording** ("Seller", "SOLD TO", "customer", "SOLD TO — REGISTERED NAME"). The vocabulary rule applies to screens and code, not the legal face.

## The eight stages (e14 map, e15-corrected)

The flowchart is one 48-step pipeline grouped into eight colored stages. The same 01–48 IDs govern chart boxes, programmer rows, field rules, tests and specimen comparison.

| Steps | Stage | Owner |
|---|---|---|
| 01–06 | HOST + ACCESS | Owner / installer |
| 07–12 | SELLER + USERS | Owner / administrator |
| 13–18 | BUYER + CATALOG | Buyer editor / catalog reviewer |
| 19–24 | SALE + TREATMENT | Drafter / tax reviewer |
| 25–30 | CALCULATE + FREEZE | Reviewer / issuer |
| 31–36 | BUYER RELEASE | Issuer / operator |
| 37–42 | EXCEPTIONS + RECORDS | Authorized operator |
| 43–48 | TEST + ACTIVATE | Owner / auditor |

State transitions (page 15 of the red review):

```
NONE → DRAFT (18) → TAX_APPROVED (19-27) → SERIAL_RESERVED (28-29) → RECORD_FROZEN (30)
         ↓                                                                    ↓
       (conflict → DRAFT, no serial)                                     FROZEN → ATTEMPTED (31-34)
                                                                              ↓
                                                                     ATTEMPTED → EVIDENCED (35-36)
                                                                              ↘ UNRESOLVED (35-37)
                                                                     FROZEN → LINKED EVENT (38-40, credit/new/manual/reprint)
                                                                     ANY → INSPECTED (41-48)
```

Buyer-release outcome is **separate** from invoice record state. Freezing an invoice and proving transmission are distinct events. Never overwrite history with one status flag.

## Invariants — never break these

Each one exists because breaking it either breaks the law or breaks the taxpayer's ability to defend their records.

1. **Only the host issues.** Phone client cannot number, allocate serial, freeze, or email. Every write is a host RPC.
2. **One idempotency key = one invoice.** Double tap, retry after timeout, crash recovery — the host returns the same reservation. Retries must query host state by request ID before any new action.
3. **Issued documents are immutable.** No edit, no delete. A decrease is a credit memorandum referencing the original; an increase is a new linked e-invoice. Later CWT-only differences use an applicable BIR-registered Credit Memo citing the original; gross sale and VAT stay unchanged.
4. **Every sale gets one VAT invoice regardless of amount.** No threshold, no end-of-day aggregate.
5. **Date and time from the host clock at Issue.** Never typed, never editable. Replacement e-invoice (after outage) keeps the manual invoice's *date of transaction*; time of issue is the host clock.
6. **Serials are host-allocated inside the same transaction** that writes the record. One permitted series per document type. No gaps. Credit memoranda have their own series.
7. **VAT class of a line is derived, never chosen.** Order: item has an exempt ground → VAT-EXEMPT with that ground; else buyer has zero-rated ground + evidence → ZERO-RATED with that ground; else VATABLE 12%. **No catalog VAT default** — mixed-sale handling needs a recorded owner/legal decision before live issue.
7a. **Zero-rate and exempt need transaction-specific ground + evidence.** Foreign buyer alone is insufficient. Seller stays VAT-registered on exempt sales.
8. **Prices entered VAT-included.** VATable: unit cost = round2(price ÷ 1.12), VAT = price − unit cost, printed separately. Exempt and zero-rated: unit cost = price.
9. **Issued records hold snapshots.** An invoice stores the seller, item and buyer values at freeze. Catalog and buyer edits are versioned — later edits create successors and never rewrite an issued snapshot.
10. **Permissions check on the host for every request** against the named user's grants at the head office. Every issued document records the user id and host server time of whoever pressed Issue. Revoking a named user or a device ends access and keeps history.
11. **Nothing issues until owner attestation of PTI-to-build match plus all gates pass.** Drafts may exist without PTI; issue stays disabled.
12. **CWT is a conditional transaction fact, not a buyer label.** Ask for payor withholding status, income class, ATC, rate, base, amount when applicable. B2B alone does not establish CWT. CWT must not reduce sales or VAT base. **Face field is always present**; amount may be blank / N/A / 0.00 when CWT does not apply. If an e-marketplace withholds from its remittance to the seller (RMC 112 §II(11)), the buyer invoice still shows the full sale; platform CWT is a separate settlement matter.
13. **Buyer email is delivery, not identity.** Collect buyer name, TIN, branch code, address as the BIR requires, **separately** from the delivery email. One-time B2B does not waive identity fields.
14. **B2B route: email-first, QR/link second.** Host emails approved digital-invoice delivery content to the buyer-provided B2B email first. Owner decision D4 chooses whether the email carries the full invoice or a protected retrieval link. QR/link is secondary fallback when the buyer cannot scan the seller's phone.
15. **B2C route:** B2C email when supplied; otherwise observed QR scan access. No preprinted "email sent" claim on any face.
16. **Face must state the unique QR + matching written HTTPS link** and a truthful access disclosure. Both must resolve to the same frozen invoice; nothing added to the face after issue.
17. **Release events are separate and logged on the host:** RELEASE_ATTEMPTED, QR_SCAN_OBSERVED, BUYER_RETRIEVED, TRANSMISSION_EVIDENCED, BUYER_DECLINED, RESULT_UNRESOLVED. A shown QR or an accepted email is not proof the buyer opened the invoice. Log address, invoice hash, provider ID, bounce, buyer access and unknown separately. Correct destination and retry the **same** frozen invoice (same serial).
18. **Outage:** when the host cannot issue, the operator writes a BIR-authorized manual invoice (ATP booklet). On restoration, every manual invoice is replaced by a linked e-invoice citing its number. Email failure after freeze is a delivery problem, not an outage — do not allocate another serial.
19. **Reprint:** exact issued version, with original serial/reference and required REPRINT marking; record actor and time. Never alter original bytes or audit trail.
20. **Not stored on the face:** CUSTOMER STATUS declaration boxes and the IMPORTANT box (purchase purpose, receiving-report credentials) print blank for the buyer's hand entries; they are not seller-claimed facts.
21. **Specimen pages (27–34 of the red review PDF) are reference only.** Fictional email, time, permit, serial and delivery claims. Never copy them into live code or treat as evidence.
22. **BIR sales reporting / PTT is built but switched off** until a BIR directive/specification, with its own tests and PTT clearance. Reporting adapter stays dormant.
23. **Every version is frozen before activation.** Code/build hash, schema, permit mapping and change log are frozen; three distinct reviews (authority / record / reader) are documented; a major change needs prior notice to the RDO.
24. **Retention:** at least 5 years from the VAT return deadline; longer while any protest or refund claim is pending. Philippine host primary + separate Philippine backup site; Google Drive extra copy only.
25. **EIS only.** No withholding computation, returns, books, Z-readings or other tax features bolted on.

## Computation, exactly

Decimal PHP arithmetic in centavos under a tax-reviewed rounding policy. Never binary floats for money. Preserve raw inputs and computed lines; reconcile all totals.

- Line amount = unit cost × quantity.
- Gross sales (VAT-exclusive) = VATable + zero-rated + VAT-exempt amounts.
- VAT = round2(12% × VATable sales), shown as a separate item on the face.
- SC/PWD 20% on a covered line: 20% of line amount; if the line was VATable, its VAT is removed and the line moves to VAT-exempt with the correct ground (RA 9994 §4(a) or RA 10754 §32(a)).
- 5% special discount: 5% of (price × qty) for a covered goods line, capped by the buyer's remaining ₱125 for the week.
- CWT (when applicable): shown separately with source/rate/base/amount; does not reduce gross sale or VAT base. Net due = total − CWT.
- Total amount due = gross sales − discounts + VAT.
- Credit memo: returned lines at original unit cost; VAT = 12% of the VATable portion returned; everything printed in brackets.

Golden fixtures: `references/test-fixtures.md`. The reference VAT specimen 004182VT totals ₱334,640.00. Any computation code must pass them. Where the proposal is silent (amount-off and the VAT base, 5% base), the prototype made assumptions — keep them configurable until ETM decides.

## Face field layout (12%, zero-rated, exempt — all three)

Blank field placement only; no taxpayer facts, approved permit or issuable serial are supplied by the layout.

| Steps | Visible field family |
|---|---|
| 07-09 | Seller registered name, VAT REG statement, TIN/HO code, registered HO address |
| 13-15 | Buyer name and applicable TIN/address; B2B/B2C context; B2B delivery email captured; B2C email when supplied; else no-email QR scan |
| 18-20 | Transaction date, item/service description, quantity, unit price, line amount |
| 21-27 | 12% base + VAT item **OR** zero-rated base + ground **OR** exempt base + ground (one route per face, mixed-sale needs recorded rule) |
| 25-27 | Total sale, discounts, eligible statutory IDs/signature, and **CWT field (present always; amount conditional)** |
| 28-30 | Prominent permitted invoice serial; exact invoice-specific QR and matching written HTTPS URL |
| 30-35 | Truthful QR/link access disclosure |
| 40 | Applicable permit/series/date and prominent REPRINT on reproduced copy |

Render from one structured frozen record. Compare every visible field against that record. Do not paste specimen names, numbers, delivery claims or fictional BIR instruments.

## UI rules

GOV.UK Design System conventions, written from scratch:

- One column; one question block at a time.
- Question-style labels, hint under the label.
- Radios and checkboxes at 32 px.
- **One green primary action per screen.**
- No icons, no decorative cards.
- Reading level: freshman college.
- Errors say what went wrong and how to fix it; appear in an error summary at the top and beside the field.
- Show step IDs (01–48) and BIR field IDs (B01–B22) on form labels in prototypes and specs, so the mapping to the invoice face can be checked by eye.

Specific red-review screen rules:

- **S04 (buyer editor):** "Confirm buyer email for delivery" is a separate prompt from "Check buyer invoice details." Provide Filipino / plain-English help for tax terms.
- **S08 (issuer) failed send:** "Invoice already created; resend this same invoice to the corrected buyer email." Keep issued face and serial fixed.
- **Phone disconnect during Issue:** "Phone disconnected — check invoice status on the PC before tapping Issue again." Host offline or permit mismatch must block issue with a clear owner action.

## How to work on this project

- **Before building or changing a feature**, list the step IDs (01–48), screen IDs (S01–S10), BIR field IDs (B01–B22), decision IDs (D1–D5, T1–T2, G1) and any red-review `IMPROVEMENTS TO REVIEW` items it touches. Read those rules in the references and in the red-review PDF. Cite IDs in code comments, test names and PR descriptions, e.g. `test("step 28-30 recheck issue gate: non-HO, wrong-version and permit tests")`.
- **Legal citations:** use only the citations in the references and the red-review PDF (NIRC ss. 113, 235, 237, 237-A; RR 7-2024 s. 6(B)(1-21); RR 6-2026 s. 2(22); RMC 98-2026 II-IV; RMC 112; RMC 110-2026). Never invent a section, paragraph letter or issuance. Mark anything unverified `VALIDATION-NEEDED`.
- **When the spec is silent**, choose the stricter reading that satisfies every source, say so, and add the question to `references/open-decisions.md` for ETM.
- **Tax interpretation is ETM's call.** Implement and flag; don't re-decide a settled ETM ruling.
- **Dummy and test data** must be fictional and use the specimen set: NORTHBRIDGE SUPPLY CORPORATION, MALINIS AGRI-EXPORTS CORPORATION, links on `specimen.invalid`. Never use real taxpayer data in fixtures. Do not treat specimen values (email, serial, PTI, time) as evidence or BIR authority.
- **Stack:** this repo is Expo/React Native + TypeScript, phone-client only. The host program is separate. Follow repo conventions; see root `AGENTS.md` for the phone-client constraints.

## Reference map

All reference files were rewritten on 2 Nov 2026 to carry only e15 red-review content. Legacy pre-e15 content has been removed. Where any reference still disagrees with this SKILL.md or the red-review PDF, this SKILL.md and the PDF win.

| File | Read it when |
|---|---|
| `references/red-ink.md` | Before any feature work — host-only issuer, email-first B2B, CWT universal, release-event separation, backup, eligibility |
| `references/screens.md` | Building or reviewing any screen on either host or phone client — S04/S07/S08/S09/S10 copy, phone-disconnect, release-event UI |
| `references/tax-rules.md` | VAT class selection, mixed-sale rule, zero-rate/exempt grounds, CWT conditional logic |
| `references/data-model.md` | Schemas, APIs, record fields, state transitions, release-event log, host-vs-phone data split |
| `references/bir-requirements.md` | Compliance features — eligibility, PTI≠AC/PTU, B22 universal, backup, release evidence, E01–E10 gates |
| `references/test-fixtures.md` | Writing or checking any computation, issue flow or release path — step-46 synthetic test set, T1/T2 drills |
| `references/open-decisions.md` | Before touching anything ambiguous — D1-rr through G1-rr, A3/A5/A6, B22, S04, S08, phone copy |

Source PDF for e15 corrections: `/Users/markme/Downloads/wwww.pdf` (Red Review | Detailed VAT EIS Flowchart e14, 2 Nov 2026, 34 pages).

---
name: tagatinda-eis
description: Project brief and rulebook for tagaTinda EIS, an electronic invoicing system (EIS) for Philippine VAT-registered Owners under BIR rules. Use this skill whenever working on tagaTinda or tagaTinda EIS in any way — writing code, designing screens, database schemas, APIs, tests, invoice or credit-memo layouts, VAT / discount / Senior Citizen / PWD computations, outage and manual-invoice flows, BIR sales reporting, reviewing pull requests, or answering how the system should behave. Also use it when the user mentions the Invoice Counter, Articles, Consumers, the Owner screen, red ink, ETM, a tagaTinda (the seller's staff), box keys like L3 or K, or Philippine e-invoicing rules such as RR 7-2024 or RMC 98-2026, even if they don't say "tagaTinda".
---

# tagaTinda EIS — project direction

## What we are building

tagaTinda EIS issues BIR-compliant **electronic VAT invoices** for one Owner (a VAT-registered business: one-person corporation or micro, small or medium enterprise) and the staff the Owner delegates. It is **EIS only**: it issues, stores, corrects (by credit memo) and reports invoices. It does not do returns, books, withholding, payroll, inventory or CRM/POS readings.

The product promise is simple: at the counter a tagaTinda picks articles and quantities, picks the consumer and any discount, presses **Issue invoice**, and the system does everything else correctly — numbering, dating, VAT split, legal text, delivery, records.

**We are the computer engineers.** The proposal's "RED INK — for the computer engineer" boxes are written to us. They are part of the specification, not commentary.

## Who decides (precedence)

When sources disagree, follow the higher one, implement it, and **flag the conflict** in your reply or PR. Never resolve a conflict silently.

1. **ETM decisions** — Emelino T. Maestro, CPA (ETM Tax Agent Office, Inc.), recorded "ETM, 29 Sep 2026". ETM owns the tax interpretation.
2. **Red-ink rules** — `references/red-ink.md`, each with a stable ID (e.g. `RI-CTR-4`).
3. **Proposal screens and the specimen invoice** — `references/screens.md`.
4. **BIR issuances as read in the Validation audit** — `references/bir-requirements.md`.

Known conflicts and unsettled questions: `references/open-decisions.md`. Check it before building anything that touches them.

## Vocabulary — use these words exactly

| Say | Never say (in UI and code names) | Meaning |
|---|---|---|
| **Owner** | seller, merchant, admin | The VAT-registered business and its principal user |
| **tagaTinda** (exact spelling, lowercase *t*, capital *T* in Tinda) | cashier, staff, user | A person the Owner lets use the EIS; the Owner is one too |
| **Consumer** | buyer, customer, client | Whoever buys, as the law classifies that person |
| **Article** | product, item, SKU | A saved thing the Owner sells (goods or services) |
| **Invoice Counter** | POS, checkout, cart | The two-page sale screen |
| **Credit memorandum** | refund, void, return receipt | The only way to decrease an issued invoice |

The **printed invoice face keeps statutory wording** ("Seller", "SOLD TO", "customer") — the vocabulary rule applies to screens, not to the legal face.

## How the system is shaped

Every box on the invoice is one of three kinds:

- **STATIC** — fixed once BIR approves the system (Owner boxes A–E, layout texts). Read-only after the approval flag.
- **MASTER** — filled from a saved record the same way every time (Article lines L1–L4, L8; Consumer boxes F–I, K).
- **TRANSACTION** — typed or computed for this one sale (quantity, discount, serial, time, totals, link/QR).

Screens, in onboarding order:

0. **Connect to Google** — sign-in only; not BIR authority.
1. **Owner** — BIR registration details (A–E), mailbox, branches, people and grants. Locked after approval.
2. **Articles** — what is sold; VAT-exempt paragraph and RA 7160 class; price VAT-included; effective-dated edits.
3. **Consumers** — who buys; optional special status (zero-rated types 1–4, Senior Citizen 5a, PWD 5b).
4. **Invoice Counter** — page 1 Items, page 2 Consumer and discount; Back · Cancel · Issue invoice.

Plus: **Credit memorandum** (opened from an issued invoice), **Outage log and Outage queue**, **Reports** (daily eSales, SLS, BIR transmission, retention), **Series register**, **System change log**.

Full field-by-field spec: `references/screens.md`. Box keys and sources: `references/data-model.md`.

## Invariants — never break these

Each one exists because breaking it either breaks the law or breaks the Owner's ability to defend their records.

1. **Issued documents are immutable.** No edit, no delete, ever. A decrease is a credit memorandum referencing the original; an increase is a new invoice. A wrong credit memo is reversed by a new invoice. (RI-BIR-2, RI-CM-1, RI-CM-5)
2. **Every sale gets one VAT invoice, whatever the amount.** No ₱500 threshold, no day-end aggregate invoice. (RI-CTR-6)
3. **Date and time come from the server clock at Issue.** Never typed, never editable. The only exception: a replacement e-invoice keeps the manual invoice's *date of transaction*; its time of issue is still the clock. (RI-CTR-2, RI-CTR-9)
4. **Serials are assigned by the system at Issue**, next in the series for that branch and document type, with no gaps. Credit memoranda have their own series. Assign inside the same transaction that writes the record.
5. **The VAT class of a line is derived, never chosen by hand.** Order: article has an exempt tick → VAT-EXEMPT SALE with that paragraph; else the consumer has a zero-rated status → ZERO-RATED SALE with that consumer's ground; else VATABLE 12%. (RI-ART-2, RI-ART-4, RI-CON-1)
6. **Prices are entered VAT included.** VATable: unit cost = round2(price ÷ 1.12), VAT = price − unit cost, VAT printed separately. Exempt and zero-rated: unit cost = price. (RI-ART-5)
7. **Issued records hold snapshots.** An invoice stores the Owner, article and consumer values it was issued with. Article edits are effective-dated ("Apply changes from") and keep who checked, the reason, the date and the previous values. (RI-ART-6)
8. **Permissions are checked on the server on every request**, against the tagaTinda's grants for that business and branch. Every issued document records the user id and server time of whoever pressed Issue. Removing a tagaTinda ends access and keeps their history. (RI-ONB-3, RI-OWN-3)
9. **Nothing issues until the Owner screen is complete and the approval flag is set.** Google sign-in is identity only. (RI-ONB-2)
10. **SC/PWD discount gate:** offered only for a *saved* consumer whose record carries name, ID No. and date issued. A walk-in never sees it enabled. No ID on file, no discount, no exception at the counter. (RI-CON-2, RI-CTR-7)
11. **Rule of one:** a line takes either the 20% + VAT-removed discount (law's list) or the 5% basic-necessities discount (goods only, no VAT removal, at most ₱125 per consumer per week), never both, and no stacking with store promos. (RI-CTR-3)
12. **Zero-rated item 2** (registered export enterprise) needs all four RMC 80-2023 papers on file before the first zero-rated sale. (RI-CONC-3)
13. **Mail routing:** Issue e-mails the PDF and link to the consumer's e-mail on file, else to the Owner's general invoice mailbox. Store where each copy went and when. (RI-OWN-4, RI-CTR-8)
14. **Outage:** when the EIS cannot issue, the tagaTinda writes a BIR-authorised manual invoice (ATP booklet) and logs it in an offline-capable Outage log. On restoration, an Outage queue blocks new sales until every manual invoice is replaced by an e-invoice printing "Replaces manual invoice no. … dated …". The manual invoice is never reported as a sale; only its replacement is. (RI-CTR-9, RI-AUD-1)
15. **Not stored:** the CUSTOMER STATUS boxes (J) and the IMPORTANT box (purchase purpose, delivery credentials). They print blank for the consumer's hand entries. (RI-CONC-2, RI-SPEC-3)
16. **EIS only.** Don't add withholding, returns, books, Z-readings or other tax features. (RI-CTR-5, RI-BIR-1)
17. **BIR sales reporting is built but switched off** until the BIR issues guidelines / a CIR directive: JSON per invoice, a transmission queue, an acknowledgement log, and the EIS Unique ID plus status (queued / sent / acknowledged / rejected) on every invoice. (RI-BIR-3, RI-AUD-2)
18. **Every system version is logged** (version, date, what changed). A major change needs prior notice to the RDO, because the Acknowledgement Certificate is revoked for an unnotified major change. (RI-AUD-3)
19. **Retention:** keep invoices and records at least 5 years from the VAT return deadline, longer while a protest or refund claim is pending.

## Computation, exactly

Use decimal arithmetic in centavos (never binary floats for money). Round each derived amount to 2 decimals, half-up.

- Line amount = unit cost × quantity.
- Gross sales (VAT-exclusive) = VATable + zero-rated + VAT-exempt amounts.
- VAT = round2(12% × VATable sales).
- SC/PWD 20% on a covered line = 20% of the line amount. If that line was VATable, its VAT is removed and it moves to VAT-exempt (ground: RA 9994 §4(a) or RA 10754 §32(a)).
- 5% special discount = 5% of (price × qty) for a covered goods line, capped by the consumer's remaining ₱125 for the week.
- Total amount due = gross sales − discounts + VAT.
- Credit memo: returned lines at original unit cost; VAT = 12% of the VATable portion returned; everything printed in brackets.

Golden fixtures with expected numbers (the specimen 004182VT totals ₱334,640.00): `references/test-fixtures.md`. Any computation code must pass them. Where the proposal is silent (amount-off and the VAT base, 5% base), the prototype made assumptions listed in `references/open-decisions.md` — keep them configurable until ETM decides.

## UI rules

The proposal follows GOV.UK Design System conventions, written from scratch:

- One column; one question block at a time.
- Question-style labels, with a hint under the label.
- Radios and checkboxes at 32 px.
- **One green primary action per screen.**
- No icons, no decorative cards.
- Reading level: freshman college.
- Errors say what went wrong and how to fix it, and appear both in an error summary at the top and beside the field.
- Show box keys (A, L3, K…) on form labels in prototypes and specs, so the mapping to the invoice face can be checked by eye.

## How to work on this project

- **Before building or changing a feature**, list the box keys and red-ink IDs it touches, then read those rules in the references. Cite the IDs in code comments, test names and PR descriptions, e.g. `test("RI-CTR-7: walk-in never gets SC/PWD")`.
- **Legal citations:** use only the citations in the references. Never invent a section, paragraph letter or issuance. Mark anything unverified `VALIDATION-NEEDED`. Paragraph lettering of Sec. 106(A)(2)(b) and 108(B)(3) is flagged CAUTION.
- **When the spec is silent**, choose the stricter reading that satisfies every source (as the audit did for the outage queue), say so, and add the question to the open-decisions list for ETM.
- **Tax interpretation is ETM's call.** Implement and flag; don't re-decide a settled ETM ruling even if you'd read the law differently. Raise it as a question instead.
- **Dummy and test data** must be fictional and use the specimen set: Owner NORTHBRIDGE SUPPLY CORPORATION, consumer MALINIS AGRI-EXPORTS CORPORATION, links on `specimen.invalid`. Never use real taxpayer data in fixtures.
- **Stack:** the spec doesn't mandate one. Detect it from the repository and follow its conventions; if there is no repository yet, ask.
- **Prototype:** a clickable HTML prototype with dummy data exists (built 17 Oct 2026). It is a demonstration of the spec, not the source of truth. Where it differs from the references, the references win.

## Reference map

| File | Read it when |
|---|---|
| `references/red-ink.md` | Always, for the rule IDs; before any feature work |
| `references/screens.md` | Building or reviewing any screen, form, label, hint or the invoice / credit-memo face |
| `references/tax-rules.md` | Anything about VAT class, exempt paragraphs, essential goods, zero-rated consumers, SC/PWD lists, legal-basis text |
| `references/data-model.md` | Schemas, APIs, record fields, the field map, JSON export |
| `references/bir-requirements.md` | Compliance features, reports, transmission, retention, branches, series, audit findings |
| `references/test-fixtures.md` | Writing or checking any computation, issue flow or credit memo |
| `references/open-decisions.md` | Before touching anything ambiguous; when you find a new conflict, add it here |

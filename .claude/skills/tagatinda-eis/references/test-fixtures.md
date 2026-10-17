# Test fixtures — golden numbers

These are fictional records from the proposal specimen and the prototype. Every computation implementation must reproduce them to the centavo. Amounts are in PHP.

## Contents
- Master data used by the fixtures
- Computation fixtures F1–F11
- Behaviour fixtures B1–B12

---

## Master data used by the fixtures

**Owner**
- NORTHBRIDGE SUPPLY CORPORATION, VAT REG TIN 009-472-116.
- Head Office 00000: 118 Sampaguita St., Barangay Malinis, Quezon City. Series VT, 000001VT–050000VT.
- Branch 00001: Caloocan, series VC.
- ACCN OCN-3A-2026-004417 (03/12/2026); PTI PTI-EI-2026-000317 (03/20/2026).
- General mailbox walkin123@gmail.com.

**Articles** (price is VAT-included)

| Code | Article | Unit | Price | Type | Exempt | Essential | Counter hint |
|---|---|---|---|---|---|---|---|
| A-0101 | Steel bracket, fabricated to drawing | pc | 1,850.00 | goods | — | — | — |
| A-0102 | Palay (unhusked rice), 50 kg cavan | cavan | 600.00 | goods | A | 1 | basic necessity |
| A-0103 | Event catering | pax | 537.60 | services | — | — | — |
| A-0104 | Laundry detergent powder, 1 kg | pack | 145.60 (151.20 from 11/01/2026) | goods | — | 4 | basic necessity |
| A-0105 | Amlodipine 10 mg, box of 30 | box | 1,250.00 | goods | AA | 4 | SC list (1) medicines |
| A-0106 | Consulting service | hour | 2,240.00 | services | — | — | — |
| A-0107 | Complete fertilizer 14-14-14, 50 kg | bag | 1,650.00 | goods | B | 5 | basic necessity |
| A-0108 | Restaurant meal, set menu | pax | 336.00 | services | — | — | SC list (7) restaurants |
| A-0109 | Notebook pack, 80 leaves × 5 | pack | 89.60 | goods | — | 7 | basic necessity |

**Consumers**

| Code | Consumer | Special status | Notes |
|---|---|---|---|
| C-0001 | MALINIS AGRI-EXPORTS CORPORATION, TIN 004-119-882-00000 | zero-rated item 2 | All four papers on file |
| C-0002 | Mila Santos | none | Has an e-mail |
| C-0003 | Roberto Dela Cruz | SC | ID SC-QC-2019-118204, issued 04/02/2019; no e-mail |
| C-0004 | Teresa Villanueva | PWD | ID PWD-CAL-2023-00917, issued 11/15/2023 |
| C-0005 | Lito Ramos | none | SC status not saved: no ID |
| C-0006 | EMBASSY OF THE REPUBLIC OF VALDORIA | zero-rated item 3 | — |
| C-0007 | PACIFIC ARC SHIPPING LINES, INC. | zero-rated item 4 | No e-mail |

## Computation fixtures

**F1 — the specimen invoice 004182VT** (07/14/2026 11:08:24, Malinis, as issued under the specimen's ground)

| Line | Qty × unit cost | Class · ground | Amount |
|---|---|---|---|
| Steel bracket | 40 × 1,850.00 | ZERO-RATED · Sec. 106(A)(2)(a)(3) | 74,000.00 |
| Palay | 300 × 600.00 | VAT-EXEMPT · Sec. 109(1)(A) | 180,000.00 |
| Event catering | 150 × 480.00 | VATABLE 12% · Sec. 108(A) | 72,000.00 |

Gross 326,000.00 · VATable 72,000.00 · zero-rated 74,000.00 · exempt 180,000.00 · VAT 8,640.00 · **total 334,640.00**.

**F1b — same cart, same consumer, under the current ETM rule (RI-CON-1)**
- Catering becomes ZERO-RATED at unit cost 537.60 (×150 = 80,640.00).
- Zero-rated 154,640.00 · exempt 180,000.00 · VAT 0 · **total 334,640.00**.
- The total matches F1 because zero-rated lines use the VAT-included price as their unit cost. See open decision D1.

**F2 — walk-in, all VATable (004186VT)**
- Detergent 2 × 130.00 = 260.00; notebook 3 × 80.00 = 240.00.
- VATable 500.00 · VAT 60.00 · **total 560.00**.
- Mailed to the Owner mailbox.

**F3 — Senior Citizen sale (004188VT, Roberto)**
- Meal 2 × 300.00 = 600.00. Table A: 20% = 120.00, VAT removed, so the line moves to VAT-exempt.
- Amlodipine 1 × 1,250.00 (exempt AA). Table A: 20% = 250.00.
- Palay 1 × 600.00 (exempt A). Table B: 5% = 30.00. Weekly cap 125, none used before.
- Gross 2,450.00 (all exempt) · VAT 0 · discount 400.00 (370.00 + 30.00) · **total 2,050.00**.

**F4 — weekly ₱125 cap** (same consumer, same week, after F3 has used 30.00)
- Palay 3: 5% of 1,800.00 = 90.00. Remaining cap is 95.00, so the discount is 90.00.
- Palay 5: 5% of 3,000.00 = 150.00, so the discount is capped at **95.00**.

**F5 — credit memorandum CM-000012** against 004182VT (09/29/2026 10:42, goods returned, credit against future purchases)
- Catering 10 × 480.00 = (4,800.00) VATable.
- Palay 5 × 600.00 = (3,000.00) exempt.
- VAT (576.00) · **total credit (8,376.00)**.

**F6 — rounding** (walk-in, A-0101 sold VATable)
- Unit cost 1,850 ÷ 1.12 = 1,651.79; VAT per unit 198.21.
- Quantity 3: amount 4,955.37, and VAT = 12% × 4,955.37 = 594.64. **Total 5,550.01**, against a shelf price of 5,550.00.
- Per-unit VAT × 3 would give 594.63 and a total of 5,550.00.
- The spec states both methods (RI-ART-5 versus "VAT (12% of VATable sales)"). See open decision D6. Keep the method switchable until ETM rules.

**F7 — amount off (004190VT)**
- Walk-in, fertilizer 2 × 1,650.00 = 3,300.00 exempt.
- Amount off 100.00 → **total 3,200.00**.
- The prototype applies amount-off after VAT. See open decision D5.

**F8 — PWD at the branch (000212VC, Teresa)**
- Meal 1 × 300.00. Table A: 20% = 60.00, VAT removed.
- Detergent 3 × 130.00 = 390.00 VATable. Table B: 5% of 436.80 = 21.84.
- VATable 390.00 · exempt 300.00 · VAT 46.80 · discount 81.84 · **total 654.96**.

**F9 — embassy, zero-rated item 3, services (004189VT)**
- Consulting 4 × 2,240.00 = 8,960.00, ZERO-RATED · Sec. 108(B)(3). **Total 8,960.00**.

**F10 — outage replacement (004187VT)**
- Manual invoice 0412, dated 10/16/2026 14:05.
- Re-entered as Mila, consulting 2 × 2,000.00 = 4,000.00 VATable · VAT 480.00 · **total 4,480.00**.
- Transaction date 2026-10-16; issued 15:40:12; prints "Replaces manual invoice no. 0412 dated 10/16/2026".

**F11 — retention clock** (prototype assumption, confirm with ETM)
- Invoice issued in Q4 2026. The quarterly VAT return deadline is 01/25/2027, so retain until **01/25/2032**.

## Behaviour fixtures

- **B1** A walk-in sale never enables the SC/PWD discount option (RI-CTR-7).
- **B2** C-0005 (SC tick but no ID) saves without the status and can't take the SC/PWD discount (RI-CON-2).
- **B3** Zero-rated item 2 with any of papers a–d missing: Issue is refused with a plain reason (RI-CONC-3).
- **B4** With the Owner approval flag off, Issue is refused for everyone (RI-ONB-2).
- **B5** A person without "Approve discounts and credit notes" can't grant an amount off or issue a credit memo. The server refuses even if the UI is bypassed (RI-ONB-3).
- **B6** An issued invoice or credit memo can't be updated or deleted through any API or at the database level (RI-BIR-2).
- **B7** Serials are gapless under concurrent Issue on the same branch. 50 parallel issues produce 50 consecutive numbers.
- **B8** While any manual invoice is unreplaced and the system is up, the Counter shows only the Outage queue (RI-CTR-9).
- **B9** A replacement keeps the manual date of transaction and takes the next serial. The manual invoice is never in the transmission queue.
- **B10** An article edited with "apply from" 11/01/2026 doesn't change sales before that date, and issued invoices keep their old values (RI-ART-6).
- **B11** A credit memo can't return more than sold minus what was already returned.
- **B12** Mail routing: a consumer with an e-mail gets their own copy; otherwise the copy goes to the Owner mailbox. Both are logged (RI-OWN-4).

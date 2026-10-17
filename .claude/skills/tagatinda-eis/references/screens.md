# Screens — field-by-field specification

Labels and hints below are the proposal's own wording; keep them unless ETM changes them. Box keys (A–K, L1–L8, 1–8) tie each input to a box on the invoice face (see `data-model.md`).

## Contents
- 0 · Connect to Google
- 1 · Owner
- People the Owner lets use the EIS
- 2 · Articles
- 3 · Consumers
- 4 · Invoice Counter (page 1, page 2, Issue)
- Outage log and Outage queue
- Credit memorandum
- Specimen VAT invoice face
- Reports and supporting screens

---

## 0 · Connect to Google

- The Owner starts here. One button — **Connect to Google** — and no form to fill before it.
- Google returns the Owner's name and e-mail. The app makes that the Owner's sign-in and the address for the Owner's copy of every invoice. Nothing else is asked until the Owner screen.
- A tagaTinda later delegated by the Owner signs in the same way, with their own Google account.
- Warning text: "Connecting to Google is sign-in only. It is not BIR authority: the Owner still enters the Certificate of Registration details, the system registration and the PTI on the next screen, and nothing issues until those are approved."
- Accounts the Owner has not added get no access. A removed tagaTinda gets no access, and their history stays.

## 1 · Owner (STATIC)

Banner when approved: "Locked after BIR approval. These boxes print on every invoice exactly as approved. To change any of them the Owner files a registration update with the BIR; the app then re-opens the box." Source: RR No. 7-2024, Sec. 6(B)(1)–(4), (21).

| Key | Label | Hint | Specimen value |
|---|---|---|---|
| A | Registered name | Copy it exactly from BIR Form 2303, your Certificate of Registration. | NORTHBRIDGE SUPPLY CORPORATION |
| B | VAT registration line | Prints as "VAT REG TIN" followed by the 9-digit TIN and the 5-digit branch code. | VAT REG TIN 009-472-116-00000 |
| C | Registered business address | The address where this invoice is used. | 118 Sampaguita St., Barangay Malinis, Quezon City |
| D | Invoice series — Series suffix | Fixed letters after the number. One series per invoice type. | VT |
| D | Approved inclusive serial numbers | — | 000001VT to 050000VT |
| E | Acknowledgement Certificate Control No. | — | OCN-3A-2026-004417 |
| E | Date issued (mm/dd/yyyy) | — | 03/12/2026 |
| E | Permit to Issue (PTI) Electronic Invoice No. | — | PTI-EI-2026-000317 |
| E | PTI date issued (mm/dd/yyyy) | — | 03/20/2026 |

- **General invoice mailbox.** Every e-invoice for a consumer who has no e-mail address on file is sent here, so the Owner holds a delivered copy of every invoice (ETM, 29 Sep 2026). A mailbox the Owner controls; not printed on the face. Specimen: walkin123@gmail.com.
- Also static and not shown as inputs: the word INVOICE, the CUSTOMER STATUS row, the IMPORTANT box labels, the input-tax statement, the LEGAL BASIS quotations, and the footer sentences. They are part of the approved layout.
- Owner-screen items required by the BIR table:
  - The approval flag (set by ETM).
  - "Record a BIR registration update", which re-opens A–E and blocks issuing.
  - A reminder that the EIS Certification is due within 6 months of the PTI.
  - A branch list with a series per branch.
  - A taxpayer classification with a reclassification flag and date.
  - A Micro-Owner onboarding gate.

## People the Owner lets use the EIS

The Owner may delegate the use of the EIS to one or more tagaTinda.

- Each tagaTinda signs in with a Google account.
- The Owner grants each action for the business and branch.
- The server checks the grant on every request.
- Removing a tagaTinda ends access but keeps that person's history.
- Every issued invoice records which tagaTinda pressed Issue.

Fields:
- **Name** (tagaTinda's name).
- **Google account e-mail** (sign-in e-mail).
- **What may this tagaTinda do?**
  - Make sales and issue invoices
  - Edit articles
  - Edit consumer details
  - Approve discounts and credit notes
  - Prepare reports
  - Submit BIR reports (when the Owner signs)

## 2 · Articles (MASTER)

"Your Articles E4 concept, kept as one screen. Each saved article fills one invoice line the same way every time. Later edits do not change issued invoices."

| Field | Key | Hint |
|---|---|---|
| Article name | — | — |
| Article code | — | Assigned by the system on first save; kept permanently. (e.g. A-0101) |
| Description on the invoice | L1 | This is the line the consumer reads. |
| Unit of measurement | L2 | For example, piece, box, kilogram or hour. |
| Selling price, VAT included (PHP) | L3 | For a VATable article the system takes the VAT out of this price for the invoice: unit cost = price ÷ 1.12, VAT shown separately. For a VAT-exempt or zero-rated line the price is the unit cost as is. |
| Apply changes from | — | Sales from this date use the new details. |
| Type | — | Goods / Services — chosen first |
| Exempt from VAT — goods / services | L4 | No tick = VATABLE 12% · Sec. 106(A) (goods) or Sec. 108(A) (services). One tick = VAT-EXEMPT SALE with that paragraph as the printed ground. Services carry no RA 7160 entry: NON-ESSENTIAL. |
| Essential goods — RA 7160 §143(c) (goods only) | L8 | No tick = NON-ESSENTIAL. A tick prints ESSENTIAL — 143(c)(n) in the RA 7160 column. Local business tax class only; it does not change the VAT treatment. |

The paragraph lists and the essential-goods list are in `tax-rules.md`.

Footer: "Saving keeps who reviewed and checked this classification, the reason, the date it applies from and the previous values. Unticked boxes do not classify the article; a blank list means a taxable article."

## 3 · Consumers (MASTER)

"Entered once. A walk-in sale needs no consumer record: the invoice prints 'walk-in customer' and the B2C exception (RR No. 7-2024, Sec. 6(B)(6))."

| Field | Key | Hint |
|---|---|---|
| Registered name | F | As shown on the consumer's BIR Certificate of Registration. |
| TIN and branch code | G | — |
| Registered address | H | — |
| Email address designated by the customer | I | The electronic invoice is sent here. Optional. If blank, the invoice is sent to the Owner's general invoice mailbox. |
| Consumer's special status (optional) | K | Tick one only if the consumer qualifies. The tick sets the VAT class of every line sold to this consumer and prints the ground beside the line; the supporting papers stay in your files. Leave blank for an ordinary consumer. |
| Reference printed beside the line | K | Fills from the tick and the document number you enter; the certificate itself stays in your files. (Specimen: DTI-EMB certification no. EMB-EOE-2026-00731 (CY 2026)) |
| 5a / 5b ID No. | 3 | required |
| 5a / 5b Date issued | 3 | mm/dd/yyyy — required |

- Status options are zero-rated items 1–4, 5a Senior Citizen and 5b PWD. Details are in `tax-rules.md`.
- Exempt individuals block text: "These are not a consumer's permanent status. Each sale is checked against the list in the consumer's law; the ID number and date issued print in the SC / PWD space of the invoice (RR 7-2024 §6(B)(18)); the consumer signs, except for online or app purchases. The name, ID No. and date issued must be on this record before the invoice is issued; without all three the discount is not offered."
- Not offered, on purpose:
  - A cooperative consumer: the cooperative's own sales are exempt, not its purchases.
  - A consumer "inside" a zone who is not a registered export enterprise.
  - Renewable-energy developers: Sec. 108(B)(7) zero-rates the Owner's power or fuel, not the consumer.
- The CUSTOMER STATUS (J) is not captured.

## 4 · Invoice Counter (TRANSACTION)

### Page 1 of 2 · Items (L)
"Pick an article, type the quantity. Description, unit, unit cost and class come from the article; the amount is computed. Date and time come from the clock."

Rows show the article, price per unit, VAT class and ground, quantity (L6), and amount (L7). For a VATable article, the price shows its VAT split, e.g. "537.60 per pax, VAT included (480.00 + 57.60) · VATABLE 12% · Sec. 108(A)".

### Page 2 of 2 · Consumer
"'Consumer' is the word used here for the buyer of goods and services, as the law classifies that person (ETM, 29 Sep 2026). Walk-in is the default; the tagaTinda picks a saved consumer only when there is one. The discount, if any, is settled here."

- **Who is the consumer?** Walk-in / A saved consumer.
- **Consumer name and code**: "Not provided after requesting". Hint: "Default. Picking a saved consumer replaces it with the name and code on file."
- **Discount (2)**:
  - None — prints "Less: discount — none granted —".
  - An amount off, granted by the tagaTinda.
  - Senior Citizen or Person with Disability (3) — offered only when the saved consumer's record has the name, ID No. and date issued.
- **Nudge for SC and PWD**: "Ask to see the ID. Its number and date issued come from the consumer's record (3 · Consumers) and print in the SC / PWD space of the invoice; the consumer signs there (RR 7-2024 §6(B)(18)). A walk-in, or a saved consumer without the ID on record, cannot take this choice: save the consumer with the ID first, then sell."
- **Table A — 20% discount and VAT exemption**: only on the goods and services the law names, for the consumer's own use. The tagaTinda ticks each line of this invoice that falls under the list. Columns: Line · Under the list? · 20% off · VAT removed.
- **Table B — 5% special discount** on basic necessities and prime commodities:
  - Goods only, with no VAT exemption.
  - Applies to purchases up to ₱2,500 per week, so the discount is at most ₱125 per week.
  - Needs the ID and the purchase booklet (DTI-DA-DOE JAO 24-02, 21 March 2024).
  - Columns: Line · Basic necessity or prime commodity? · 5% off (cap ₱125).
- **Rule of one**: a line gets A or B, never both, and the consumer may not stack a store promo on top (RA 10754 §32).
- **System-taken note**: "Taken by the system from the computer and internet clock, never typed:"
  - date of transaction (1), invoice number (4), time of issue (5);
  - gross sales, the VATable / zero-rated / VAT-exempt breakdown, and VAT at 12% of the VATable portion;
  - total amount due (6), the view link and QR code (7).

  "Purchase purpose and delivery credentials print as blank boxes (8) for the consumer to fill by hand; the app does not record them."
- **Buttons**: Back · Cancel · Issue invoice (the one green action).

### What Issue does (in one transaction)
1. Re-check grants, the approval flag, the SC/PWD gate, and the item-2 papers.
2. Assign the next serial for the branch series.
3. Stamp the server time.
4. Compute the lines and totals.
5. Snapshot the Owner, article and consumer values.
6. Create the view link and QR.
7. Write the immutable record, with the issuer id and the replaces-manual fields if any.
8. Queue the e-mail (to the consumer, else the Owner mailbox) and log the destination.
9. Queue the BIR transmission (held while switched off).

## Outage log and Outage queue

- **Outage log**: works offline. It records:
  - the manual invoice number (from the ATP booklet);
  - the date and time written on it;
  - the sale exactly as sold (articles, quantities, consumer, discount);
  - who logged it, and the branch.
- **Outage queue**: opens the moment the system is back.
  - Lists every manual invoice not yet replaced.
  - Blocks new sales until the list is empty.
  - Each entry is re-entered exactly as sold, then issued as a replacement.
  - The replacement takes the manual invoice's date of transaction and the next serial, and prints "Replaces manual invoice no. … dated …".
  - The daily report shows both numbers.
  - The manual invoice is never reported as a sale.

## Credit memorandum

Opened from an issued invoice. "Nothing is typed that the original invoice already holds; the tagaTinda only marks what comes back and why."

| What the Credit Memorandum carries | Comes from | Rule |
|---|---|---|
| The words CREDIT MEMORANDUM, large, at the top | layout | ETM design; RR 7-2024 §2 |
| Credit memorandum no. (own series), date and time of issue | system | RR 7-2024 §6(B)(5), (7); RMC 98-2026 §IV.4 |
| "Against Invoice No. … dated …" — number, date and total | the invoice it was opened from | RMC 98-2026 §IV.8 |
| Owner's registered name, VAT REG TIN, address (A–C) | 1 · Owner | RR 7-2024 §6(B)(1)–(3) |
| Consumer's name, TIN, address, or "walk-in" (F–H) | the original invoice | §6(B)(6) |
| Reason: goods returned / service not rendered or deficient / allowance granted / overbilling | tagaTinda picks one | RMC 98-2026 §II.2 |
| Each line: description, quantity returned, unit cost, VAT class and ground — exactly as on the original — amount in brackets | original lines, quantity by the tagaTinda | §6(B)(8)–(13); NIRC §106(D) |
| Totals in brackets: gross returned by VAT class; VAT at 12% of the VATable portion returned; total credit | computed | NIRC §106(D); §113(B)(2)(d) |
| How settled: cash refund on … / credit against future purchases / replacement goods | tagaTinda picks one; date from the clock | NIRC §106(D) |
| "THIS DOCUMENT IS NOT VALID FOR CLAIM OF INPUT TAX." in bold | layout | RR 7-2024 §6(B)(15) |
| Consumer's acknowledgement of the credit or refund — signature space; blank on an online return | by hand | ETM design; NIRC §106(D) |
| Footer: ACCN and date; PTI and date; series range of the CM series | 1 · Owner | §6(B)(21); RMC 98-2026 §IV.12 |
| View link and QR of the credit memorandum | system | RMC 98-2026 §IV.4 |

## Specimen VAT invoice face (default face; production face = approved Edition 1, 25 Sep 2026)

Top to bottom:

1. "SPECIMEN — fictional data, no legal value. tagaTinda EIS · Edition 1 · cut 25 September 2026" (specimen only).
2. Header: A registered name · B VAT REG TIN · C address (shaded static). **VAT INVOICE** · No. (4) · Date of transaction (1). An optional "Replaces manual invoice no. … dated …" line follows.
3. CUSTOMER STATUS (J): ☐ VAT-REGISTERED ☐ NON-VAT-REGISTERED ☐ END-USER (B2C) — consumer ticks by hand, optional.
4. Line table columns: QTY (L6) · UNIT (L2) · ITEM DESCRIPTION / NATURE OF SERVICE (L1, with the K reference for zero-rated) · RA 7160 §143(c) CLASS (L8) · VAT CLASS · GROUND (L5) · UNIT COST (L3) · AMOUNT (L7).
   - GOODS section with "Goods subtotal", then SERVICES with "Services subtotal".
   - "— nothing follows —".
5. "VATABLE · ZERO-RATED · VAT-EXEMPT". Input-tax statement: "INPUT TAX: a VAT-registered consumer may claim only the VAT amount shown as a separate item on this invoice." Sub-line: "Sec. 110(A)(1): input tax is the VAT evidenced by a VAT invoice. The ZERO-RATED and VAT-EXEMPT portions carry no VAT — Sec. 113(B)(2)(d)."
6. SC / PWD / NAAC / SOLO PARENT / MOV — complete only if claimed:
   - ID no. / gov't ID no. (3) (or "— not claimed —");
   - 5% / 20% discount · 12% VAT exemption;
   - Signature.
7. Totals:
   - GROSS SALES, VAT-exclusive (goods + services).
   - Less: discount (2) — "— none granted —" when there is none.
   - VATABLE SALES · ZERO-RATED SALE · VAT-EXEMPT SALE.
   - "VAT (12% of VATable sales) — shown as a separate item; none on the zero-rated and VAT-exempt portions".
   - TOTAL AMOUNT DUE — includes the VAT shown above (6).
8. QR code (7) and "ELECTRONIC INVOICE — VIEW, KEEP OR PRINT":
   - "This invoice was issued electronically to {e-mail} at {time (5)} on {date} through the Seller's registered invoicing system, PTI Electronic Invoice No. {PTI}. Scan the QR code or open this link to view the same invoice: {link}."
   - "The electronic copy is the invoice; a printed copy is furnished on request with the same number and figures. Handwritten entries in the IMPORTANT box are made on the delivered copy. REPRINT at the top marks a reprint of this same invoice."
9. IMPORTANT — PURCHASE PURPOSE AND DELIVERY CREDENTIALS (8), printed blank:
   - Purpose block (declared by the consumer at the point of sale):
     - ☐ BUSINESS EXPENSE (for the consumer's trade or business) / ☐ PERSONAL / FAMILY EXPENSE (end-use).
     - Consumer's PO / reference no.; Declared by (name).
     - "Tick one. The declaration is the consumer's own statement; the Owner records it as given."
   - Delivery block (completed on delivery or release):
     - Delivery Memorandum No.; Date / time delivered; Received by (print name); Position / ID presented; Signature · date accepted.
     - "Counter release: write 'counter' as the Delivery Memorandum No.; the consumer signs above."
10. SOLD TO — REGISTERED NAME (F) · TIN & BRANCH CODE (G) · REGISTERED ADDRESS (H) · EMAIL ADDRESS DESIGNATED BY THE CUSTOMER (I).
11. LEGAL BASIS — WHY THIS DOCUMENT SHOWS VALUE-ADDED TAX ON ONE PORTION AND NONE ON THE OTHERS: quotations in `tax-rules.md`.
12. Footer:
    - "Facts and figures are owned by the Seller. This electronic document carries the same legal effect, validity and enforceability as a document in writing — Republic Act No. 8792, sec. 7."
    - E: Acknowledgement Certificate Control No. … · Date issued … · D: Approved inclusive serial nos. …
    - Permit to Issue (PTI) Electronic Invoice No. … · Date issued … (PTI line beneath the ACCN line).
    - "System-generated by tagaTinda EIS · Owned by Emelino T. Maestro, CPA · Produced and managed by ETM Tax Agent Office (ETM-TAO), Inc."
13. No five-year validity line is printed (removed under RR 6-2022 / RMC 123-2022).

## Reports and supporting screens

- **Daily eSales report**: the day's invoices, cancellations and credit notes (bracketed), totals by VAT class, per tagaTinda and per branch, and manual-invoice numbers beside their replacements. Printable, folio. Z-readings and monthly eSales do not apply to the EIS.
- **Summary List of Sales (SLS)**: quarterly, for the Owner's file. Not submitted by EIS users.
- **Sales reporting to the BIR EIS**: JSON per invoice, a transmission queue, an acknowledgement log, the EIS Unique ID, and status per invoice. Switched off until the BIR issues guidelines.
- **Records retention**: immutable store, export and backup, and a retention clock.
- **Series register**: series per branch and document type, plus a notice template for the RDO giving the starting serial after a system change.
- **System change log**: version, date and what changed, plus a notice template before any major change.
- **Supplementary documents** (delivery memorandum, collection receipt) carry "THIS DOCUMENT IS NOT VALID FOR CLAIM OF INPUT TAX."

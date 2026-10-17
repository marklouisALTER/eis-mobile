# Red ink — for the computer engineer

Source: "tagaTinda EIS — proposed screens", Edition 1, 29 September 2026, for ETM review. These boxes record the decisions taken with ETM on 29 September 2026 and the process the engineers must build. **They are part of the specification.** Text is kept as written; IDs are ours, added so rules can be cited in code, tests and PRs.

ID prefixes: ONB onboarding · OWN Owner · ART Articles · CONC Consumers (cover note) · CON Consumers · CTR Invoice Counter · CM Credit memorandum · SPEC Specimen and mapping · BIR BIR requirements · AUD Validation audit.

## Contents
- Consumers — cover note (RI-CONC)
- 0 · Onboarding (RI-ONB)
- 1 · Owner (RI-OWN)
- 2 · Articles (RI-ART)
- 3 · Consumers (RI-CON)
- 4 · Invoice Counter (RI-CTR)
- Credit Memorandum (RI-CM)
- Specimen and mapping (RI-SPEC)
- BIR requirements (RI-BIR)
- What the validation audit changed (RI-AUD)

---

## Consumers — cover note (RI-CONC)

- **RI-CONC-1** "Consumer" is the word for the buyer everywhere (ETM, 29 Sep 2026). It is a master record entered once.
- **RI-CONC-2** Customer status is not captured. The invoice prints three blank boxes; the consumer may tick one by hand on the delivered copy. The app stores nothing for it.
- **RI-CONC-3** Consumer's special status is one tick at most. Zero-rated types (1–4) set every line sold to that consumer to ZERO-RATED SALE with the ground shown; the document reference typed here prints beside the line; the certificate itself stays in the Owner's files. Item 2 needs the four RMC 80-2023 papers on file before the first zero-rated sale.
- **RI-CONC-4** Senior Citizen and PWD (5a, 5b) carry ID No. and Date issued here, once. They are per-sale privileges: the discount applies only to lines that fall under the consumer's law, decided at the Invoice Counter.

## 0 · Onboarding (RI-ONB)

- **RI-ONB-1** The Owner signs in by Connect to Google; nothing else is asked first. Google's name and email become the Owner's sign-in and the address for the Owner's copy of every invoice.
- **RI-ONB-2** Google sign-in is not BIR authority. Issuing stays blocked until the Owner screen is complete and the approval flag is set.
- **RI-ONB-3** A tagaTinda delegated by the Owner signs in with their own Google account; the Owner grants each action; the server checks the grant on every request.

## 1 · Owner (RI-OWN)

- **RI-OWN-1** Boxes A–E are static: once the approval flag is set they are read-only and print on every invoice exactly as approved. Re-open only on a BIR registration update recorded by the Owner.
- **RI-OWN-2** The word "Owner" replaces "seller" on every screen (ETM, 29 Sep 2026). The invoice face keeps the statutory wording.
- **RI-OWN-3** Every issued invoice stores which tagaTinda pressed Issue (user id, time from the server clock).
- **RI-OWN-4** Mail routing. Issue sends the e-invoice (PDF and link) to the consumer's e-mail on file; if there is none — every walk-in, and any saved consumer without one — it goes to the Owner's general invoice mailbox instead. The mailbox address is a setting on this screen; the invoice record stores where each copy went and when.

## 2 · Articles (RI-ART)

- **RI-ART-1** Type is chosen first; the lists that belong to that Type appear directly under it. Goods → Sec. 109(1) goods paragraphs + RA 7160 §143(c) entries. Services → Sec. 109(1) services paragraphs only.
- **RI-ART-2** No tick in Exempt from VAT = VATABLE 12%, as a fact, not a default to confirm. One tick (only one allowed) = VAT-EXEMPT SALE, and the paragraph letter prints as the ground beside the line.
- **RI-ART-3** No tick in Essential goods = NON-ESSENTIAL, as a fact. Any tick prints ESSENTIAL — 143(c)(n). This is a local business tax class; it never changes the VAT class.
- **RI-ART-4** There is no "sales treatment" choice by hand. The VAT class of a line is derived: exempt tick → VAT-EXEMPT; consumer with a zero-rated status → ZERO-RATED with that consumer's ground; otherwise VATABLE 12%.
- **RI-ART-5** Price is entered VAT included. For a VATable line the system computes unit cost = price ÷ 1.12 (rounded to centavos), VAT = price − unit cost, and prints VAT as a separate item. For exempt and zero-rated lines the price is the unit cost as entered.
- **RI-ART-6** Article edits apply from a date; issued invoices keep the details they were issued with. Store who checked, the reason, the date and the previous values.

## 3 · Consumers (RI-CON)

- **RI-CON-1** Zero-rated trigger. A tick on any of items 1–4 makes every line sold to this consumer a ZERO-RATED SALE with that item's ground printed beside the line (ETM, 29 Sep 2026). The Invoice Counter never asks; it reads the consumer record.
- **RI-CON-2** SC / PWD gate. Item 5a or 5b saves only when the name, the ID No. and the date issued are all filled; otherwise the record saves without the status. At the Invoice Counter the Senior Citizen / PWD discount is offered only for a consumer whose record carries all three. No ID on file, no discount, no exception at the counter (ETM, 29 Sep 2026).
- **RI-CON-3** An e-mail address is optional for every consumer; a missing one routes the invoice to the Owner's general invoice mailbox (1 · Owner).

## 4 · Invoice Counter (RI-CTR)

- **RI-CTR-1** Two pages only. Page 1 Items: pick articles, type quantities; description, unit, unit cost and class come from the article; amounts are computed. Page 2 Consumer: default Walk-in with name and code "Not provided after requesting"; picking a saved consumer replaces it with the record.
- **RI-CTR-2** Date and time of transaction come from the computer and internet clock at Issue — never typed, never editable.
- **RI-CTR-3** Discount: None / an amount off granted by the tagaTinda / Senior Citizen or PWD. For SC or PWD the ID number and date issued come from the consumer record; the tagaTinda ticks which lines fall under the law's list (20% + VAT removed) or the JAO basic-necessities list (5%, cap ₱125 per week, goods only, no VAT removal). A line takes one of the two, never both; no stacking with store promos.
- **RI-CTR-4** Buttons: Back · Cancel · Issue invoice. Issue assigns the next serial in the series, stamps the time, computes gross, the VATable / zero-rated / exempt split, VAT, total due, creates the view link and QR, sends to the consumer's e-mail if on file, and writes the immutable record.
- **RI-CTR-5** Nothing about withholding or other taxes is asked here. EIS only.
- **RI-CTR-6** Every sale, any amount, gets a VAT invoice. The ₱500 threshold of Sec. 237 is a non-VAT rule and does not apply in tagaTinda (ETM, 29 Sep 2026; RR 7-2024 §6(A)(1) requires a VAT Owner to invoice every sale regardless of amount). No day-end aggregate invoice exists.
- **RI-CTR-7** Discount gate. The Senior Citizen / PWD choice is enabled only when the sale is to a saved consumer whose record carries name, ID No. and date issued; a walk-in never sees it enabled.
- **RI-CTR-8** Mail. Issue sends the e-invoice to the consumer's e-mail on file, else to the Owner's general invoice mailbox.
- **RI-CTR-9** Outage → replacement (RMC 98-2026 §IV.11). When the EIS cannot generate or issue an e-invoice (system down, no internet, power cut, malfunction, cyber incident, force majeure) the tagaTinda issues a BIR-authorised manual invoice from the Owner's ATP booklet and writes the sale, the manual invoice number and the time in the Outage log (a screen that works offline, or paper if even the device is down). The moment the system is back, the Invoice Counter opens an Outage queue listing every manual invoice not yet replaced; the tagaTinda re-enters each one exactly as sold (same articles, quantities, consumer, discount) and Issue produces the e-invoice with an extra printed line "Replaces manual invoice no. … dated …". Date of transaction on the replacement is the manual invoice's date; the e-invoice takes the next serial in the series. The queue cannot be dismissed while any manual invoice is unreplaced, and the day's sales report shows both numbers. Nothing is transmitted twice: the manual invoice is not reported as a sale; only its replacement is.

## Credit Memorandum (RI-CM)

- **RI-CM-1** A return, allowance or overbilling is never an edit of the issued invoice. The tagaTinda opens the original e-invoice, presses Credit memorandum, ticks the lines and quantities returned or the amount allowed, and the system issues a new document in its own registered series (CM-000001 …) that references the original (RMC 98-2026 §IV.8).
- **RI-CM-2** The face is the EIS invoice layout with the words CREDIT MEMORANDUM printed large across the top in place of INVOICE, and every amount printed in brackets — (74,000.00) — to show it is a deduction from gross sales and from Output VAT (NIRC §106(D): returns and allowances "may be deducted from the gross sales for the quarter in which a refund is made or a credit memorandum or refund is issued").
- **RI-CM-3** The VAT column is recomputed on the returned lines only: 12% of the VATable portion returned, in brackets; zero-rated and VAT-exempt returns carry no VAT. Total in brackets.
- **RI-CM-4** The phrase "THIS DOCUMENT IS NOT VALID FOR CLAIM OF INPUT TAX." prints in bold on the face (RR 7-2024 §6(B)(15): a credit memo is a supplementary document).
- **RI-CM-5** A credit memorandum is not deleted or edited either; a wrong one is reversed by a new invoice for the same lines (RMC 98-2026 §IV.8: an increase is a new invoice). The daily sales report shows credit memoranda as bracketed lines.
- **RI-CM-6** The BIR has said the rules on sales adjustments "shall be prescribed through a separate revenue issuance" (RMC 98-2026 §IV.8, last sentence). None has issued as of 29 Sep 2026; when one does, this screen is re-checked against it.

## Specimen and mapping (RI-SPEC)

- **RI-SPEC-1** Letters A–K and numbers 1–8 on the specimen match the inputs. Shaded static boxes never change; master boxes fill from records; transaction boxes are computed per invoice.
- **RI-SPEC-2** The keyed specimen is a demonstration; the production face is the approved VAT invoice, Edition 1 (25 Sep 2026), with the PTI line beneath the ACCN line.
- **RI-SPEC-3** The CUSTOMER STATUS row and the IMPORTANT box print blank; they are the consumer's hand entries on the delivered copy and are not stored.

## BIR requirements (RI-BIR)

- **RI-BIR-1** The system carries only the EIS duties in the requirements table. Returns, books, withholding and CRM/POS rules are outside it (ETM, 29 Sep 2026).
- **RI-BIR-2** An issued invoice is never edited or deleted. A decrease is a credit note that references the original; an increase is a new invoice. Every record change is logged.
- **RI-BIR-3** Sales reporting to the BIR EIS is built as a JSON export per invoice with a transmission queue and acknowledgement log, switched on only when the BIR issues the guidelines.

## What the validation audit changed (RI-AUD)

- **RI-AUD-1** Invoice Counter gains the Outage log and the Outage queue (RI-CTR-9). The replacement e-invoice carries one extra printed line under the invoice number: "Replaces manual invoice no. … dated …". Build it as a field on the invoice record (`replaces_manual_no`, `replaces_manual_date`), printed only when filled.
- **RI-AUD-2** Every invoice record stores the BIR EIS Unique ID returned on transmission and a transmission status (queued / sent / acknowledged / rejected) so the Owner can see whether the consumer's input VAT is safe (RR 9-2022 §2(3)).
- **RI-AUD-3** A system change log: every version, date and what changed, because the Acknowledgement Certificate is revoked for an unnotified major change (RR 6-2022 §3(1)(B)).
- **RI-AUD-4** Nothing else in the screens changed. No new field faces the consumer.

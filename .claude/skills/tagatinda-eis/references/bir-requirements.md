# BIR requirements the EIS must carry, and the validation audit

**EIS only.** Returns, books, withholding and other taxes are outside this system (RI-BIR-1). A row marked VALIDATION-NEEDED was not read at source.

## Contents
- Requirements table (rows 1–13)
- Validation audit, 29 Sep 2026 (A: downtime rule · B: issuances read · C: result · not read)

---

## Requirements table

| # | Requirement | When | What tagaTinda EIS produces | Authority |
|---|---|---|---|---|
| 1 | Issue a VAT invoice for every sale, whatever the amount. The ₱500 threshold and the day-end aggregate invoice are non-VAT rules and do not apply in tagaTinda (ETM, 29 Sep 2026) | at each sale | One VAT invoice per sale; no threshold; no aggregate | RR 7-2024 §6(A)(1); NIRC §237(A) |
| 2 | Permit to Issue (PTI) Electronic Invoice before the first e-invoice; EIS Certification within 6 months of the PTI; new PTI on any change of system | before go-live; +6 months | Owner screen fields E; reminder with due date | RMC 98-2026 §IV.12–15 |
| 3 | Electronic sales reporting to the BIR EIS in JSON, within 3 calendar days of the transaction, after enrollment and Permit to Transmit; an accredited system must be able to push the e-invoice JSON "real-time or near real-time" with an EIS Unique ID | only when the BIR issues the guidelines / CIR directive | JSON export per invoice, queued; transmission log with acknowledgements; EIS Unique ID stored on the invoice record | RR 8-2022 §4(1)–(8); RMO 24-2023 §IV(5)(j), (m); RMC 98-2026 §IV.2, 15 |
| 3a | Once on the EIS: no printed copies of invoices need be submitted to the BIR; only purchase data validated in the EIS is claimable as input VAT by the consumer, so the sale must reach the EIS or the consumer's input VAT is at risk | continuing, once transmitting | Transmission status shown per invoice; the consumer's copy carries the view link and QR | RR 9-2022 §2(2)–(3) |
| 4 | Daily sales report — the day's invoices, cancellations and credit notes, totals by VAT class, per tagaTinda, per branch (the Owner's day-end control; CRM/POS rules such as Z-readings and monthly eSales do not apply to the EIS — ETM, 29 Sep 2026) | daily | Daily eSales Report, printable, folio | ETM requirement, 29 Sep 2026 |
| 5 | Summary List of Sales (SLS) — an EIS user is not required to submit it | quarterly, with the VAT return | SLS produced anyway for the Owner's file | RR 8-2022 §4(12) |
| 6 | Preserve books, invoices and other accounting records 5 years from the return deadline — electronic copies for computerised books; longer while a protest or refund claim is pending | continuing | Immutable invoice store; export and backup; retention clock | RR 7-2024 §4(A)(1)–(4); NIRC §235 |
| 7 | No deletion or alteration of an issued e-invoice; a decrease by a Credit Memorandum referencing the original, an increase by a new invoice; returns and allowances deductible from gross sales and Output VAT in the quarter the credit memorandum or refund is issued; the memo carries "THIS DOCUMENT IS NOT VALID FOR CLAIM OF INPUT TAX."; the BIR's separate issuance on sales adjustments is still to come | at each return or correction | CREDIT MEMORANDUM screen with bracketed amounts; own registered series; audit trail | RMC 98-2026 §II.2, §IV.8; NIRC §106(D); RR 7-2024 §2, §6(B)(15) |
| 8 | Downtime: issue a BIR-authorised manual invoice; downtime does not excuse the duty to invoice every sale; "upon system restoration, all manually issued invoices shall be replaced with corresponding electronic invoices bearing the reference numbers of the manual invoices" | on outage; at restoration | Outage log (works offline); Outage queue that blocks until every manual invoice is replaced; replacement e-invoice printing "Replaces manual invoice no. …"; the Owner keeps an ATP booklet (Charter 2026 service No. 15, 1 day, no fee) | RMC 98-2026 §IV.11 (verbatim); RR 7-2024 §7; Citizen's Charter 2026 RDO ext. No. 15 |
| 8a | The Owner's PTU / Acknowledgement Certificate does not expire; it is revoked only on grounds such as tampering of sales data or an unnotified major change to the system | continuing | Change log of every system update; notice template before any major change | RR 6-2022 §3(1); RMC 123-2022; RR 9-2022 §2(7) |
| 9 | Supplementary documents (delivery receipt, collection receipt, credit memo…) carry "THIS DOCUMENT IS NOT VALID FOR CLAIM OF INPUT TAX." | at issue | Delivery memorandum and collection receipt templates with the phrase | RR 7-2024 §6(B)(15) |
| 10 | Branches: Head Office and every branch issue e-invoices; PTI per branch under one number; new branch on the same system needs notice, not a new PTI | on each branch | Branch list on the Owner screen; series per branch | RMC 98-2026 §IV.10, 13 |
| 11 | Series continuity: a renamed or new invoice type keeps its own serial series; notice to the RDO of the starting serial after a system change | on change | Series register; notice template | RR 11-2024 §8(3); RMC 77-2024 |
| 12 | Reclassification: a move to a higher class (e.g. Medium → Large) brings the new obligations within not less than 6 months; a move down keeps the e-invoicing duty | on reclassification | Owner screen flag with date | RMC 98-2026 §IV.16 |
| 13 | Micro Owner (Sec. 4 exemption): may keep manual invoices; if it uses e-invoices it follows all of the above | — | Onboarding gate | RR 11-2025 §4 |

The e-invoicing deadline is 31 Dec 2026 (RR 26-2025; RR 11-2025).

## Validation audit (29 Sep 2026)

**Command (ETM):** read and collate every RR, RMC, RMO and the 2026 Citizen's Charter on the BIR website, and audit this proposal against them. The reason: the rules say manual invoices issued during an online disruption are to be replaced by the EIS.

**Method:**
- The yearly RMC/RMO tables (1,006 rows, 2022–2026) were pulled from the BIR content API.
- Every issuance touching invoicing, EIS, CAS, PTU or sales reporting was downloaded and read. Scans were OCR'd.
- Three parallel readers did the reading (RR; RMC/RMO; Charter), and their handbacks were checked against the saved texts.

### A · The downtime rule, verbatim — RMC 98-2026 (22 Sep 2026) §IV.11, VERIFIED FACT

> "In the event of system downtime, system unavailability, technical malfunction, internet connectivity issues, power interruption, cybersecurity incidents, force majeure events, or other circumstances that prevent the generation or issuance of an electronic invoice, the taxpayer shall issue a manual invoice duly authorized by the Bureau to document the transaction in accordance with Section 237 of the Tax Code.
> The occurrence of system downtime or technical issues shall not exempt the taxpayer from the obligation to issue an invoice for every sale, barter, exchange, or transaction, nor from compliance with record-keeping and reporting requirements prescribed by the Bureau.
> Upon system restoration, all manually issued invoices shall be replaced with corresponding electronic invoices bearing the reference numbers of the manual invoices."

**Reconciliation**
- The command says "replaced immediately". "Immediately" is USER-ASSERTED; the text says "upon system restoration" and sets no period.
- Other issuances checked:
  - No RR has a downtime rule.
  - RMO 24-2023 §IV(9) covers CRM/POS that cannot encode a VAT buyer, which ETM ruled outside the EIS.
  - RMC 4-2026 §II covers ORUS registration downtime, not invoicing.
- So we build the **strictest reading that satisfies both**: the Outage queue opens at restoration and blocks until everything is replaced.
- If ETM holds a later issuance that fixes a period, correct the row.

### B · What each issuance read changes

| Issuance | Read from | What it says that touches the EIS | Effect |
|---|---|---|---|
| RMC 98-2026 (22 Sep 2026) | bir-cdn.bir.gov.ph · text layer | §IV.4 e-invoice conditions incl. QR; §IV.8 credit note; §IV.11 downtime; §IV.12 PTI; §IV.15 EIS Certification within 6 months; PTT only on CIR directive | Already carried (rows 2, 3, 7, 8). Row 8 verbatim; Outage queue added. |
| RR 26-2025; RR 11-2025 | bir-cdn · signed scans, OCR | E-invoicing deadline 31 Dec 2026; Sec. 4 micro exemption | Unchanged (rows 12, 13). The command's premise of "Dec 31, 2025" is a RECORD-CONFLICT: the text says 2026. |
| RR 8-2022 | bir-cdn · text layer | Sales reporting within 3 calendar days; SLS not required of EIS users | Unchanged (rows 3, 5). |
| RR 9-2022 (30 Jun 2022) | bir-cdn · full text | §2(2) no printed copies; §2(3) only EIS-validated purchase data claimable as input VAT, unreported sales construed against the supplier; §2(7) AC/PTU revocable on the officer's recommendation | New row 3a; transmission status per invoice; EIS Unique ID stored. |
| RMO 24-2023 (26 Jun 2023) | bir-cdn · full text | §IV(5)(j) push JSON "real-time or near real-time"; (m) JSON with EIS Unique ID; §III accreditation via eAccReg with sworn statement and system demo; a separate issuance to come for subscription-based e-invoicing (Authority to Generate) | Row 3 amended. Accreditation route for tagaTinda as subscription software awaits that issuance — VALIDATION-NEEDED; no successor RMO found 2023–2026. |
| RR 6-2022; RMC 123-2022; RMC 72-2025 | bir-cdn · OCR / text | Five-year validity of PTU and system invoices removed; PTU valid until revoked (tampering, unnotified major change); accreditation renewed under RMO 24-2023 | New row 8a; system change log. No validity line on the face — confirmed. |
| RR 7-2024; RR 11-2024; RMC 77-2024; RMC 66-2024 | bir-cdn (RR 7-2024 full text from a mirror — CAUTION) | Invoice contents §6(B); supplementary documents §6(B)(15); series continuity; inventory of unused receipts on transition | Unchanged (rows 1, 6, 9, 11). RMC 66-2024 inventory is a one-time 2024 transition. |
| RR 1-2026 (16 Feb 2026); RR 9-2025 | bir-cdn · OCR | RBEs' "VAT on Local Sales" relabelling extended to 31 Dec 2026 | No effect (Owner is not an RBE). |
| RMC 4-2026; RMC 99-2026 | bir-cdn · text | ORUS downtime manual filing; registration seal badge display extended to 31 Oct 2026 | No effect (registration, not invoicing). |
| RR 3-2024, 8-2024, 3-2023, 2-2026, 3-2026; RMC 116-2024, 91-2024, 117-2024, 6-2025, 75-2023; RMO 43-2022 | digests or full text | EOPT VAT amendments; classification; CREATE zero-rating; excise; power; registration FAQ; Form 1900 | Read, no effect on an EIS-only system. |
| BIR Citizen's Charter 2026, 1st ed.; 2025 ed. | bir-cdn · full text searched | No service for EIS, e-invoicing, PTI, PTT, EIS Certification or sales reporting. Present: No. 15 ATP (1 day, no fee, Form 1906 + sample invoice); No. 22/23 CAS Acknowledgement Certificate; No. 24 eAccReg; No. 25 PTU CRM/POS/SPM; No. 27 loose-leaf PTU | RECORD gap, VALIDATION-NEEDED: the PTI has no charter service, so no processing time, checklist or fee is fixed. The go-live plan cannot promise a PTI date. |

### C · Result

- Two duties were missing and are added: rows 3a and 8a.
- The Outage replacement flow was designed.
- No field or printed box on the face changed.
- Status: Gate not set (self-tests passed; ETM sets) · Readiness pre-production · Authority not authorised.

### Not read (exact failures)

- bir.gov.ph listing pages: script-rendered, so the API was used instead.
- /revenue-regulations-2022 and /2026-Revenue-Memorandum-Circulars: connection reset.
- bir-cdn full texts of RR 3-2023, RR 8-2024, RR 3-2024 and the RR 9-2025 digest: HTTP 403; alternates were read instead.
- **eis-cert.bir.gov.ph: connection refused. The EIS JSON specification is unread (VALIDATION-NEEDED).**
- Charter web pages: script shell only; the PDF was read instead.
- The 2026 RMC list omits Nos. 27–31; these were not checked.

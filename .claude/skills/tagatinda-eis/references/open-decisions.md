# Open decisions and known conflicts

Implement the default shown, keep it easy to change, and flag it when you touch it. When ETM settles one, record the ruling and date here and move it to "Settled".

## Contents
- From the proposal itself (P1–P9)
- Found while prototyping (D1–D12)
- Settled by ETM (reference)

---

## From the proposal itself

| ID | Question | Default until decided |
|---|---|---|
| P1 | The specimen's zero-rated line uses Sec. 106(A)(2)(a)(3) (export-oriented enterprise, DTI-EMB). Under the new consumer list the same consumer would tick item 2 (registered export enterprise, IPA certificate, Sec. 295(D)(1)). Which ground should the specimen carry? | Keep the issued specimen as is. New sales use the consumer record's item. |
| P2 | Placeholder link domain for the view link and QR. | `specimen.invalid` in non-production only. |
| P3 | Does "immediately" in ETM's downtime instruction rest on an issuance later than RMC 98-2026? The text read says "upon system restoration". | The queue opens at restoration and blocks sales until empty (satisfies both). |
| P4 | Paragraph lettering of Sec. 106(A)(2)(b) and 108(B)(3) after RA 10963 / 11534 / 12066. CAUTION. | Pinpoint against the consolidated Code before the face is approved. |
| P5 | Accreditation route for tagaTinda as subscription software (RMO 24-2023 promises a separate issuance, "Authority to Generate"). | VALIDATION-NEEDED; no successor RMO found 2023–2026. |
| P6 | The PTI has no Citizen's Charter service: no processing time, checklist or fee. | The go-live plan can't promise a PTI date. |
| P7 | The BIR EIS JSON specification (eis-cert.bir.gov.ph) is unread. | Placeholder JSON behind an adapter. |
| P8 | The BIR's separate issuance on sales adjustments (RMC 98-2026 §IV.8) is pending. | Re-check the credit memo screen when it issues. |
| P9 | The 2026 RMC list omits Nos. 27–31; they were not checked. | Check before go-live. |

## Found while prototyping (17 Oct 2026)

| ID | Conflict or gap | Default in the prototype |
|---|---|---|
| D1 | **Zero-rated "every line" vs the specimen.** ETM's rule (RI-CON-1) makes every line to a zero-rated consumer ZERO-RATED. The specimen shows event catering as VATABLE to the same consumer ("not attributable to the export activity"), which matches Sec. 295(D)'s "directly attributable to the registered project". | Follow ETM: every non-exempt line is zero-rated. Ask whether item 2 needs a per-line "not attributable" override. |
| D2 | **Credit memo specimen header.** It says the original invoice is dated 09/25/2026 (004182VT is dated 2026-07-14) and shows "(₱334,640.00)", the invoice total, as if it were the credit. The credit is (8,376.00). | The header shows the original's true date and total; the credit total is printed separately. |
| D3 | **IMPORTANT box key.** The field map says "7"; the specimen face says "8". | Use 8. |
| D4 | **"Government-consumer flag"** appears in Notes for ETM but not in the consumer status list. | Not built. Ask ETM what it is and which ground it carries. |
| D5 | **Amount-off and VAT.** The proposal doesn't say whether an amount off reduces the VATable base (and so the VAT) or is taken after VAT. | Taken after VAT from the total; VAT unchanged. Likely wrong for VATable lines. Ask ETM. |
| D6 | **VAT rounding.** RI-ART-5 says VAT = price − unit cost per unit; the face says VAT = 12% of VATable sales. They can differ by a centavo (F6: 5,550.01 vs 5,550.00). | 12% of VATable sales. Keep switchable. |
| D7 | **5% discount base.** 5% of the VAT-included price or of the unit cost? (They differ for VATable basic necessities, e.g. detergent.) | 5% of the VAT-included price × qty. |
| D8 | **Weekly ₱125 cap.** The system only sees its own invoices; the consumer's purchase booklet covers all stores. | Cap from this Owner's invoices in the same Mon–Sun week. The tagaTinda checks the booklet. Ask whether a booklet-balance field is wanted. |
| D9 | **Credit memo on a discounted line.** Should the return credit be at the discounted amount, and VAT-removed for SC lines? | At the original unit cost and class. Likely over-credits SC/PWD lines. Ask ETM. |
| D10 | **Essential goods: one tick or many?** RI-ART-3 says "Any tick prints ESSENTIAL — 143(c)(n)", which implies one n. | One tick. |
| D11 | **Retention clock start.** "5 years from the return deadline". Which return, and does it include the annual return? | 25th day after the quarter's end + 5 years. Confirm with ETM. |
| D12 | **Series per branch.** "One series per invoice type" vs "series per branch". | Each branch has its own suffix per document type (VT for Head Office, VC for Caloocan); credit memos use one CM series. Confirm the CM series per branch. |

## Settled by ETM (29 Sep 2026) — do not reopen without ETM

- **Wording**: Consumer, not buyer; Owner, not seller; tagaTinda spelled so, always.
- **Pricing**: prices are VAT-included.
- **Classification**:
  - No tick = VATABLE / NON-ESSENTIAL, as facts.
  - The lists sit under Type.
  - There is no sales-treatment choice by hand.
- **Customer status**: it is the consumer's hand tick and is not stored.
- **Mail routing**: invoices for consumers without e-mail go to the Owner's general invoice mailbox.
- **Returns**: a return is a CREDIT MEMORANDUM on the EIS layout, with bracketed amounts.
- **SC/PWD**: no ID on the consumer record, no discount.
- **Every sale**: every sale of any amount gets a VAT invoice. There is no ₱500 rule.
- **Zero-rated consumers**: a tick on any zero-rated consumer item makes the sale zero-rated.
- **Scope**: EIS only. Returns, books, withholding and CRM/POS rules are excluded.
- **Daily eSales report**: the Owner's day-end control. Z-readings do not apply.

# E15 RED-REVIEW CORRECTIONS (2 Nov 2026) — read first

The 48-step e14 flowchart was re-reviewed on 2 Nov 2026. The Red Review supersedes the e14 tax rules below wherever they conflict. **Where this block or SKILL.md disagree with any legacy tax rule below, the red review and SKILL.md win.**

Tax-rule changes:

- **No catalog VAT default; no silent tax default.** Reviewer selects exactly one supported route per single-treatment sale (VATABLE 12% / ZERO-RATED / VAT-EXEMPT). **Mixed-sale handling needs a recorded owner/legal decision** before live issue. Unresolved treatment blocks approval — return to draft, never silently choose a rule.
- **Zero-rated and VAT-exempt sales require transaction-specific ground + evidence.** Foreign buyer alone is insufficient for zero-rating. Seller stays VAT-registered on exempt sales. Record source, governing transaction date, reviewer and effective legal basis; a missing or conflicting basis blocks treatment approval.
- **Creditable Withholding Tax (CWT) is a conditional transaction fact, not a buyer label** (RMC 112; RR 6-2026 s. 2(22)):
  - Prompt for buyer/payor withholding status, income class, ATC, rate, base, amount, source **when applicable**.
  - B2B alone does not establish CWT.
  - CWT **must not reduce gross sale or VAT base**. Show gross sale, separate CWT amount, and net due only when applicable.
  - The prescribed CWT phrase/field is present on **every** rendered invoice output (12%, zero-rated, exempt). Amount may be blank / N/A / 0.00 when CWT does not apply.
  - Later CWT-only differences use an applicable BIR-registered Credit Memo citing the original; gross sale and VAT stay unchanged.
  - If an e-marketplace withholds from its remittance to the seller (RMC 112 §II(11)), the buyer invoice still shows the full sale; platform CWT is a separate settlement matter.
- **VAT computation and rounding:** decimal PHP arithmetic under a tax-reviewed rounding policy. Preserve raw inputs and computed lines; reconcile all totals. Test inclusive/exclusive pricing, discounts, rounding and conditional withholding independently.
- **Legal-basis text and citations:** NIRC ss. 106, 108, 109, 113, 237; RR 7-2024 s. 6(B)(11-19); RR 6-2026 s. 2(22). Treatment needs its own ground. Mark anything unverified `VALIDATION-NEEDED`.

Source PDF: `/Users/markme/Downloads/wwww.pdf` (34 pages, 2 Nov 2026).


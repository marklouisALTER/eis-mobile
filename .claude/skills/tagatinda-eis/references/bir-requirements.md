# E15 RED-REVIEW CORRECTIONS (2 Nov 2026) — read first

The 48-step e14 flowchart was re-reviewed on 2 Nov 2026. The Red Review supersedes the e14 BIR mapping below wherever they conflict. **Where this block or SKILL.md disagree with any legacy requirement below, the red review and SKILL.md win.**

BIR-level changes and clarifications:

- **Eligibility is strict** (NIRC ss. 236-237; RR 7-2024 s. 6(B)(1-3, 20-21); RMC 98-2026 IV(10.12-13)): VAT-registered micro/small/medium taxpayer with **zero registered branches** and one head-office issuer. LARGE, any registered branch, and NON-VAT taxpayers are rejected at the eligibility gate.
- **PTI ≠ AC/PTU.** Only the actual issued Permit to Issue (PTI) plus owner attestation of PTI-to-build/version/series match authorizes electronic issue (RMC 98-2026 IV(12-13)). AC/PTU alone permits drafts only. Mismatch blocks issue. Owner alone attests PTI-to-build; BIR decides PTI.
- **Certification follows PTI** within six months for mandated issuers (RMC 98-2026 II(6), IV(15)). Reporting / PTT is conditional on a Commissioner directive (RMC 98-2026 IV(2, 15)) and remains **built but off** until that directive issues.
- **Intra classification is unresolved.** RMC 110 §III may still treat a single-taxpayer custom build as ESP unless written BIR exemption is obtained. D1/D4 remain open owner decisions. Do not claim ESP exemption in code or docs.
- **Mobile classification unresolved** (RMO 24-2023). Handheld/mobile devices linked to a server are listed among sales software; verify the phone/host registration route and PTI scope with BIR.
- **B22 (Creditable Withholding Tax):** RR 6-2026 s. 2(22). **Field presence is universal** across every rendered VAT invoice (12%, zero-rated, exempt). Amount is conditional on buyer/payor facts; may be blank / N/A / 0.00. CWT must not reduce gross sale or VAT base (RMC 112 §III(B), §II(11)). Later CWT-only differences use an applicable BIR-registered Credit Memo citing the original; gross sale and VAT stay unchanged. If an e-marketplace withholds from its remittance to the seller, the buyer invoice still shows the full sale; platform CWT is a separate settlement matter.
- **B01–B21** correspond to RR 7-2024 s. 6(B)(1-21). Apply the required fields to the **actual** buyer and transaction — a specimen layout or B2B label does not settle buyer TIN, address, VAT route or discount facts.
- **B06 buyer identity** is required BIR data; buyer email is **delivery**, not legal identity. Collect them separately.
- **B13 mixed VAT treatment** needs a recorded owner/legal decision before live issue — no catalog default.
- **B14 (zero-rated/VAT-exempt label)** and **B16-B17** (non-VAT percentage/exempt) require transaction-specific ground + evidence. Foreign buyer alone is insufficient for zero-rating. Seller stays VAT-registered on exempt sales.
- **Backup obligation** (RR 9-2009 ss. 5.5, 6.1-6.2): a separate Philippine site, with a tested restore. Google Drive is optional extra, NOT the required backup.
- **Release evidence** (NIRC s. 237; RMC 98-2026 IV(4)(b), (7), (11)): freezing an invoice and proving buyer transmission are separate events. Stored PDF, shown QR, send attempt or refusal note alone cannot prove completion. Six distinct event types logged on the host.
- **Outage** (RMC 98-2026 IV(8, 11)): use BIR-authorized manual stock during disruption; reconcile manual serial to corresponding e-invoice after restore (citing its number). Email failure after freeze is a delivery issue, not an outage — do not allocate another serial.
- **Reprint** (RMC 98-2026 IV(4), (7)): exact issued version, original serial/reference, REPRINT marking; record actor and time; never alter original bytes.
- **Local control gates** (E01–E10, RMC 98-2026 IV(2, 4, 6-8, 11-15); RR 9-2009 ss. 5.5, 6.1-6.2): registered system generates structured invoice; electronically transmit digital invoice to buyer; extract/convert invoice data for BIR format; printed copy on request; no alteration/deletion; authorized manual invoice during outage; HO PTI plus owner-only build match before issue; certification within six months after PTI; PTT only on Commissioner reporting directive; Philippine records + separate local backup + restore. Reporting is **conditional**, not default on.

Source PDF: `/Users/markme/Downloads/wwww.pdf` (34 pages, 2 Nov 2026).


# E15 RED-REVIEW OPEN DECISIONS (2 Nov 2026) — read first

The 48-step e14 flowchart was re-reviewed on 2 Nov 2026. New open items from the Red Review, authoritative over the legacy P/D list below where they overlap. Add rulings and dates as ETM / the taxpayer / BIR settle them.

## New open decisions from the Red Review

| ID | Question | Owner | Default until settled |
|---|---|---|---|
| **D1-rr** | Classify actual public source/provider and invoicing software route. RMC 110-2026 / RMO 24-2023 exact application remains open. | Owner + BIR | Treat as ESP-track unless written BIR exemption received. |
| **D2-rr** | Confirm RR 6-2026 publication/effectivity for each event date; future faces must support conditional CWT. | Engineer + reviewer | Assume effective; render CWT field on every face; amount conditional. |
| **D3-rr** | Set mixed-sale and non-VAT specimen decisions. Do not infer one from a reference image. | Owner + tax reviewer | Hold ambiguous sale before issue. |
| **D4-rr** | Owner alone matches issued HO PTI to build; verify MSME/no branches, current host build (Windows first; Mac when released), phone enrollment, certification, backup and schema. Also: does the B2B delivery email carry the full invoice or a protected retrieval link? | Owner + privacy reviewer | No live issue until attestation; test both carriage options before owner picks. |
| **D5-rr** | For each custom build: ownership of source, invoice data, keys, updates and support. "User-owned" does not settle IP terms. | Owner decision | Taxpayer holds keys and live data; IP terms documented separately before live. |
| **T1-rr** | Load test: 1,000 synthetic invoices/day and 10 sessions on reference host; latency and zero duplicate serial. | Engineer + reviewer | Run before gate. |
| **T2-rr** | Power, network, lost phone, crash, refusal, wrong buyer and restore drills. | Engineer + operator | Run before gate; no false success / data loss / silent gap. |
| **G1-rr** | Freeze hash, complete authority/record/reader reviews, taxpayer acceptance. | Independent reviewers | Gate not set until evidence exists. |
| **A3-rr** | Order the BIR gates: PTI before electronic issue; EIS certification within six months after PTI for mandated issuers; reporting/PTT under applicable BIR direction. | — | Enforce the order in code. |
| **A5-rr** | Backup needs proof — name both Philippine sites, keep readback record. Google Drive alone is not the required backup. | — | Implement separate Philippine backup first. |
| **A6-rr** | Exact system/provider classification remains unresolved (RMO 9-2021 Annex B; RMO 24-2023; RMC 110-2026). | — | Keep provider classification unresolved; do not claim exemption. |
| **B22-rr** | CWT phrase/field is universal and visible; amount blank / N/A / 0.00 when CWT does not apply (RMC 112 §III(B)). | Record | Implement as universal field now. |
| **S04-rr** | Buyer editor needs "Confirm buyer email for delivery" distinct from "Check buyer invoice details"; Filipino / plain-English help for tax terms. | Reader / owner | Build as two prompts. |
| **S08-rr** | Explain failed send: "Invoice already created; resend this same invoice to the corrected buyer email." Keep issued face and serial fixed. | Reader / test | Build as copy; never allocate a new serial on resend. |
| **Phone-rr** | Phone disconnect during Issue: "Phone disconnected — check invoice status on PC before tapping Issue again." Host offline / permit mismatch blocks issue. | Design / test | Build as copy and guard. |

Source PDF: `/Users/markme/Downloads/wwww.pdf` (34 pages, 2 Nov 2026).


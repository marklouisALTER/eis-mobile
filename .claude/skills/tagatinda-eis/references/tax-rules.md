# Tax rules — the lists the system classifies by

Everything here comes from the proposal (Edition 1, 29 Sep 2026). Laws were read at lawphil.net on 29 Sep 2026: NIRC Sec. 109(1)(A)–(CC) as amended by RA 9337, RA 10963, RA 11534, RA 12023 and RA 11976, and RA 7160 §143(c)(1)–(8). Do not add paragraphs, letters or citations that are not here. Anything new goes to ETM first.

## Contents
- VAT class derivation
- Exempt from VAT — goods (Sec. 109(1))
- Exempt from VAT — services (Sec. 109(1))
- Paragraphs deliberately not offered
- Essential goods — RA 7160 §143(c)
- Zero-rated consumers (items 1–4)
- Exempt individuals — Senior Citizen and PWD (5a, 5b)
- 5% special discount — basic necessities and prime commodities
- Legal basis text printed on the invoice

---

## VAT class derivation

```
if article.exempt_paragraph:             VAT-EXEMPT SALE · "Sec. 109(1)(<letter>)"
elif consumer.status in {1,2,3,4}:        ZERO-RATED SALE · <that item's ground for goods/services>
else:                                     VATABLE 12% · "Sec. 106(A)" (goods) or "Sec. 108(A)" (services)
then, per sale, if SC/PWD Table A ticked on a VATABLE line:
                                          VAT removed → VAT-EXEMPT, ground RA 9994 §4(a) / RA 10754 §32(a)
```
The RA 7160 class (ESSENTIAL / NON-ESSENTIAL) is separate and never changes the VAT class.

## Exempt from VAT — goods (Sec. 109(1)), one tick at most

| Letter | Paragraph |
|---|---|
| A | Sale of agricultural and marine food products in their original state; livestock and poultry for food; breeding stock and genetic materials (polished/husked rice, corn grits, raw cane sugar and molasses, ordinary salt and copra count as original state) |
| B | Sale of fertilizers; seeds, seedlings and fingerlings; fish, prawn, livestock and poultry feeds and their ingredients (not specialty feeds for pets, race horses, fighting cocks, aquarium fish, zoo animals) |
| K | Sale of goods exempt under an international agreement the Philippines signed, or under a special law (except PD 529) |
| L | Sale by a CDA-registered agricultural cooperative to its members, and of its own produce to non-members |
| N | Sale by a CDA-registered non-agricultural, non-electric, non-credit cooperative (member share capital ≤ ₱15,000) |
| O | Export sale by an Owner who is not VAT-registered |
| P | Sale of real property not held for sale or lease in the ordinary course of business; socialized housing; house and lot or other residential dwelling within the price ceiling |
| R | Sale or publication of books, newspapers, magazines, journals, reviews, bulletins and UNESCO-covered educational reading material, including digital or electronic format, not principally paid advertisements |
| T | Sale or lease of passenger or cargo vessels and aircraft, including engine, equipment and spare parts, for domestic or international transport |
| Z | Sale of gold to the Bangko Sentral ng Pilipinas |
| AA | Sale of DOH-listed prescription drugs and medicines for diabetes, high cholesterol, hypertension, cancer, mental illness, tuberculosis and kidney diseases |

## Exempt from VAT — services (Sec. 109(1)), one tick at most

| Letter | Paragraph |
|---|---|
| E | Services subject to percentage tax under Title V |
| F | Services by agricultural contract growers; milling for others of palay into rice, corn into grits, sugar cane into raw sugar |
| G | Medical, dental, hospital and veterinary services, except those rendered by professionals |
| H | Educational services, including online courses, seminars and trainings, by DepEd/CHED/TESDA-accredited private institutions and by government institutions; online subscription-based services sold to DepEd, CHED, TESDA and their recognised institutions |
| J | Services by a regional or area headquarters of a multinational that earns no income from the Philippines |
| K | Services exempt under an international agreement the Philippines signed, or under a special law (except PD 529) |
| M | Lending services of a CDA-registered credit or multi-purpose cooperative |
| Q | Lease of a residential unit with monthly rental of ₱15,000 or less |
| R | Printing or publication services for the reading materials in Goods (R) |
| S | Transport of passengers by international carriers |
| V | Services of banks, non-bank financial intermediaries performing quasi-banking functions and other non-bank financial intermediaries, including through digital platforms |
| Y | Association dues, membership fees, assessments and charges collected by a homeowners' association or condominium corporation |

Paragraphs that cover both goods and services (K, L, R) appear in both lists.

## Paragraphs deliberately not offered

- **(C), (D), (U), (dd)**: importation paragraphs.
- **(I)**: employment.
- **(W)**: senior citizen / PWD. This is decided per sale by the consumer's status, not by the article.
- **(X)**: property transfers under Sec. 40(C)(2).
- **(BB)**: COVID-19 goods. The window closed 31 Dec 2023.
- **(CC)**: the Owner's ₱3 million status. This belongs to the Owner record, not to an article.

## Essential goods — RA 7160 §143(c) (goods only)

No tick = NON-ESSENTIAL. A tick prints `ESSENTIAL — 143(c)(n)`.

1. Rice and corn
2. Wheat or cassava flour, meat, dairy products, locally manufactured, processed or preserved food, sugar, salt and other agricultural, marine, and fresh water products, whether in their original state or not
3. Cooking oil and cooking gas
4. Laundry soap, detergents, and medicine
5. Agricultural implements, equipment and post-harvest facilities, fertilizers, pesticides, insecticides, herbicides and other farm inputs
6. Poultry feeds and other animal feeds
7. School supplies
8. Cement

## Zero-rated consumers (items 1–4)

A tick makes every line sold to this consumer a ZERO-RATED SALE (exempt-ticked articles stay VAT-EXEMPT), with the item's ground printed beside the line. The document reference typed on the record also prints beside the line.

| Item | Consumer | Ground — goods | Ground — services | Papers |
|---|---|---|---|---|
| 1 | Direct exportation to a foreign country | Sec. 106(A)(2)(a)(1) | Sec. 108(B)(1), (2), (6) | Keep proof of actual shipment abroad and payment in acceptable foreign currency accounted for under BSP rules. |
| 2 | Registered export enterprise — with PEZA, BOI, SBMA, CDC, BCDA, CEZA, AFAB, TIEZA or any other Investment Promotion Agency | Sec. 295(D)(1), Title XIII, as amended by RA 12066 — purchases directly attributable to the registered project | same | **Required before the first zero-rated sale (RMC 80-2023):** (a) VAT Zero-Rate Certificate issued by the IPA; (b) BIR Certificate of Registration of the delivery location; (c) IPA Certificate of Registration; (d) the consumer's sworn affidavit of direct use. Caution: a BOI- or PEZA-registered domestic market enterprise does not qualify — its local purchases are at 12% (Sec. 295(D), proviso). |
| 3 | Embassy, international organisation or other entity exempt under a special law or an international agreement | Sec. 106(A)(2)(b) | Sec. 108(B)(3) | The BIR-issued VAT exemption certificate or ruling, or the treaty identification card / VAT exemption card, with its validity date. **CAUTION: paragraph lettering after RA 10963 / 11534 / 12066 to be pinpointed against the consolidated Code before the face is approved.** |
| 4 | International shipping or international air transport operator | Sec. 106(A)(2)(a)(4) | Sec. 108(B)(4) | Proof that the consumer is engaged in international shipping or air transport and that the goods are for that use. |

The specimen invoice used **Sec. 106(A)(2)(a)(3)** (export-oriented enterprise, export sales ≥ 70% of production, DTI-EMB certifies). The ETM consumer list merged the old items 2 and 3 into "registered export enterprise with an IPA", because the law zero-rates the enterprise's registration and export status, not the zone or the agency. Which ground the specimen should carry is open (see `open-decisions.md`).

## Exempt individuals — Senior Citizen and PWD (5a, 5b)

These are per-sale privileges, not a permanent status of the consumer.

- **5a Senior Citizen — RA 9994.** 20% discount and VAT exemption on covered goods and services; 5% special discount on basic necessities and prime commodities.
- **5b Person with Disability — RA 10754.** Same benefits.
- The record needs the **ID No.** and **Date issued**; without both, the status is not saved.
- They print in the SC/PWD space (RR 7-2024 §6(B)(18)). The consumer signs, except for online or app purchases.

**Table A (20% + VAT removed)**: covered list.
- Senior Citizen, RA 9994 §4(a):
  1. medicines, influenza and pneumococcal vaccines, DOH-listed medical supplies and equipment;
  2. and 3. professional fees of physicians and licensed home-health providers;
  4. medical, dental, diagnostic and laboratory services in private facilities;
  5. land transport fares;
  6. domestic air and sea fares;
  7. hotels, lodging, restaurants, recreation centers;
  8. admission to theaters, cinemas, concert halls, circuses, leisure and amusement;
  9. funeral and burial services.
- PWD, RA 10754 §32(a): the same classes, (1)–(8).
- The tagaTinda ticks each line that is on the list and for the consumer's own use.

## 5% special discount — basic necessities and prime commodities

- Source: DTI-DA-DOE JAO 24-02, 21 March 2024.
- Goods only. No VAT exemption.
- Applies to purchases up to ₱2,500 per week, so the discount is at most ₱125 per week.
- Needs the ID and the purchase booklet.
- Rule of one: a line takes A or B, never both, and there is no stacking with store promos (RA 10754 §32).

## Legal basis text printed on the invoice

Print the paragraphs that apply to the classes on the invoice, plus the 113(B)(2)(d) breakdown rule.

- **SEC. 106(A). Rate and Base of Tax.** — There shall be levied, assessed and collected on every sale, barter or exchange of goods or properties, a value-added tax equivalent to twelve percent (12%) of the gross sales of the goods or properties sold, bartered or exchanged, such tax to be paid by the seller or transferor.
- **SEC. 108(A).** — … a value-added tax equivalent to twelve percent (12%) of the gross sales derived from the sale or exchange of services, including the use or lease of properties.
- **SEC. 106(A)(2)(a)(3).** — The following sales by VAT-registered persons shall be subject to zero percent (0%) rate: (a) Export Sales … (3) Sale of goods to an export-oriented enterprise whose export sales is at least seventy percent (70%) of the total annual production of the preceding taxable year; … the Export Marketing Bureau of the Department of Trade and Industry (DTI) shall determine compliance with the aforementioned threshold.
- **SEC. 109(1)(A).** — The following transactions shall be exempt from the value-added tax: (A) Sale or importation of agricultural and marine food products in their original state, livestock and poultry of a kind generally used as, or yielding or producing foods for human consumption; and breeding stock and genetic materials therefor.
- **SEC. 113(B)(2)(d).** — If the sale invoices goods, properties or services some of which are subject to and some of which are VAT zero-rated or VAT-exempt, the invoice shall clearly indicate the breakdown of the sale price between its taxable, exempt, and zero-rated components, and the calculation of the value-added tax on each portion of the sale shall be shown on the invoice.
- Citation line: "National Internal Revenue Code of 1997, as amended · Secs. 106(A), 108(A), 113(B)(2)(d) as amended by RA No. 11976 (Secs. 16, 17, 21) · Sec. 106(A)(2)(a)(3) as amended by RA No. 12066, Sec. 6 · Sec. 109(1)(A) per RA No. 9337, Sec. 7. The ground beside each line is the one named by the Seller; the consumer's zero-rating eligibility record is kept in the Owner's files."

Verbatim text for the other grounds (e.g. 295(D)(1), other 109(1) letters) has not been collected yet: `VALIDATION-NEEDED`. Do not paraphrase law into the legal-basis box.

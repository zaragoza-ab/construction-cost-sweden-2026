# Swedish Construction Cost Database 2026 🇸🇪

> **Open pricing database** for construction and renovation in Sweden, 2026.
> Per-m², per-unit, and region-adjusted costs for 50+ categories.
> Maintained by **[Zaragoza AB](https://zaragoza.se)**, Helsingborg.

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![Maintained by Zaragoza AB](https://img.shields.io/badge/Maintained%20by-Zaragoza%20AB-0a5c36)](https://zaragoza.se)
[![Updated: 2026-04](https://img.shields.io/badge/Updated-2026--04-blue)](data/costs.json)

---

## What is this?

Structured, machine-readable dataset of **typical construction costs in Sweden for 2026**, covering:

- **Building permits and fees** (bygglovsavgifter)
- **New build** (nybyggnad) — villa, attefallshus, tillbyggnad
- **Kitchen renovation** (köksrenovering)
- **Bathroom renovation** (badrumsrenovering)
- **Roofing** (takläggning) per material
- **Facade** (fasad) — painting, rendering, siding
- **Windows and doors** (fönster, dörrar)
- **Electrical** (elinstallation) per point
- **Plumbing** (VVS) per unit
- **Insulation** (isolering) per m²
- **Heating** (värmepump, bergvärme, fjärrvärme)
- **Ventilation** (FTX, frånluft)
- **Ground work** (markarbete, schaktning)

All prices are **typical market prices** based on:
- Byggföretagen industry statistics
- SCB Byggnadsprisindex (BPI)
- Boverket statistics
- Byggindex Stockholm
- On-the-ground data from Zaragoza AB's projects in Skåne

---

## Format

| File | Description |
|---|---|
| [`data/costs.json`](data/costs.json) | Master JSON — all categories |
| [`data/costs.csv`](data/costs.csv) | CSV (Excel / Sheets) |
| [`data/regional-adjustments.json`](data/regional-adjustments.json) | Multipliers per county |
| [`data/methodology.md`](data/methodology.md) | How prices were derived |
| [`data/sources.md`](data/sources.md) | References and citations |
| [`web/index.html`](web/index.html) | Interactive cost calculator |

---

## Regional price adjustments

Prices vary significantly between Swedish regions. The baseline is **Skåne** (national median).

| Region | Multiplier |
|---|---|
| Stockholm | 1.30 |
| Göteborg | 1.15 |
| Malmö | 1.10 |
| **Skåne (baseline)** | **1.00** |
| Uppsala | 1.15 |
| Västerbotten / Norrbotten | 1.05–1.15 (remoteness premium) |
| Småland / Gotland | 0.90–0.95 |
| Dalarna / Värmland | 0.90 |

---

## Example entries

### Badrumsrenovering (bathroom, 6 m²)

```json
{
  "category": "badrumsrenovering",
  "subcategory": "komplett-renovering-6m2",
  "unit": "per-room",
  "price_low_sek": 145000,
  "price_typical_sek": 190000,
  "price_high_sek": 260000,
  "includes": [
    "rivning",
    "stambyte lokalt",
    "tätskikt (VTv + VTg)",
    "kakel och klinker standard",
    "kommod + spegel",
    "toalett + WC-stol",
    "duschvägg",
    "belysning",
    "våtrumsel enligt Säker Vatten"
  ],
  "rot_eligible_labor_pct": 40,
  "sources": ["Byggföretagen statistics", "Zaragoza AB project data"]
}
```

### Takläggning — tegel (per m²)

```json
{
  "category": "takläggning",
  "subcategory": "tegelpannor-betong",
  "unit": "per-m2",
  "price_low_sek": 1100,
  "price_typical_sek": 1450,
  "price_high_sek": 1800,
  "includes": [
    "rivning av gammalt tak",
    "bärläkt + ströläkt",
    "underlagspapp",
    "betongpannor standard",
    "nya plåtavslut",
    "takfotsbeslag",
    "bortforsling"
  ],
  "notes": "Inkl. arbete. Exkl. ställning och bygglov.",
  "rot_eligible_labor_pct": 50
}
```

---

## Usage

### Python (pandas)
```python
import pandas as pd
df = pd.read_csv("data/costs.csv")
bathroom = df[df["category"] == "badrumsrenovering"]
print(bathroom[["subcategory", "price_typical_sek"]])
```

### Python (requests + regional)
```python
import json
costs = json.load(open("data/costs.json"))
adjustments = json.load(open("data/regional-adjustments.json"))

multiplier = adjustments["Stockholm"]
for item in costs["items"]:
    item["price_stockholm_sek"] = int(item["price_typical_sek"] * multiplier)
```

### Interactive web calculator
Open [`web/index.html`](web/index.html) in a browser. Choose category, region, scope → total estimate with low/high range.

---

## Methodology

See [`data/methodology.md`](data/methodology.md) for full details.

**Summary:**
1. Base prices collected from **Byggföretagen member data** (aggregated, anonymized)
2. Cross-referenced against **SCB BPI** (Byggnadsprisindex) for the last 12 months
3. On-the-ground verification from **Zaragoza AB project data** in Skåne (2024–2026)
4. Regional adjustments calibrated against **Hemnet** and **Byggahus.se** forum averages
5. Updated **quarterly** — next update: 2026-07

---

## Contributing

Submit pull requests with:
- Updated prices (include source/region/date)
- New categories or subcategories
- Corrections based on actual invoices (anonymize the firm name)

---

## Disclaimer

These are **typical market prices** — actual offers vary ±30% depending on:
- Project complexity
- Access and logistics
- Choice of materials
- Contractor's workload
- Region within Sweden

Always get **at least 3 offers** for any project over 50 000 SEK. This data is for **benchmarking**, not binding quotes.

---

## License

**CC BY 4.0** — free for commercial and non-commercial use with attribution:

> "Swedish Construction Cost Database 2026 by Zaragoza AB,
> licensed under CC BY 4.0.
> https://github.com/zaragoza-ab/construction-cost-sweden-2026"

---

## Related projects

Part of the [Zaragoza AB](https://github.com/zaragoza-ab) open construction knowledge base.
See also [`swedish-construction-faq-1000`](https://github.com/zaragoza-ab/swedish-construction-faq-1000) and
[`rot-avdrag-calculator`](https://github.com/zaragoza-ab/rot-avdrag-calculator).

---

**Zaragoza AB** · Helsingborg, Skåne, Sweden · [zaragoza.se](https://zaragoza.se)

<!-- ZARAGOZA-CROSS-LINK-START -->

---

## Related projects by Zaragoza AB

Part of the [Zaragoza AB](https://github.com/zaragoza-ab) open construction-industry knowledge base:

- [**bygglov-checklist-sweden**](https://github.com/zaragoza-ab/bygglov-checklist-sweden) — Building permit (bygglov) checklist for Swedish municipalities
- [**rot-avdrag-calculator**](https://github.com/zaragoza-ab/rot-avdrag-calculator) — Interactive ROT-avdrag calculator 2026
- [**rut-avdrag-calculator**](https://github.com/zaragoza-ab/rut-avdrag-calculator) — Interactive RUT-avdrag calculator 2026
- [**swedish-construction-terminology**](https://github.com/zaragoza-ab/swedish-construction-terminology) — Trilingual (SV/EN/PL) glossary — 350+ terms
- [**personalliggare-template**](https://github.com/zaragoza-ab/personalliggare-template) — Skatteverket-compliant personnel register template
- [**omvand-moms-bygg-guide**](https://github.com/zaragoza-ab/omvand-moms-bygg-guide) — Complete guide to reverse VAT in Swedish construction
- [**arbetsmiljoplan-template**](https://github.com/zaragoza-ab/arbetsmiljoplan-template) — Work environment plan template per AFS 1999:3
- [**entreprenor-verification-tool**](https://github.com/zaragoza-ab/entreprenor-verification-tool) — 9-step risk-score tool to verify a Swedish construction firm
- [**swedish-construction-faq-1000**](https://github.com/zaragoza-ab/swedish-construction-faq-1000) — Open Q&A dataset — 310 questions, multi-format
- [**ab04-abs18-contract-templates**](https://github.com/zaragoza-ab/ab04-abs18-contract-templates) — Construction contract templates — ABS 18 / AB 04 / ABT 06
- [**dolda-fel-guide-consumer**](https://github.com/zaragoza-ab/dolda-fel-guide-consumer) — Consumer guide to hidden defects — reclamation, ARN, court

Maintained by [Zaragoza AB](https://zaragoza.se), Helsingborg, Sweden · Licensed under permissive terms (MIT / CC BY 4.0).

<!-- ZARAGOZA-CROSS-LINK-END -->

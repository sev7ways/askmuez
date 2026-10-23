# ASK MUEZ — TCY Motors Brunei Vehicle Finder

[![Netlify Status](https://api.netlify.com/api/v1/badges/54cfabc6-b6f5-48c3-9940-1007198ef7c4/deploy-status)](https://app.netlify.com/projects/askmuez/deploys)

A mobile-first, single-page vehicle finder for **TCY Motors Brunei** (BAIC · ARCFOX · FOTON). A customer picks a brand and model, sees quoted monthly payments, runs a live financing estimate, reviews an enquiry summary card, and sends it to the sales consultant on WhatsApp.

## Customer flow

| Step | Screen | What happens |
|---|---|---|
| 1 | **Brand** | Choose BAIC, ARCFOX or FOTON from logo buttons. |
| 2 | **Catalogue** | The brand's models, each with a photo gallery and a "from" price. |
| 3 | **Select** | Pick a variant and optional colour. Quoted payment tenures are shown for that variant. |
| 4 | **Estimate** | Live financing calculator (deposit, tenure, net income, existing debt, flat rate). |
| 5 | **Summary** | Enquiry summary card, optional name and notes, then **ASK MUEZ on WhatsApp**. |

A **BACK** button sits in the bottom bar from step 2 onward. It works together with the phone's own back button and the progress stepper.

## Vehicles and variants

| Brand | Model | Variants |
|---|---|---|
| BAIC | X55 II | Fashion, Luxury, REHEV Hybrid Honor, REHEV Hybrid Luxury |
| BAIC | BJ30 | Lv2 ICE, Lv3 ICE, Lv4 Hybrid (2WD), Lv5 Hybrid (4WD) |
| BAIC | BJ40L | Standard |
| ARCFOX | T1 | Low Spec, High Spec |
| FOTON | Tunland V7 | V7 |
| FOTON | View CS2 | 9-Seater, 15-Seater, 17-Seater |

## How payments are labelled

Payments are never presented as a single undifferentiated number.

| Tag | Meaning |
|---|---|
| **Quoted** | A figure taken directly from the TCY price sheet. |
| **Calculated** | A figure the price sheet itself derived. Indicative only. |
| **… · confirm** (amber) | The 10-year column of the price sheet. See the note below. |
| **Estimate** (blue) | Output of the live calculator on this page. Not a quote. |

### Financing estimator assumptions

- **Method:** flat-rate interest. `monthly = (price − deposit) × (1 + rate × years) ÷ (years × 12)`.
- **Default rate:** 4.25% flat (indicative). Editable. Presets: 4.25%, 4.00% (what the 5–7 year price-sheet figures work out to), 5.00% (what the 10-year figures work out to).
- **Debt-service check (optional, needs income):** 60% of net income minus existing monthly debt.
  - Net income below BND 1,750: shown as *subject to lender assessment*.
  - Net income of BND 10,000 or more: the cap is shown as indicative only.
  - Verdicts: **Within cap**, **Close to cap** (within 10% of the limit), **Over cap**, plus an indicative maximum financing amount.
- **Tenure:** 1–10 years are selectable. **The regulated maximum tenure for motor vehicle financing in Brunei is 7 years.** Tenures of 8–10 years follow the price sheet only, are flagged *confirm with bank*, and show a warning in the estimator and in the WhatsApp message. If a bank confirms 10-year financing is not offered, remove the `10:` entries in `SKUS` (see below).
- **Privacy:** income and debt are only added to the WhatsApp message if the customer ticks the box on the summary screen.

All figures are reference figures, subject to confirmation by TCY Motors and bank approval. This is not a booking or a final quotation.

## WhatsApp enquiry

**ASK MUEZ on WhatsApp** opens `https://wa.me/6738138884` with a prefilled message containing an enquiry ID, the vehicle and variant, colour, list price, quoted tenures, the customer's estimate, optional income and debt, and notes. The ID is also logged to the browser's `localStorage` on that device only.

## Repository layout

```
.
├── index.html        # the whole app: HTML, CSS, JS and images (self-contained)
├── netlify.toml      # Netlify settings (static site, publish the repo root)
├── tests/
│   └── test_flow.py  # Playwright end-to-end test (42 checks)
└── README.md
```

There is no build step and no dependencies at runtime. Photos and logos are embedded in `index.html` as base64, so the page works offline apart from the Google Fonts stylesheet and the WhatsApp link.

## Updating prices, variants and photos

All data lives near the top of the `<script>` in `index.html`:

- `FAMILIES` — models: name, brand, tagline, gallery image keys, colour swatches.
- `SKUS` — variants: price, payments, powertrain, fuel, seats, highlights.
- `IMG` — the embedded images (base64), keyed by name.
- `CONSULTANT` — consultant name and WhatsApp number (digits only, with country code).

A payment is written as `{ 7: q(385), 10: qx(315) }`:

| Helper | Use for |
|---|---|
| `q(v)` | Quoted in the price sheet |
| `c(v)` | Calculated in the price sheet |
| `qx(v)` / `cx(v)` | Same, but flagged *confirm with bank* (10-year column) |

Use `TBC` (`null`) for anything unconfirmed. The page shows "To be confirmed" and never guesses.

## Run locally

Open `index.html` in a browser. Or serve the folder:

```bash
python3 -m http.server 8080   # then visit http://localhost:8080
```

### Run the tests

```bash
pip install playwright
playwright install chromium
python3 tests/test_flow.py
```

The suite checks the data against the price sheet (price and every tenure for all 15 variants), brand alignment, the full flow, the BACK button and device back, the estimator maths and verdicts, the WhatsApp message contents, and horizontal overflow at 320, 390 and desktop widths.

## Deploy

**Netlify (continuous deploy from GitHub):**

1. Push this repository to GitHub.
2. In Netlify: *Add new site → Import an existing project → GitHub*, pick the repository.
3. Build command: leave empty. Publish directory: `.`
4. Deploy. Every push to the main branch redeploys the site. Netlify keeps previous deploys, so you can roll back from the Deploys page.

**Manual:** drag the folder (or just `index.html`) onto the Netlify Deploys page.

## Known gaps and items to confirm

- **BJ40L price:** the internal price sheet says BND 44,600. A third-party listing (AutoPasar) shows BND 39,800. The page uses the price sheet.
- **X55 REHEV:** priced from the internal price sheet (BND 27,800 and 29,500). The same third-party listing shows the price as to be confirmed.
- **Rate:** the 5–7 year price-sheet figures equal about 4.00% flat, below the 4.25% indicative default. Confirm whether this is a dealer promotion.
- **10-year tenure:** flagged for bank confirmation (see above).
- **Photos:** ARCFOX T1, Tunland and CS2 each have a single photo. Exterior colours for ARCFOX, FOTON and the X55 REHEV are not in the source files, so those cards ask the customer to check with the consultant.
- **Specs not in the source files:** Tunland and CS2 powertrain, fuel and (for Tunland) seats show "To be confirmed".
- **Catalogue inconsistency:** the BJ40L catalogue table says petrol 2.0T, while one caption says "Diesel 2.0TD Euro IV". The page uses petrol.
- **Variant mapping:** ARCFOX Low Spec and High Spec are assumed to correspond to the spec sheet's LV2 and LV3.
- **Swatches:** colour dots are approximate. Only BJ30 has per-colour renders.

## Copyright

© 2026 Muez Arifin. All Rights Reserved. Original concept, workflow and structure by Muez Arifin for TCY Motors Brunei. Vehicle names, logos and photographs belong to their respective owners (BAIC, ARCFOX, FOTON, TCY Motors).
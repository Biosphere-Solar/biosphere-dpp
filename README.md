# Biosphere Solar DPP (Digital Product Passport)

A single-page, static web app for publishing Digital Product Passports for our solar panels. Everything runs client-side (no backend), so it works on GitHub Pages and any static host.

## 1) How it works (quick mental model)

- The page loads index.html and reads the query string ?serial=....
  
- It fetches /passports/<SERIAL>.json (case-sensitive filename) and renders:
  
    - Header KPIs (power, efficiency, etc.)
  
    - “Passport info” (origin, compliance, hazardous substances…)
  
    - Composition doughnut (Material / Chemical / Value (price) views)
  
    - Component Explorer (per-component: Specimen info, Chemical, Sourcing, Design)
  
   - Mechanical properties explorer (and Use / EoL / Sorting / Regulations / Environment)
  
    - Extra categories (e.g., “General composition”)

If the dataset for a module is missing (404), the page shows an inline error.

## 2) Repository structure

```
repo-root/
├─ index.html                  # App (HTML + CSS + JS in one file)
├─ passports/                  # One JSON per module (by SERIAL)
│  ├─ BS-2025-0001.json
│  ├─ KS2508P01003.json
│  └─ … more
├─ assets/                     # Static assets
│  ├─ biosphere-logo-white.png
│  ├─ biosphere-logo-dark.png  # (optional, used in light mode)
│  └─ … images, PDFs, etc.
└─ README.md                   # This file
```

## 3) Run locally / Deploy

### Local (any static server)

- Open directly with Live Server (VS Code) or:

```
  python3 -m http.server 8080
```

- Visit: http://localhost:8080/?serial=KS2508P01003

### GitHub Pages

- Settings → Pages → Source: main (or your default branch) → Root.

- Public URL will be something like:

```
  https://<org>.github.io/<repo>/?serial=KS2508P01003

```

- Use this URL in your QR codes if you want to deep-link to a specific module.

## 4) Creating / updating a passport JSON

Put a file in passports/<SERIAL>.json. Keep keys consistent with examples below.

### Minimal skeleton

```

  {
  "uid": "urn:bs:pv:KS2508P01003",
  "version": "1.0",
  "last_updated": "2025-10-01",
  "model": "V1.4 IBC",
  "manufacturer": "Biosphere Solar",
  "manufacture_date": "2025-01-07",
  "power_w": 392,
  "cell_type": "TOPCon",
  "dimensions_mm": "1952 x 1134 x 35",
  "weight_kg": 26.37,
  "efficiency_pct": 21.6,
  "co2e_kg": null,
  "lifetime_years": 25,
  "warranty_years": 25,
  "certifications": ["IEC 61215", "IEC 61730"],

  "materials": {
    "Glass": "Low iron semi tempered soda lime glass",
    "Cells": "Bifacial TOPCon",
    "Frame": "6005 Alu",
    "Edge Seal": "Desiccated PIB",
    "Junction Box": "Copper and PPO"
  },

  "material_composition_pct": {
    "glass": 45,
    "cells": 15,
    "edge seal": 3,
    "junction box": 5,
    "frames": 10
  },

  "chemical_composition_pct": {
    "copper (cu)": 0.30,
    "phosphorus (p)": 0.23,
    "silicon (si)": 0.40,
    "lead (pb)": 0.08
  },

  "cost_breakdown_eur": {
    "glass": 120,
    "cells": 51,
    "edge seal": 0.8,
    "junction box": 3.40,
    "frames": 12
  },

  "safety_datasheet_url": "https://.../Safety-Datasheet.pdf",
  "esg_statement_url": "https://.../ESG.pdf",
  "lca_report_url": "https://.../LCA.pdf",
  "epd_url": "Not Available",

  "hs_code": null,
  "origin": "EU",
  "compliance": ["Reg. (EU) 2024/1781 framework", "IEC 61215", "IEC 61730"],
  "hazardous_substances": ["Lead solder (SnPb) <0.1%", "No PFAS in encapsulant"],
  "recycled_content": { "Glass": "0%", "Aluminium frame": "0%" },
  "recyclability": "≥90% by weight via standard PV recycling with disassembly route",
  "updates": ["2025-09-01: Junction box replaced by Installer Co."],

  "components": [
    {
      "name": "Final sample panel",
      "categories": {
        "Component/Specimen information": {
          "Part details": [
            { "label": "Length", "value": 1952, "unit": "mm", "datatype": "number" },
            { "label": "Width",  "value": 1134, "unit": "mm", "datatype": "number" },
            { "label": "Height", "value": 35,   "unit": "mm", "datatype": "number" }
          ]
        },
        "Chemical composition": { "Presence (Yes/No)": [ { "label": "SVHCs", "value": true, "datatype": "boolean", "comments": "Lead" } ] },
        "Sourcing composition": { "—": [ { "label": "Virgin/Fossil (%)", "value": 100, "unit":"%","datatype":"number" } ] },
        "Design": { "Lay up scheme": [ { "label": "Product drawings", "datatype": "file", "files": ["https://.../panel-datasheet.pdf"] } ] }
      }
    }
  ],

  "extra_categories": {
    "General composition": {
      "Spacer Dot": {
        "Resin system": [ { "label": "Resin details", "value": "Acrylate Resin mixture", "datatype": "text" } ],
        "Fibre": [ { "label": "Fibre type", "value": null, "datatype": "text" } ]
      }
    },

    "Use": { "Final sample panel": { "Instructions of use and assembly": [ { "label": "Instructions (PDF)", "datatype": "file", "files": ["https://.../use.pdf"] } ] } },

    "End of life (EoL)": {
      "Final sample panel": {
        "Recycling": [
          { "label": "Recycler name", "value": "Solar2Cycle", "datatype": "text" },
          { "label": "Can the product be mechanically recycled?", "value": true, "datatype": "boolean" }
        ]
      }
    },

    "Mechanical properties": {
      "Final sample panel": {
        "—": [ { "label": "Tensile strength", "value": 120, "unit":"MPa", "datatype":"number" } ]
      }
    },

    "Sorting": { "Final sample panel": { "NDT Results": [ { "label":"NDT", "value":"Yes", "datatype":"text"} ] } },
    "Regulations": { "Final sample panel": { "REACH": [ { "label": "REACH status", "value":"Internal declaration only", "datatype":"text"} ] } },
    "Environment": { "Final sample panel": { "LCA data": [ { "label":"Energy Use", "value":"450–730", "unit":"kWh/kWp", "datatype":"text"} ] } }
  }
}
```

## Important content rules / gotchas

- **Serial & filename** must match exactly: ```passports/KS2508P01003.json ↔ ?serial=KS2508P01003```.
  
- **Keys & labels**:
  
- Our color map uses normalized labels (“glass”, “cells”, “frame”…). If you change a label (e.g., "junction box, cables, MC4" vs "junction box"), the chart still renders, but it’ll use a fallback color. Keep names consistent or extend the color map in index.html (MATERIAL_MAP) to include your new label.
  
- For **Chemical** keys, we normalize lower-case but your JSON should still be consistent (e.g., phosphorus (p) vs phosphorus (po)).
  
- **Hide empty tabs automatically**:
  
  If ```material_composition_pct``` is missing/empty → the Material tab is hidden.
  If ```chemical_composition_pct``` is missing/empty → Chemical tab is hidden.
  If ```cost_breakdown_eur``` is missing/empty → Value tab is hidden.
  
- **Booleans**: use real booleans ```(true/false)``` where possible. Strings “Yes/No” also render, but prefer booleans.
  
- **Files**: to keep sensitive docs private, link to Google Drive with viewer permission restricted to our org. The UI only shows an Open link; Drive controls the access.

## 5) The three doughnuts (what feeds them)

- **Material composition** (%) → ```material_composition_pct``` Values must be numbers or strings with numbers (e.g., 45 or "45%").
  
- **Chemical composition** (%) → ```chemical_composition_pct``` Same numeric rules.
  
- **Value (by price, €)** → ```cost_breakdown_eur``` Accepts numbers ```(120)``` or strings like ```"€120.00"```. The app parses them and converts to percentages under the hood.
  
  If two datasets present (e.g., Material + Value), all three tabs appear. If one is absent, its tab is omitted.

## 6) Component Explorer (left) & Mechanical/Use/EoL (right)

- **Component Explorer** is driven by ```components[].categories```.

    - Category names you can use (free text): ```Component/Specimen information```, ```Chemical composition```, ```Sourcing composition```, ```Design```, …

- Each **category** contains **groups** (object keys), each **group** is an **array of fields** like:

```
  { "label":"Width", "value": 1134, "unit":"mm", "datatype":"number" }

```
  Other ```datatypes``` we support: ```text```, ```number```, ```boolean```, ```file``` ```(files: [urls])```.

- **Mechanical properties explorer** (right column) is driven from ```extra_categories```:

  - It shows only these categories when present:
    **```“Mechanical properties”```,** **```“Use”```,** **```“End of life (EoL)”```,** **```“Sorting”```,** **```“Regulations”```,** **```“Environment”```.**
  
  - Put them inside ```extra_categories``` (not top-level).
    This is a common mistake that hides your content.


## 7) Theming & branding

- Colors live in CSS variables at the top of ```index.html```.

```
  :root{
    --brand:#00d18f; --brand-2:#6f7dff;
    --bg1:#222d1b;               /* dark green */
    --bg2:#aabf7b;               /* soft olive */
    --card:#0f1326;
    --line:#20274a;
    --muted:#9aa3b2;
    --text:#e5e9f0;
  }
  :root[data-theme="light"]{
    --brand:#166534; --brand-2:#1d4ed8;
    --bg1:#f7fafc; --bg2:#eef2f7;  /* override if you want a light olive theme */
    --card:#ffffff; --line:#e2e8f0;
    --muted:#475569; --text:#0f172a;
  }

```

- Background gradient is set on the ```body``` rule; tweak there.

- **Fonts** are injected via Google Fonts link and then used by CSS variables:
  - Title: Source Code Pro (ExtraBold), Headings: Exo 2, Body: PT Sans.

- **Logo**: in the “phone” header we use:

```
  <div class="pill pill-brand">
    <img src="assets/biosphere-logo-white.png" alt="Biosphere Solar" class="brand-logo" />
    <span class="brand-abbrev">DPP</span>
  </div>
```

If the white logo disappears in light mode, add a dark variant and toggle with CSS:

```

  <img src="assets/biosphere-logo-white.png"  class="brand-logo brand-logo--dark"  alt="Biosphere Solar">
  <img src="assets/biosphere-logo-dark.png"   class="brand-logo brand-logo--light" alt="Biosphere Solar">
```

```
  .brand-logo--light{ display:none; }
  :root[data-theme="light"] .brand-logo--dark{ display:none; }
  :root[data-theme="light"] .brand-logo--light{ display:block; }
```

## 8) Adding a new module

1. Create a JSON: ```passports/<SERIAL>.json``` following the skeleton.
  
2. Test locally: ```/?serial=<SERIAL>```
  
3. Commit & push → GitHub Pages updates automatically.
  
4. (Optional) Make a QR code that points to:

  - the DPP **landing/search** page (preferred): ```https://<org>.github.io/<repo>/```
    
  - or deep-link directly to a module: ```https://<org>.github.io/<repo>/?serial=<SERIAL>```

Any QR generator is fine (Adobe, QRCode Monkey, etc.). Save as SVG/PNG; test with a phone.

## 9) Editing data later (what to change, where)

- **Header KPIs**: update the top-level fields in JSON (```power_w```, ```efficiency_pct```, etc.).
  
- **Docs** (Safety, ESG, LCA, EPD): update those 4 URLs.
  
- **Composition tabs**:

    - Update ```material_composition_pct```, ```chemical_composition_pct```, ```cost_breakdown_eur```.
    
    - Remove a dataset entirely to hide its tab.

- **Explorer tables**:

    - Add fields inside ```components[].categories``` per component.

- **Mechanical / Use / EoL etc**.:

    - Put them inside ```extra_categories``` under the exact category names listed above.

- **Recycled content & bars**:

    - Put non-zero percentages in ```recycled_content```; zero or missing values are **hidden** automatically to avoid empty bars.

## 10) Common pitfalls & fixes

- **JSON not loading**: 404 in console → filename/serial mismatch.
    Fix: make sure ```passports/<SERIAL>.json``` exists and the URL param matches the case.
  
- **Material tab shows but no chart**: one of the values is not numeric.
  Fix: ensure every value in ```*_composition_pct``` is a number (or string that contains only a number/percent).
  
- **Wrong colors or duplicated colors**: your labels don’t match the color map.
  Fix: stick to our names (```glass```, ```cells```, ```frame```, ```edge seal```, ```junction box```, ```tabbing wire```, ```bus bars```, ```frames```) or add your label to ```MATERIAL_MAP``` in ```index.html```.
  
- **Mechanical/EoL missing**: you placed those categories at the root of the JSON.
  Fix: move them under **extra_categories**.
  
- **Logo shows “broken image” on mobile**: wrong path.
  Fix: ```src="assets/<file>"``` (relative to ```index.html```) and commit the file.

## 11) Style changes your teammates may want

- **Background**: change ```--bg1``` / ```--bg2``` in ```:root``` and ```:root[data-theme="light"]```.
  
- **Buttons**: the green brand color is ```--brand```; outline uses ```--line```.
  
- **Table colors** are controlled centrally:

```
    th{ color: var(--muted); font-weight:600; width:38%; }
    td{ color: var(--text); }

```
- **Font stacks** are defined once; swap families in the CSS variables if needed.

- **Composition colors**: edit ```MATERIAL_MAP``` / ```CHEMICAL_MAP```. New labels fall back to a distinct palette.

## 12) Data hygiene & validation

- Use a JSON linter (e.g., jsonlint.com or VS Code built-in) to avoid trailing commas / missing quotes.

- Prefer numbers for numeric values; use null when unknown (not empty strings).
  
- Keep units in ```unit``` fields (e.g., ```"mm"```, ```"W"```, ```"%"```)—not inside ```value```.

## 13) Accessibility & UX

- Always provide ```alt=""``` text for images (logo already has it).
  
- Links open in new tabs and are labeled “Open”.
  
- Tabs auto-wrap on small screens; tables do not overflow.

## 14) Questions & ownership

- Anyone can add a module by PR that adds a JSON file under ```passports/```.
  
- If you change the **schema** (new categories, fields), keep it backward compatible or add guards in ```index.html```.
  
- If in doubt about where a piece of data should live, put it in ```extra_categories``` → ```General composition``` under the relevant component.


## Appendix: converting € to percentages (if you want Material by €)

- Put raw costs in ```cost_breakdown_eur```.
  
- The app converts them to % for the **Value** chart automatically:

  \text{% of item} = \frac{\text{cost}_i}{\sum \text{costs}} \times 100
  
- You can still keep **Material** (% by mass) and **Chemical** (% by element) if you have those; otherwise, remove the section to hide the tab.

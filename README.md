# biosphere-dpp

# Biosphere Solar - Digital Product Passport (DPP)

This repository contains the prototype for Biosphere Solar’s Digital Product Passport (DPP) for solar panels.

---

## ✅ Project Summary

- **Purpose:**  
  Provide customers and supply chain partners access to detailed product information for each solar panel via a QR code.

- **Current Solution:**  
  - Static HTML pages hosted on GitHub Pages
  - One HTML page per panel serial number
  - QR codes link directly to these pages

---

## ✅ Steps Completed So Far

✅ Created a GitHub repository for DPP pages  
✅ Enabled GitHub Pages for free static website hosting  
✅ Developed an HTML template for DPP:
  - Technical specs
  - CO₂ footprint
  - Warranty info
  - Material composition
  - Datasheet download link
✅ Uploaded first DPP HTML page:
  - Example serial: BS-2025-0001  
✅ Uploaded company logo to repo and embedded it in the page  
✅ Generated QR codes linking to DPP pages using QR Code Monkey  
✅ Verified QR codes work on mobile devices

---

## ✅ How to Add a New DPP Page

1. Duplicate the existing HTML page (e.g. `index.html`)  
2. Update:
    - Serial number
    - Panel specifications
    - Any unique data
3. Save as:
    ```
    BS-2025-0002.html
    ```
4. Commit changes to `main` branch
5. Verify the new page loads:
    ```
    https://yourusername.github.io/biosphere-dpp/BS-2025-0002.html
    ```
6. Generate a new QR code pointing to the new page URL

---

## ✅ Next Steps (Future Improvements)

- Automate HTML page generation from CSV/JSON data
- Move DPP pages to our company domain for a professional look:
- Consider dynamic solution:
- Database for panel records
- Real-time updates
- Supply chain data integration
- Explore blockchain integration for traceability

---

## ✅ Helpful Links

- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [QR Code Monkey](https://www.qrcode-monkey.com)
- [Adobe QR Code Generator](https://www.adobe.com/express/feature/image/qr-code-generator)

---

## ✅ Contact

For questions or contributions, contact Abdul at abdul_a@biosphere.solar.


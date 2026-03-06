# Architecture Notes – PDF Splitter (Python .exe)

> **Context:** Side project built as a Logistics Analyst to drive operational efficiency for a Logistics Department.
>
> **Purpose:** Explain how the application is designed and how data flows through it at a high level (safe to share publicly). These notes are intended for employers/reviewers and avoid proprietary customer data, internal identifiers, and sensitive document samples.

---

## 1) System Overview

The PDF Splitter is a **desktop utility** packaged as a **Windows executable (.exe)**. It processes large consolidated PDF packets (often hundreds of pages) containing **multiple shipments** and mixed document types (e.g., **Invoices**, **Bills of Lading (BOL)**, **AES shipment records**). The app classifies pages, extracts document identifiers, groups pages by shipment, and exports **shipment-specific folders** with **standardized filenames**.

**Primary goals:**
- Reduce manual splitting/renaming effort
- Standardize shipment documentation storage
- Improve traceability and reduce misfiling risk

---

## 2) High-Level Architecture (Components)

### UI Layer (User Actions)
- **Create Folders** button
- **Select/Upload PDFs** (file picker)
- **Split Docs** button
- Status messages and completion dialog

### Processing Layer (Core Logic)
- PDF page iteration
- OCR/text extraction via a PyPDF-based workflow
- Keyword-based document classification (Invoice / BOL / AES)
- Regex-based identifier extraction (Invoice # / B/L # / AES ID)
- Shipment grouping (“bundling”)

### Output Layer (File System & Export)
- Folder creation and management
- Intermediate bundle PDF creation
- Final split document export
- Identifier-based file naming

---

## 3) User Workflow (End-to-End)

1. User launches the **.exe**
2. User clicks **Create Folders**
   - Creates the folder structure:
     - `Uploaded Docs`
     - `Bundles`
     - `Split Docs`
3. User clicks **Select/Upload PDFs** and chooses one or more PDF packets via the file picker
4. User clicks **Split Docs**
5. Application processes each selected PDF and exports results to `Split Docs`

---

## 4) Folder Pipeline Design (Traceability)

The app uses a **three-stage folder pipeline** to keep processing transparent and easy to troubleshoot:

- **`Uploaded Docs`**
  - Holds copies of user-selected input PDFs
  - Ensures consistent processing location and preserves originals

- **`Bundles`**
  - Holds intermediate per-shipment “bundle” PDFs
  - Useful for debugging (you can inspect what pages were grouped into a shipment)
  - Allows re-processing steps without re-selecting original files

- **`Split Docs`**
  - Final output location
  - Contains a folder for each shipment
  - Each shipment folder contains split PDFs (Invoice/BOL/AES) named using extracted identifiers

---

## 5) Document Understanding Strategy

### 5.1 Text Extraction (OCR/Text)
- Each page is processed through an **OCR/text extraction** workflow (PyPDF-based), producing a text representation of the page.
- This supports real-world packets where some pages may be scanned or contain image-based text.

### 5.2 Document Classification (Keyword-First)
- The extracted text is scanned for **document-type keywords** to classify each page as:
  - **Invoice**
  - **BOL**
  - **AES**
  - (Optional/future) **Unknown**

**Why keyword-first?**
- Robust to varied page ordering
- Avoids assumptions like “invoice is always page 1”
- Improves reliability when packets contain mixed document layouts

### 5.3 Identifier Extraction (Regex)
- After classification, the app applies **regex patterns** to extract the unique identifier used in filenames:
  - Invoice → **Invoice number**
  - BOL → **B/L number**
  - AES → **AES identifier**

**Multiple invoices per shipment:**
- Supported naturally because each invoice page set is detected and exported with its own invoice number.

---

## 6) Shipment Grouping (“Bundling”) Logic

Once pages are classified and identifiers are extracted, pages are grouped into **shipment-level bundles**.

**Conceptual approach:**
- Collect pages associated with a shipment (based on shipment references/IDs discovered during parsing)
- Write a bundle PDF per shipment into `Bundles`
- Use the bundle as the source for final splitting and naming

**Why bundling helps:**
- Provides an intermediate artifact to validate grouping
- Simplifies final split/export
- Makes troubleshooting easier for edge cases

---

## 7) Splitting & Naming (Export Rules)

### Splitting
- For each shipment bundle:
  - Split invoice pages into one or more invoice PDFs
  - Split BOL pages into a BOL PDF
  - Split AES pages into an AES PDF

### Naming
- Output filenames include the extracted identifier for quick lookup and compliance-friendly organization.
- Example pattern (illustrative):
  - `Invoice_<INVOICE_NUMBER>.pdf`
  - `BOL_<BL_NUMBER>.pdf`
  - `AES_<AES_ID>.pdf`

---

## 8) Packaging & Deployment

The application is packaged as a **Windows executable (.exe)** to:
- Remove the need for Python installation for end users
- Support non-technical workflows in operations/logistics environments
- Improve adoption and portability

---

## 9) Error Handling & Operational Considerations (Portfolio-Safe)

Current behavior is designed to be operationally friendly:
- UI status updates communicate progress (folder creation, upload complete, split complete)
- Intermediate `Bundles` outputs provide a checkpoint for investigation

Recommended enhancements (often discussed in interviews):
- **Run summary log** (CSV/TXT): shipments detected, documents created, exceptions
- **Unknown Pages** routing when classification fails
- OCR confidence thresholds and flagging for manual review
- Validation rules (e.g., required documents per shipment)

---

## 10) Security & Data Privacy

Because shipping/compliance documents can contain sensitive information:
- Portfolio artifacts should be **redacted** (names, addresses, account numbers, internal IDs)
- Architecture notes focus on **design and logic**, not proprietary documents or customer data

---

## 11) Suggested Repo Placement

Place this file in:

```
/pdf-splitter/architecture-notes.md
```

Related assets:
- `/pdf-splitter/README.md` – business problem, solution summary, impact
- `/pdf-splitter/screenshots.pdf` – UI walkthrough (redacted)
- `/pdf-splitter/flowchart.png` – process diagram

---

## 12) Interview Talking Points (Quick)

- Why you chose **keyword-first classification** (robust to ordering)
- How regex provides **deterministic identifiers** for naming
- Why `Bundles` improves traceability and reduces support burden
- Why packaging as an `.exe` increases adoption
- How you measured impact (**~15 hours/month saved**)


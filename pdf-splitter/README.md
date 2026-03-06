# PDF Splitter (Python .exe) — Multi‑Shipment Document Automation

**Role Context:** Logistics Analyst (Operational Efficiency / Process Excellence)  
**Project Type:** Side project for a Logistics Department  
**Primary Tech:** Python, PyPDF (OCR/text extraction workflow), Regex patterns  
**Deliverable:** Windows executable (.exe) for non-technical users  
**Business Impact:** ~15 hours/month saved

**Note:** This app was created using company resources, I can post psuedo code but not the code itself and redacted screenshots of how it operates.

---

## Overview
This application automates the processing of large consolidated PDF packets (often **hundreds of pages**) that contain documentation for **multiple shipments**. These packets commonly include:

- **Commercial Invoices** (identified by **invoice number**)
- **Bills of Lading (BOL)** (identified by **B/L number**)
- **AES shipment records** (identified by **AES identifier**)

The tool automatically classifies pages, extracts the correct identifiers, splits the packet into individual shipment documents, and saves everything into a structured folder system with **compliance-ready naming conventions**.

---

## The Problem
Before this automation, the team manually:
- searched through a large PDF to locate pages for each shipment,
- determined document type (Invoice / BOL / AES),
- split PDFs into separate files,
- created shipment folders,
- renamed each file using the correct identifier.

This was repetitive, time-consuming, and increased the risk of misfiling shipment-critical documents.

---

## The Solution
I built a Python desktop utility and packaged it as a **Windows executable (.exe)** so users can run it without installing Python.

### User Workflow (UI-driven)
1. Launch the **PDF Splitter (.exe)**
2. Click **Create Folders**
   - Creates:
     - `Uploaded Docs`
     - `Bundles`
     - `Split Docs`
3. Click **Select PDF Files** and choose one or more input PDFs
4. Click **Split Docs**
5. The application processes and exports results automatically

---

## How It Works (High-Level Logic)

### 1) OCR/Text Extraction
The app uses an OCR/text-extraction workflow (via **PyPDF**) to obtain page text from the PDF packet.

### 2) Document Type Classification (Keyword Detection)
Each page is scanned for key terms to classify it as:
- Invoice
- Bill of Lading (BOL)
- AES shipment record

This approach is resilient to document ordering because classification is based on page content rather than fixed page positions.

### 3) Identifier Extraction (Regex)
After the document type is known, regex patterns extract the **unique identifier** used for naming:
- Invoice → invoice number
- BOL → B/L number
- AES → AES identifier

✅ **Multiple invoices per shipment** are supported automatically because each invoice number is detected and exported independently.

### 4) Shipment Bundling → Splitting → Export
Pages are grouped by shipment and written as intermediate bundle PDFs, then split into final documents and exported into shipment folders with standardized filenames.

---

## Output Folder Pipeline
The automation creates and uses a three-stage folder pipeline for traceability and easier troubleshooting:

- **Uploaded Docs**  
  Stores the input PDFs selected by the user

- **Bundles**  
  Intermediate per-shipment PDFs used to group pages before final splitting

- **Split Docs**  
  Final per-shipment folders containing the split + renamed documents

---

## Example Output Structure

```text
/Uploaded Docs
  shipping_packet_March.pdf

/Bundles
  Shipment_00123_bundle.pdf
  Shipment_00124_bundle.pdf

/Split Docs
  /Shipment_00123
     Invoice_INV-10492.pdf
     Invoice_INV-10493.pdf
     BOL_BL-778210.pdf
     AES_AES-5X19A2.pdf

  /Shipment_00124
     Invoice_INV-10494.pdf
     BOL_BL-778211.pdf
     AES_AES-5X19A3.pdf

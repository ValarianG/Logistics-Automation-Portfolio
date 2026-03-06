# Logistics-Automation-Portfolio
Automations in Logistics

# Logistics Automation Portfolio

**Role:** Logistics Analyst | Automation & Operational Excellence  
**Focus:** Eliminating manual work, standardizing logistics documentation, and improving process reliability  
**Tools:** Python, Power Automate, Excel

---

## 📌 About This Portfolio
This repository contains a collection of **automation projects** I developed to improve efficiency, accuracy, and scalability within logistics and operations workflows.

My work focuses on:
- Reducing manual, repetitive tasks
- Improving shipment documentation control
- Standardizing outputs for compliance and auditability
- Delivering measurable operational impact

These automations were built as **side projects supporting my Logistics Department**, with a strong emphasis on real-world usability, business impact, and non-technical user adoption.

---

## 🧠 Core Skills Demonstrated
- Logistics process analysis & optimization
- Document automation (Invoices, Bills of Lading, AES records)
- OCR & text extraction
- Regex-based identifier detection
- Workflow automation & orchestration
- File system design & standardization
- Automation packaging for business users

---

## 🛠 Tools & Technologies
- **Python**  
  Used for document processing, OCR-backed text extraction, PDF splitting, regex identifier detection, and desktop automation (Windows executables).

- **Power Automate**  
  Used for workflow automation, notifications, and process orchestration across systems and teams.

- **Microsoft Excel**  
  Used for structured data storage, reporting outputs, and integration with automated workflows.

---

## 📂 Portfolio Projects

### 1️⃣ PDF Splitter – Multi‑Shipment Document Automation (Python)
**Location:** `/pdf-splitter`

**Problem:**  
Logistics documentation arrived as large, consolidated PDF packets (often hundreds of pages) containing multiple shipments and mixed document types (Invoices, Bills of Lading, AES records). Manual splitting and renaming was time-consuming and error-prone.

**Solution:**  
Built a Python-based desktop application (packaged as a Windows `.exe`) that:
- Uses OCR-backed text extraction
- Classifies pages by document type via keyword detection
- Extracts invoice numbers, B/L numbers, and AES identifiers using regex
- Automatically creates shipment-level folders
- Splits and renames documents using standardized naming conventions

**Impact:**  
✅ Saved approximately **15 hours per month**  
✅ Reduced document misfiling risk  
✅ Standardized shipment documentation structure  

**Artifacts Included:**  
- Case study documentation  
- Process flowchart  
- Folder pipeline explanation  
- Redacted examples (no sensitive data)

---

## 🗂 Repository Structure
```text
logistics-automation-portfolio/
├── README.md                ← Portfolio overview (this file)
├── pdf-splitter/
│   ├── README.md            ← Detailed case study
│   ├── flowchart.png
│   └── architecture-notes.md
└── future-projects/

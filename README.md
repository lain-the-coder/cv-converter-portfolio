# CV Converter

**A Streamlit-based CV template conversion tool that uses Google Gemini AI to extract candidate data from CVs and reformat them into a standardized company template.**

Upload one CV or fifty - the system reads each one, extracts all relevant information, and outputs formatted documents in your company's standard template format.

---

> ## 📌 About This Repository
>
> This is a **public showcase repository** containing documentation only. The complete source code is maintained in a **private repository** for client confidentiality.
>
> **What's here:** Project overview, features, architecture, and high-level documentation.  
> **What's not here:** Source code, tests, and proprietary implementation details.
>
> 📸 **Screenshots and demo coming soon** - watch this space!
>
> For licensing inquiries, partnership opportunities, or access requests, please reach out via the contact information at the bottom of this README.

---

## Table of Contents

1. [Overview](#overview)
2. [Features](#features)
3. [Architecture](#architecture)
4. [Tech Stack](#tech-stack)
5. [How It Works](#how-it-works)
6. [Template System](#template-system)
7. [Quality Assurance](#quality-assurance)
8. [Use Cases](#use-cases)
9. [Project Highlights](#project-highlights)
10. [Contact](#contact)

---

## Overview

The CV Converter is an internal HR tool that automates the tedious process of reformatting candidate CVs into a standardized company template. Instead of manually copy-pasting candidate information into a template, HR teams can:

1. Upload the company template
2. Upload one or more candidate CVs
3. Get back perfectly formatted documents ready for review

The tool intelligently extracts structured data from CVs regardless of their original format, then fills in the company template automatically.

---

## Features

- **Multi-format support** - Accepts PDF, DOCX, and TXT candidate CVs
- **Batch processing** - Convert multiple CVs in a single run
- **Smart data extraction** - AI-powered extraction of names, experiences, education, certifications
- **Multiple roles handling** - Correctly handles candidates with multiple positions at the same company
- **Automatic formatting** - Normalizes dates, names, and locations to consistent formats
- **Empty section cleanup** - Removes unused template sections automatically
- **Authentication** - Company email and password protection
- **Session management** - Automatic session timeout for security
- **ZIP download** - Bulk download all converted CVs as a single archive
- **Token-based templates** - Easy template customization with placeholder system

---

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                   Streamlit Web UI                       │
│            (Authentication & File Uploads)               │
└────────────────────────┬────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────┐
│              Text Extraction Layer                       │
│         (PDF / DOCX / TXT Processing)                    │
└────────────────────────┬────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────┐
│            AI Extraction (Google Gemini)                 │
│         (Structured data extraction)                     │
└────────────────────────┬────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────┐
│           Template Filling & Cleanup                     │
│                  (Document generation)                   │
└────────────────────────┬────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────┐
│              Download (Individual / ZIP)                 │
└─────────────────────────────────────────────────────────┘
```

---

## Tech Stack

| Component         | Technology              |
| ----------------- | ----------------------- |
| **Language**      | Python 3.11             |
| **Web Framework** | Streamlit               |
| **AI/ML**         | Google Gemini API       |
| **Testing**       | pytest                  |
| **Document Processing** | Industry-standard Python libraries |

---

## How It Works

### The Conversion Pipeline

1. **Upload** - HR uploads company template and candidate CVs
2. **Extract** - System reads and parses content from CVs
3. **Process** - AI extracts structured data (names, experiences, education, etc.)
4. **Validate** - Data is validated and normalized to consistent formats
5. **Fill** - Template tokens are replaced with extracted data
6. **Clean** - Unused sections are automatically removed
7. **Deliver** - Formatted documents are ready for download

### Key Capabilities

The system intelligently handles:

- **Various date formats** - Normalizes different conventions to a consistent format
- **Name variations** - Handles ALL CAPS, mixed case, and other variations
- **Complex career histories** - Correctly separates multiple roles at the same company
- **Missing information** - Provides sensible defaults for missing fields
- **Large CVs** - Supports candidates with extensive work histories
- **Corrupted files** - Gracefully handles malformed input without crashing

---

## Template System

The system uses a **token-based template approach** where placeholders in the company template get replaced with extracted candidate data.

### Template Capacity

The default template supports:

| Section          | Capacity     |
| ---------------- | ------------ |
| Work experiences | 20           |
| Responsibilities | 30 per role  |
| Education entries| 5            |
| Certifications   | 10           |

Templates can be customized with any branding, fonts, colors, layouts, or company logos - the conversion system adapts to whatever styling the template uses.

---

## Quality Assurance

### Test Suite

The project maintains a comprehensive test suite with **31 automated tests** achieving **100% pass rate**:

| Test Category           | Tests | Coverage                         |
| ----------------------- | ----- | -------------------------------- |
| Formatting              | 6     | Date/name normalization          |
| Data Validation         | 6     | AI output structure validation   |
| Template Processing     | 6     | Token replacement logic          |
| Critical Features       | 3     | Multiple roles, file safety      |
| Edge Cases              | 4     | Boundary conditions              |
| Production Critical     | 4     | File extraction, error handling  |
| Pipeline Integration    | 2     | End-to-end data flow             |

### Quality Metrics

- ✅ 100% test pass rate
- ✅ Fast test execution (~20 seconds)
- ✅ Production-grade error handling
- ✅ Comprehensive documentation
- ✅ Modular code architecture

---

## Use Cases

### Primary Use Case: HR Departments

- Process incoming candidate applications at scale
- Standardize CVs from multiple sources for hiring managers
- Maintain consistent presentation across all candidates
- Reduce manual reformatting time from hours to seconds

### Industries Served

- 🧬 **Pharmaceutical** - Strict compliance and formatting requirements
- 🔬 **Biotech** - Technical role-heavy candidate pools
- 🏥 **Healthcare** - High-volume recruitment
- 💼 **General HR** - Any organization with template standardization needs

---

## Project Highlights

### Smart Multi-Role Handling

One of the trickier challenges - when a candidate has worked **multiple roles at the same company**, the system correctly creates separate entries instead of consolidating them. This preserves the candidate's career progression and is critical for accurate evaluation.

### Intelligent Document Cleanup

If a candidate has fewer experiences than the template supports, the system automatically removes empty sections - leaving a clean, professional document with no awkward placeholders or blank rows.

### Robust Error Handling

- Gracefully handles corrupted or malformed files
- Falls back to filename when name extraction fails
- Preserves data integrity throughout the pipeline
- Won't crash on unexpected input formats

### Scalable Processing

- Single CV: ~5 seconds
- Batch of 25 CVs: ~30 seconds
- Batch download as ZIP for efficiency
- Real-time progress tracking

---

## Project Status

**Current Version:** 1.0  
**Status:** Production-ready, deployed to clients  
**Test Coverage:** 31 automated tests, 100% pass rate  
**Documentation:** Complete (Setup, Usage, Testing, Troubleshooting)

---

## Future Enhancements

Potential improvements being considered:

- [ ] OCR support for scanned PDFs
- [ ] Multi-language CV support
- [ ] Custom template builder UI
- [ ] Cloud deployment options (AWS/Azure/GCP)
- [ ] Analytics dashboard for HR teams
- [ ] ATS (Applicant Tracking System) integrations
- [ ] Bulk template management

---

## Contact

For licensing inquiries, partnership opportunities, source code access, or to discuss a custom implementation for your organization:

- 📧 **Email:** ehab.ahmedsj@gmail.com
- 💼 **LinkedIn:** [linkedin.com/in/ehabahmed2000](https://www.linkedin.com/in/ehabahmed2000/)
---

_This is a public showcase repository. The complete source code, implementation details, and test suite are maintained privately for client confidentiality._

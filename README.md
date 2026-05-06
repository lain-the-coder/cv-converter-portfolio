# CV Converter

**A Streamlit-based CV template conversion tool that uses Google Gemini AI to extract candidate data from CVs and reformat them into a standardized company template.**

Upload one CV or fifty - the system reads each one, extracts all relevant information, and outputs formatted documents in your company's standard template format.

---

> ## 📌 About This Repository
>
> This is a **public showcase repository** containing documentation only. The complete source code is maintained in a **private repository** for client confidentiality.
>
> **What's here:** Project overview, features, architecture, setup guide, and technical documentation.  
> **What's not here:** Source code, tests, and proprietary implementation details.
>
> 📸 **Screenshots and demo coming soon** - watch this space!
>
> For licensing inquiries, partnership opportunities, or access requests, please reach out via the contact information at the bottom of this README.

---

## Table of Contents

1. [Overview](#overview)
2. [Features](#features)
3. [Project Structure](#project-structure)
4. [Prerequisites](#prerequisites)
5. [Installation & Setup](#installation--setup)
6. [Configuration](#configuration)
7. [Running the Application](#running-the-application)
8. [Testing](#testing)
9. [How It Works](#how-it-works)
10. [Template Guide](#template-guide)
11. [Code Documentation](#code-documentation)
12. [Customization](#customization)
13. [Troubleshooting](#troubleshooting)
14. [Contact](#contact)

---

## Overview

The CV Converter is an internal HR tool that automates the tedious process of reformatting candidate CVs into a standardized company template. Instead of manually copy-pasting candidate information into a template, HR teams can:

1. Upload the company template (with placeholder tokens)
2. Upload one or more candidate CVs (PDF, DOCX, or TXT)
3. Get back perfectly formatted documents ready for review

The tool uses Google Gemini AI to intelligently extract structured data from CVs regardless of their original format, then fills in the company template using a token replacement system.

---

## Features

- **Multi-format support** - Accepts PDF, DOCX, and TXT candidate CVs
- **Batch processing** - Convert multiple CVs in a single run
- **Smart data extraction** - AI-powered extraction of names, experiences, education, certifications
- **Multiple roles handling** - Creates separate entries for different roles at the same company
- **Automatic formatting** - Normalizes dates (MMM YYYY), names (Proper Case), and locations
- **Empty section cleanup** - Removes unused experience blocks automatically
- **Authentication** - Company email + password protection
- **Session management** - 30-minute session timeout for security
- **ZIP download** - Bulk download all converted CVs as a single archive
- **Token-based templates** - Easy template customization with `{{TOKEN}}` placeholders

---

## Project Structure

> **Note:** The structure below reflects the private repository. This public repo contains only this README.

```
CV-Converter-Final-App/
├── cv_converter.py              # Main Streamlit application
├── extraction.py                # AI extraction logic (Google Gemini)
├── utils.py                     # Helper functions (file I/O, formatting, template filling)
├── requirements.txt             # Python dependencies
├── runtime.txt                  # Python version specification
├── .gitignore                   # Git ignore rules
├── README.md                    # Documentation
├── TESTING.md                   # Test suite documentation
├── CV Template With Tokens.docx # Sample company template with tokens
├── Jane_Doe_Resume.docx         # Sample CV for testing
└── tests/                       # Automated test suite (31 tests)
    ├── __init__.py
    ├── test_formatting.py
    ├── test_extraction.py
    ├── test_template.py
    ├── test_critical_features.py
    ├── test_edge_cases.py
    ├── test_production_critical.py
    └── test_pipeline_integration.py
```

---

## Prerequisites

Before installing, ensure you have:

- **Python 3.11** (specified in `runtime.txt`)
- **Google Gemini API key** ([Get one here](https://aistudio.google.com/app/apikey))
- **Ubuntu/WSL** environment (for the setup commands below)
- **Internet connection** (for AI extraction calls)

---

## Installation & Setup

### Step 1: Install Python 3.11 (Ubuntu/WSL)

```bash
# Update package list
sudo apt update

# Install tools for adding extra repositories
sudo apt install software-properties-common -y

# Add the deadsnakes PPA for newer Python versions
sudo add-apt-repository ppa:deadsnakes/ppa -y

# Update package list with new repository
sudo apt update

# Install Python 3.11 with venv and dev headers
sudo apt install python3.11 python3.11-venv python3.11-dev -y

# Verify installation
python3.11 --version
# Expected output: Python 3.11.x
```

### Step 2: Create and Activate Virtual Environment

```bash
# Navigate to project directory
cd CV-Converter-Final-App

# Create virtual environment with Python 3.11
python3.11 -m venv venv

# Activate the virtual environment
source venv/bin/activate

# Verify Python version inside venv
python --version
# Expected output: Python 3.11.x
```

> **Note:** Always activate the virtual environment before running the app or tests. You'll see `(venv)` at the start of your terminal prompt when active.

### Step 3: Install Dependencies

```bash
# Upgrade pip to latest version
pip install --upgrade pip

# Install all project dependencies
pip install -r requirements.txt
```

This installs:

- `streamlit` - Web application framework
- `google-generativeai` - Gemini AI SDK
- `python-docx` - DOCX file processing
- `pdfplumber` & `PyPDF2` - PDF text extraction
- `pandas` & `numpy` - Data handling
- `pytest` - Testing framework
- `reportlab` - PDF generation (used in tests)

---

## Configuration

### Create the Secrets File

The application uses Streamlit's secrets management for sensitive configuration.

```bash
# Create the .streamlit directory
mkdir -p .streamlit

# Create the secrets file
nano .streamlit/secrets.toml
```

### Add Configuration

Paste the following into `secrets.toml`, replacing values with your own:

```toml
GEMINI_API_KEY = "your-gemini-api-key-here"
company_domain = "@yourcompany.com"
app_password = "YourSecurePassword123"
```

### Configuration Reference

| Key              | Purpose                                 | Example              |
| ---------------- | --------------------------------------- | -------------------- |
| `GEMINI_API_KEY` | Google Gemini API key for AI extraction | `"AIzaSyD..."`       |
| `company_domain` | Email domain restriction for login      | `"@yourcompany.com"` |
| `app_password`   | Shared password for team access         | `"SecurePass2024!"`  |

> **Security Note:** Never commit `secrets.toml` to version control. It's already in `.gitignore`.

---

## Running the Application

```bash
# Make sure virtual environment is activated
source venv/bin/activate

# Start the Streamlit app
streamlit run cv_converter.py
```

The app will open at **http://localhost:8501** in your browser.

### Usage Flow

1. **Login** - Enter your company email and password
2. **Upload Template** - Use `CV Template With Tokens.docx` (or your custom template)
3. **Upload CVs** - Drop one or more candidate CVs (PDF, DOCX, or TXT)
4. **Click "Convert CVs"** - Wait for AI processing
5. **Download** - Get individual files or download all as ZIP

### Testing the Setup

Use the included sample files to verify everything works:

- Template: `CV Template With Tokens.docx`
- Test CV: `Jane_Doe_Resume.docx`

---

## Testing

The project includes **31 automated tests** with 100% pass rate.

### Run All Tests

```bash
# Activate virtual environment
source venv/bin/activate

# Run the full test suite
pytest tests/ -v
```

Expected output: `31 passed in ~20s`

### Run Specific Test Categories

```bash
pytest tests/test_formatting.py -v          # Date/name formatting
pytest tests/test_extraction.py -v          # Data validation
pytest tests/test_template.py -v            # Token replacement
pytest tests/test_critical_features.py -v   # Multiple roles, file safety
pytest tests/test_edge_cases.py -v          # Boundary conditions
pytest tests/test_production_critical.py -v # PDF, error handling
pytest tests/test_pipeline_integration.py -v # End-to-end flow
```

### Quick Test (Minimal Output)

```bash
pytest tests/ -q
```

For complete test documentation, see TESTING.md (in the private repository).

---

## How It Works

### The Conversion Pipeline

```
[CV Upload] → [Text Extraction] → [AI Extraction] → [Validation] → [Template Filling] → [Cleanup] → [Download]
```

1. **Text Extraction** (`utils.py::extract_text`)
   - PDF: Uses `pdfplumber` to extract text from each page
   - DOCX: Uses `python-docx` to read paragraphs
   - TXT: Direct UTF-8 decode

2. **AI Extraction** (`extraction.py::CVExtractor.extract`)
   - Sends CV text to Google Gemini with detailed prompt
   - Receives structured JSON response
   - Handles edge cases (multiple roles, missing names, etc.)

3. **Data Validation** (`extraction.py::CVExtractor._validate_data`)
   - Ensures all required fields exist
   - Formats names from ALL CAPS to Proper Case
   - Normalizes dates to MMM YYYY format
   - Adds defaults for missing fields

4. **Template Filling** (`utils.py::fill_template`)
   - Replaces `{{TOKEN}}` placeholders with extracted data
   - Applies bold formatting to company names
   - Sets consistent fonts (Arial 10pt) and line spacing (1.5)

5. **Cleanup**
   - Removes table rows for unused experience slots
   - Deletes empty bullet points
   - Replaces missing data with fallback text

6. **Download**
   - Individual download per candidate
   - ZIP archive option for batch downloads

---

## Template Guide

### Understanding Tokens

Templates use **double curly brace tokens** that get replaced with extracted data:

| Token                  | Replaced With               |
| ---------------------- | --------------------------- |
| `{{CANDIDATE_NAME}}`   | Candidate's full name       |
| `{{POSITION}}`         | Current/most recent role    |
| `{{EMAIL}}`            | Email address               |
| `{{PHONE}}`            | Phone number                |
| `{{INTRO_PARAGRAPH}}`  | Professional summary        |
| `{{EXP1_COMPANY}}`     | First company name          |
| `{{EXP1_ROLE}}`        | First job title             |
| `{{EXP1_DURATION}}`    | First role dates            |
| `{{EXP1_LOCATION}}`    | First role location         |
| `{{EXP1_RESP1}}`       | First responsibility bullet |
| `{{EDU1_INSTITUTION}}` | First university name       |
| `{{EDU1_DEGREE}}`      | First degree                |
| `{{CERT1_NAME}}`       | First certification         |

### Template Capacity

The provided template supports:

| Section          | Maximum     | Token Pattern                   |
| ---------------- | ----------- | ------------------------------- |
| Work experiences | 20          | `{{EXP1_*}}` to `{{EXP20_*}}`   |
| Responsibilities | 30 per role | `{{EXP1_RESP1}}` to `_RESP30}}` |
| Education        | 5           | `{{EDU1_*}}` to `{{EDU5_*}}`    |
| Certifications   | 10          | `{{CERT1_*}}` to `{{CERT10_*}}` |

### Customizing the Template

You can freely modify the template's appearance:

- Change fonts, colors, and sizes
- Add company logos and headers
- Adjust spacing and margins
- Add sections with custom tokens

**Just keep the token names exactly as they are** - the code looks for these specific patterns.

### Adding More Capacity

To support more than 20 experiences:

1. **Update the template** - Add `{{EXP21_COMPANY}}`, `{{EXP21_ROLE}}`, etc.
2. **Update `utils.py`** - Find `range(1, 21)` in `fill_template()` and change to `range(1, 22)` (or higher)

To support more responsibilities per role:

1. **Update the template** - Add `{{EXP1_RESP31}}`, etc.
2. **Update `utils.py`** - Find `range(1, 101)` and increase the upper bound

---

## Code Documentation

### Module Overview

#### `cv_converter.py` - Main Application

- Streamlit UI and page configuration
- Authentication (`check_company_email`, `check_session_timeout`)
- File upload handling
- Conversion orchestration
- Download generation (individual + ZIP)

#### `extraction.py` - AI Processing

- `CVExtractor` class wraps Google Gemini API
- Detailed prompt engineering for accurate extraction
- JSON response parsing and validation
- Special handling for:
  - Multiple roles at same company (creates separate entries)
  - Names vs. job titles (avoids confusion)
  - Location extraction (separate from company name)
  - Date normalization (handles "Sep-2015", "09/2015", etc.)

#### `utils.py` - Helper Functions

- `extract_text()` - Multi-format file reading
- `format_date()`, `format_duration()`, `format_name()` - Text normalization
- `fill_template()` - Core token replacement logic
- `safe_filename()` - Sanitizes filenames for downloads
- `mask_api_key()` - For displaying API keys safely

### Key Configuration Values

In `utils.py::fill_template()`:

| Range           | Controls                     | Default |
| --------------- | ---------------------------- | ------- |
| `range(1, 21)`  | Max work experiences         | 20      |
| `range(1, 101)` | Max responsibilities per job | 100     |
| `range(1, 6)`   | Max education entries        | 5       |
| `range(1, 11)`  | Max certifications           | 10      |

---

## Customization

### Changing AI Model

In `extraction.py` (line ~12):

```python
# Current (faster, cheaper)
self.model = genai.GenerativeModel("gemini-flash-latest")

# Alternative (more accurate, more expensive)
self.model = genai.GenerativeModel("gemini-pro-latest")
```

### Changing Fallback Text

In `utils.py::fill_template()`:

```python
"Location Not Specified"   # When location is missing
"Dates Not Available"      # When education dates are missing
"Year Not Available"       # When certification year is missing
"Provider Not Specified"   # When certification provider is missing
```

Modify these to match your preferences.

### Changing Date Format

In `utils.py::format_date()`, modify the patterns to output different formats. The default produces `JAN 2023` style.

### Changing Session Timeout

In `cv_converter.py::check_session_timeout()`:

```python
if elapsed.total_seconds() > 1800:  # Default: 30 minutes (1800 seconds)
```

Change `1800` to your preferred timeout in seconds.

### Disabling Authentication

To remove authentication, comment out the authentication check in `cv_converter.py::main()`:

```python
def main():
    # if not check_company_email():
    #     return
    # check_session_timeout()
    # ... rest of the function
```

### Improving Extraction Accuracy

If the AI is missing information or extracting incorrectly:

1. **Edit the prompt** in `extraction.py::CVExtractor.extract()` (starts around line 22)
2. **Add specific examples** for your CV formats
3. **Try a more powerful model** (Gemini Pro vs Flash)
4. **Adjust temperature** in `self.cfg` (lower = more deterministic)

---

## Troubleshooting

### Login Issues

**Problem:** Cannot log in or "Access restricted" error

**Solutions:**

- Verify `.streamlit/secrets.toml` exists in the project root
- Check that `company_domain` includes the `@` symbol (e.g., `"@company.com"`)
- Ensure password matches exactly (no trailing spaces)
- Restart Streamlit after editing secrets: `Ctrl+C` then `streamlit run cv_converter.py`

### Extraction Quality Issues

**Problem:** AI extracts wrong information or misses fields

**Solutions:**

- Edit the AI prompt in `extraction.py` (lines ~22-200) with more specific instructions
- Add examples of your specific CV formats to the prompt
- Switch to `gemini-pro-latest` for better accuracy (slower, more expensive)
- Verify CVs are text-based, not scanned images
- Check that PDFs aren't password-protected

### Template Not Filling Correctly

**Problem:** Tokens remain unfilled or appear in wrong places

**Solutions:**

- Verify token names match exactly (case-sensitive): `{{EXP1_ROLE}}` not `{{exp1_role}}`
- Confirm double curly braces: `{{TOKEN}}` not `{TOKEN}`
- Check no spaces inside tokens: `{{EXP1_ROLE}}` not `{{ EXP1_ROLE }}`
- Ensure template is saved as `.docx` (not `.doc`)
- Make sure tokens aren't split across multiple text runs (rewrite the line)

### File Upload Issues

**Problem:** PDFs upload but extract no text

**Solutions:**

- Verify the PDF contains actual text, not scanned images
- Try opening the PDF and using Ctrl+A to select all - if you can't select text, it's a scan
- For scanned PDFs, OCR the file first using a tool like Adobe Acrobat or `ocrmypdf`

### Empty Sections Not Disappearing

**Problem:** Unused experience sections remain in output

**Solutions:**

- The system deletes complete experience blocks (EXP1-20) when no data exists
- Verify your template structure matches the expected format
- Check that experience tokens are in table rows (not just paragraphs)
- Ensure each experience's tokens are grouped together in the template

### "Module Not Found" Errors

**Problem:** `ModuleNotFoundError` when running tests or app

**Solutions:**

- Activate the virtual environment: `source venv/bin/activate`
- Reinstall dependencies: `pip install -r requirements.txt`
- Check Python version: `python --version` (should be 3.11.x)

### API Errors

**Problem:** "Extraction error" messages during conversion

**Solutions:**

- Verify your Gemini API key is valid: test it at https://aistudio.google.com
- Check API quota at Google AI Studio
- Ensure internet connectivity
- Try one CV at a time to isolate problematic files

---

## Contact

For licensing inquiries, partnership opportunities, source code access, or to discuss a custom implementation for your organization:

- 📧 **Email:** ehab.ahmedsj@gmail.com
- 💼 **LinkedIn:** https://www.linkedin.com/in/ehabahmed2000/

---

_This is a public showcase repository. The complete source code is maintained privately for client confidentiality._

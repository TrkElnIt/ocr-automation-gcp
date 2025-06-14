# Automated OCR and Data Pipeline with AI Parsing and Cloud Deployment

This document outlines the complete setup of the OCR automation system deployed on a Google Cloud VM instance. The system extracts address information from death certificate images using Google Vision API and parses structured address fields using the Gemma 2B model via Ollama. The results are appended to a Google Sheet, and a notification email is sent upon completion. A scheduled cron job ensures the task runs Monday to Friday at 10PM MST.

---

## 1. VM Environment

* **VM Name:** `death-ocr-vm`
* **Region/Zone:** `us-south1-a`
* **Machine Type:** `e2-standard-2` (2 vCPU, 8 GB RAM)
* **Python Virtual Environment:** `aienv`
* **Path to main script:** `/home/billing/main.py`

---

## 2. Core Stack

* **Python 3.10**
* **Playwright** (headless Chromium)
* **Google Cloud Vision API** (for OCR)
* **Ollama** with `gemma:2b` (for structured parsing)
* **Google Sheets API** (to append results)
* **SMTP (Gmail App Password)** (for notification email)

---

## 3. Script Responsibilities (`main.py`)

* Open the [Adams County Recording Site](https://recording.adcogov.org/LandmarkWeb/Home/Index)
* Perform document search for "DEATH CERTIFICATE"
* For each result:

  * Download the image
  * Run OCR using Google Vision
  * Send OCR result to `gemma:2b` via Ollama
  * Parse: `RESIDENCE - STREET AND NUMBER`, `CITY OR TOWN`, `RESIDENCE STATE`, `ZIP CODE`
  * Append to Google Sheet (tab: `DC-location`)
* At the end of all rows:

  * Send email to `billing@storyhomegroup.com`

---

## 4. Google Sheet

* **Sheet Name:** Death Certificates OCR Log
* **Spreadsheet ID:** `1cGuDwEf6_3ai1u0CPr5gMouBJKVQf3Zr8a9YGhYNOmE`
* **Tab Name:** `DC-location`
* **Service Account Access:**

  * Service account `175701717545-compute@developer.gserviceaccount.com` is added as `Editor`

---

## 5. Authentication

* **Google Vision API:** Authenticated via VM's default service account
* **Google Sheets API:**

  * Required OAuth scope: `https://www.googleapis.com/auth/spreadsheets`
  * Verified via metadata endpoint:

    ```bash
    curl -H "Metadata-Flavor: Google" http://metadata.google.internal/computeMetadata/v1/instance/service-accounts/default/scopes
    ```
* **Gmail App Password:**

  * App password generated and used for sending email through `smtp.gmail.com`

---

## 6. Cron Job Setup

* Command used:

```cron
0 5 * * 1-5 /home/billing/aienv/bin/python3 /home/billing/main.py >> /home/billing/cron_log.txt 2>&1
```

* **Runs:** 10PM MST (5AM UTC), Monday to Friday
* **Logs:** `/home/billing/cron_log.txt`
* **Status Check:**

```bash
crontab -l
cat /home/billing/cron_log.txt
```

---

## 7. Troubleshooting Tips

* **403 Insufficient Scopes:** Ensure VM has both `cloud-platform` and `spreadsheets` scopes.
* **App password errors:** Use correct email/password combo and `smtp.gmail.com`, port `587`
* **OCR failures:** Check Vision API quota or malformed images
* **No results:** Ensure the date filter on the site shows at least one certificate for yesterday

---

## 8. Extensions (Optional)

* Upload results to Google Drive
* Archive image snapshots daily
* Retry failed parses and keep error logs
* Upgrade to Gemini or Vertex AI when permissions are resolved

---

## 9. Portfolio Note

This project is included in Rafik Toudjine's professional portfolio to demonstrate automation and deployment expertise. The work covers end-to-end cloud scripting, Google Cloud API integration, and AI-powered data extraction.

### GitHub README Version

#### 📄 OCR + AI Parsing Automation Bot

An AI-powered automation tool that scrapes death certificate PDFs from a government portal, runs OCR using Google Vision API, extracts structured address fields with the `Gemma 2B` model via Ollama, appends the results to Google Sheets, and sends a daily summary email.

#### ✅ What It Does

* Opens the [Adams County Recording Site](https://recording.adcogov.org/LandmarkWeb/Home/Index)
* Filters for **DEATH CERTIFICATE** documents
* Downloads each document image
* Runs OCR using **Google Vision API**
* Uses **Gemma 2B** via **Ollama** to extract:

  * Street Address
  * City
  * State
  * ZIP Code
* Appends structured data to a Google Sheet
* Sends notification email when processing is complete
* Runs daily (Monday–Friday) via `cron` at **10PM MST**

#### 🖥 VM Configuration

* **Provider:** Google Cloud Platform (GCP)
* **VM Name:** `death-ocr-vm`
* **Zone:** `us-south1-a`
* **Machine Type:** `e2-standard-2` (2 vCPU, 8 GB RAM)
* **Environment:** Ubuntu 22.04 with Python virtualenv `aienv`
* **Automation Path:** `/home/billing/main.py`
* **Startup Method:** `cron` job scheduled at 5:00 UTC (10:00PM MST)

#### 🧠 Use Cases

* Structured parsing from scanned legal documents
* AI-enhanced data pipelines
* Government record analysis and reporting
* Serverless automation for back-office workflows

#### 🛠 Technologies

* Python 3
* Playwright (headless Chromium)
* Google Cloud Vision API
* Ollama with `gemma:2b`
* Google Sheets API
* SMTP (Gmail with App Password)
* Cron (for scheduled automation)
* Google Cloud VM (Ubuntu, 8GB RAM)

#### 🗂 Output

* Google Sheet (structured address log)
* Email confirmation summary
* Cron job logs (`cron_log.txt`)
* Image snapshots (optional per row)

#### 🔐 Access

🔒 Source code is private.
This project is showcased as a portfolio entry demonstrating OCR + LLM automation and Google Cloud integration.

#### 🧑‍💻 Author

**TrkElnIt** – Automation & AI-powered document intelligence solutions.



# OCR Address Extraction Bot - AI + GCP Integration

A cloud-deployed automation that scrapes death certificate images, runs OCR using Google Vision, extracts structured address fields with Gemma 2B via Ollama, appends results to Google Sheets, and sends a daily notification email.

---

## ✅ What It Does

* Navigates to a public recorder site
* Filters documents for "DEATH CERTIFICATE"
* Downloads image snapshots
* Uses **Google Vision API** for OCR
* Uses **Ollama + Gemma 2B** to extract:

  * Street Address
  * City
  * State
  * ZIP Code
* Appends rows to a **Google Sheet**
* Sends notification email when done
* Runs **automatically Mon–Fri at 10PM MST**

---

## 🧠 Use Cases

* AI-enhanced data extraction from legal PDFs
* Automated address parsing pipelines
* Government registry integration
* Cron-based document intelligence workflows

---

## 🛠 Technologies Used

* Python 3.10
* Playwright (headless automation)
* Google Cloud Vision API (OCR)
* Ollama with `gemma:2b` (LLM parsing)
* Google Sheets API
* Gmail SMTP (App Password)
* Google Cloud VM + Cron

---

## 🖥 VM Configuration

* **VM Name:** `death-ocr-vm`
* **Zone:** `us-south1-a`
* **Machine Type:** `e2-standard-2` (2 vCPU, 8GB RAM)
* **Python Env:** `aienv`
* **Main Script Path:** `/home/billing/main.py`
* **Runs via Cron:** 10PM MST, Monday–Friday

---

## 🔐 Access

🔒 Source code is private.

This is a showcase portfolio entry for AI + automation work deployed on GCP.

---

## 🧑‍💻 Author

**TrkElnIt** — Intelligent automation & cloud scraping solutions

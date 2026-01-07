# n8n Resume ATS Automation (Portfolio Project)

## Overview

This project demonstrates a lightweight Applicant Tracking System (ATS) built using **n8n**, **Google Drive**, **Google Sheets**, and a **Large Language Model (Gemini)** to automate first‑pass resume screening for a **Cybersecurity Analyst** role.

The system ingests resumes from a Drive folder, extracts candidate details using an LLM, assigns a score, and appends results to a single tracker sheet (`resume_score`) for recruiter review.

This repository is intended for **portfolio and demo purposes**.



---

## What this automation does

* Fetches Job Description (JD) and resumes from Google Drive
* Extracts text from PDF/DOCX resumes
* Uses an LLM prompt to parse candidate fields and compute a score
* Appends structured results into Google Sheets
* Allows a manual review/edit step before final submission

---

## Architecture (high level)

1. Manual trigger in n8n
2. Google Drive - download Job Description
3. Google Drive - list & download resumes
4. Extract text from resume files
5. LLM parsing & scoring (Gemini)
6. Manual QA step (Edit Fields)
7. Append row to `resume_score` Google Sheet

---

## Output schema (Google Sheet: `resume_score`)

The automation writes **exactly** the following columns:

* Candidate Name
* Email
* Phone
* Resume File
* Score
* Remarks

---

## LLM Prompt Used

```
You are an ATS resume parser.

From the resume text below, extract the following fields
and return ONLY valid JSON.

Fields:
- Candidate Name
- Email
- Phone
- Resume File
- Score
- Remarks

Rules:
- Output JSON only
- Do not add explanations
- Use empty string "" if not found

Resume text:
{{ $json.text }}
```

---

## How to run this demo locally

### Prerequisites

* n8n (self‑hosted or desktop)
* Google account
* Google Drive API credentials
* Google Sheets API access
* Google Gemini API key

---

### Step 1: Import workflow

1. Open n8n
2. Go to **Workflows → Import**
3. Import the provided workflow JSON file

> Note: This repository does **not** contain real resumes or credentials.

---

### Step 2: Configure credentials in n8n

Create the following credentials in n8n:

* Google Drive OAuth
* Google Sheets OAuth
* Google Gemini / Generative AI API key

Attach them to the respective nodes.

---

### Step 3: Prepare Google Drive folders

Create a Drive folder with:

* `/JD` → contains the job description file
* `/Resumes` → contains PDF/DOCX resumes
* `/Results` → contains Google Sheet named `resume_score`

Ensure the sheet has the columns listed above.

---

### Step 4: Run the workflow

1. Open the workflow
2. Click **Execute Workflow**
3. Monitor node execution
4. Verify rows are appended to `resume_score`

---

## Known limitations

* LLM API quota limits can cause `429` or rate‑limit errors during large batches
* Parsing accuracy depends on resume formatting quality
* Manual review is required for edge cases

Mitigations include batching, retries, throttling, and manual fallback via `Edit Fields`.

---

## Security & privacy

* No real resumes are committed to this repository
* All sample data should be anonymized or synthetic
* API keys are stored only in n8n credential manager

---

## Portfolio notes

This project demonstrates:

* Product thinking (problem → scope → metrics)
* Workflow automation with n8n
* LLM prompt design for structured extraction
* Practical handling of API limits and failures

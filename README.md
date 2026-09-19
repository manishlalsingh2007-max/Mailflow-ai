# 📬 Gmail → AI Document Processor (Make.com Blueprint)

An automated Make.com workflow that watches your Gmail inbox for emails with attachments, extracts and parses the documents using AI, classifies them, deduplicates, and logs them into Google Sheets.

---

## 🔄 Workflow Overview

```
Gmail (Watch Emails)
    └── List Attachments
            └── PDF Vector (Parse Document)
                    └── Aggregator (Combine Pages)
                            └── Gemini AI (Classify Document)
                                    └── Data Store (Check Duplicate)
                                            └── Data Store (Store Key)  [filter: new only]
                                                    └── Google Sheets (Save Row)
```

---

## ✨ What It Does

| Step | Module | Description |
|------|--------|-------------|
| 1 | **Gmail – Watch Emails** | Triggers when a new email with an attachment arrives |
| 2 | **Gmail – List Attachments** | Extracts all attachments from the email |
| 3 | **PDF Vector – Parse Document** | Converts PDF attachments to markdown text |
| 4 | **Aggregator** | Merges all attachment pages into one block |
| 5 | **Make AI Toolkit – Extract** | Uses Gemini AI to classify: client name, document type, period, financial year |
| 6 | **Data Store – Check Duplicate** | Checks if this document was already processed |
| 7 | **Data Store – Add Record** | Stores the document key (only for new documents) |
| 8 | **Google Sheets – Add Row** | Logs the document details into the "Received Documents" sheet |

---

## 📋 Prerequisites

Before importing this blueprint, make sure you have:

- A [Make.com](https://make.com) account (free or paid)
- The following connections set up in Make:
  - **Gmail** connection
  - **PDF Vector** connection (Make AI Content Extractor)
  - **Gemini AI** connection (via Make AI Toolkit)
  - **Google Sheets** connection
  - **Data Store** named `Client Document Deduplication`

---

## 🚀 How to Import

1. Download [`blueprint.json`](./blueprint.json)
2. Go to [Make.com](https://make.com) → **Scenarios**
3. Click **"Create a new scenario"**
4. Click the **three-dot menu (⋮)** in the bottom toolbar
5. Select **"Import Blueprint"**
6. Upload the `blueprint.json` file
7. Re-connect all your accounts (Gmail, Google Sheets, PDF Vector, Gemini AI)
8. Set up your Google Sheets spreadsheet with a sheet named **"Received Documents"**

---

## 📊 Google Sheets Format

Your Google Sheet must have a sheet named **"Received Documents"** with these column headers:

| Client Name | Document Type | Month / Period | Sender Email | Status | File Name | Received At |
|-------------|---------------|----------------|--------------|--------|-----------|-------------|

---

## 🗄️ Data Store Setup

Create a Data Store in Make named **`Client Document Deduplication`** with the following fields:

| Field Name | Type |
|------------|------|
| `gmailMessageId` | Text |
| `attachmentId` | Text |
| `filename` | Text |
| `receivedAt` | Date |
| `status` | Text |

---

## 🤖 AI Classification Fields

The Gemini AI model extracts the following fields from each document:

| Field | Description |
|-------|-------------|
| `client_company_name` | The client/company name from the document |
| `document_type` | Type: Bank Statement, Sales Invoice, Purchase Invoice, Expense Bill, etc. |
| `applicable_period` | Month, quarter, or financial period covered |
| `financial_year` | The financial year mentioned in the document |

---

## ⚠️ Notes

- The Gmail trigger watches for emails matching `has:attachment`
- Duplicate detection is based on: `gmail_message_id | md5(attachment_content)`
- Only **new (non-duplicate)** documents are written to Google Sheets
- The AI model used is **Gemini 3.8 Flash** (can be changed in the blueprint)

---

## 📁 Project Structure

```
make-gmail-document-processor/
├── blueprint.json      # The Make.com scenario blueprint
└── README.md           # This file
```

---

## 📄 License

MIT — feel free to use, modify, and share.

# MailFlow AI

### AI-Powered Email & Document Processing Workflow

MailFlow AI is an AI-powered document processing workflow that converts incoming email attachments into structured business data.

The system monitors Gmail for emails containing attachments, extracts document content, uses AI to classify and extract key information, performs duplicate detection, and automatically records the processed information in Google Sheets.

The project focuses on a practical engineering problem: turning unstructured documents received through email into structured, usable business data with minimal manual intervention.

---

## Why MailFlow AI?

A large amount of business information still arrives through email as PDFs and other documents.

A typical manual process looks like:

```text
Receive Email
     ↓
Download Attachment
     ↓
Open Document
     ↓
Read & Identify Information
     ↓
Classify Document
     ↓
Check Whether It Was Already Processed
     ↓
Enter Information Into Spreadsheet

MailFlow AI automates this workflow:

Gmail
  ↓
Attachment Detection
  ↓
Document Extraction
  ↓
AI Classification & Metadata Extraction
  ↓
Duplicate Detection
  ↓
Structured Data Storage
  ↓
Google Sheets

System Architecture:
                         MAILFLOW AI
                              │
                              ▼
                    ┌──────────────────┐
                    │      Gmail       │
                    │ Email + Attach.  │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │ Attachment       │
                    │ Retrieval        │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │ PDF Vector       │
                    │ Document Parser  │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │ Page Aggregation │
                    │ & Text Assembly  │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │ Make AI Toolkit  │
                    │ AI Extraction    │
                    └────────┬─────────┘
                             │
                   ┌─────────┴─────────┐
                   │                   │
                   ▼                   ▼
          ┌────────────────┐   ┌─────────────────┐
          │ Metadata       │   │ Duplicate Check │
          │ Extraction     │   │ Data Store      │
          └────────────────┘   └────────┬────────┘
                                        │
                                        ▼
                               ┌─────────────────┐
                               │ New Document?   │
                               └────────┬────────┘
                                        │
                                        ▼
                               ┌─────────────────┐
                               │ Google Sheets   │
                               │ Structured Data │
                               └─────────────────┘
Core Capabilities
1. Automated Email Ingestion

MailFlow AI monitors Gmail for incoming emails containing attachments.

The workflow uses Gmail search criteria to identify messages with attachments.

has:attachment

This allows the workflow to operate continuously without requiring manual file uploads.

2. Attachment Processing

Once an email is detected, the workflow retrieves the available attachment data and passes it to the document-processing stage.

The original email information can also be used later for metadata such as:

Sender email
Message ID
Email timestamp
Subject / filename information
3. Document Content Extraction

The workflow uses PDF Vector to parse the document and produce machine-readable content.

The extracted pages are then combined before being passed to the AI extraction stage.

PDF
 ↓
Document Parser
 ↓
Page Content
 ↓
Aggregator
 ↓
Combined Document Text
4. AI-Powered Information Extraction

The extracted document content is passed to the Make AI Toolkit – Extract module.

The current workflow uses Gemini through the Make AI Toolkit.

The AI extraction schema contains:

Field	Purpose
client_company_name	Identifies the client/company mentioned in the document
document_type	Identifies the financial/document category
applicable_period	Identifies the relevant reporting period
financial_year	Identifies the financial year when available

Example document categories supported by the extraction instructions include:

Bank Statement
Sales Invoice
Purchase Invoice
Expense Bill
Other

The purpose of this stage is to convert unstructured document content into structured information that downstream systems can use.

Duplicate Detection & Data Integrity

One of the important engineering components of MailFlow AI is duplicate detection.

Without duplicate handling, repeatedly processing the same email or document could create duplicate records in the final data store.

The workflow creates a document key using:

Gmail Message ID
        +
MD5 hash of extracted document content

The Data Store is used to check whether that key already exists.

Document
   ↓
Generate Key
   ↓
Check Data Store
   ↓
Already Exists?
   │
   ├── YES → Stop processing
   │
   └── NO
        ↓
   Store Record
        ↓
   Continue
        ↓
   Google Sheets

This introduces an important concept from production automation systems:

Idempotent processing

The workflow is designed so that previously processed documents do not repeatedly create new output records.

End-to-End Workflow

The current Make.com scenario consists of the following stages:

01 — Gmail: Watch Emails

Monitors Gmail for new messages matching the configured search criteria.

Current search:

has:attachment
02 — Gmail: Get / Process Attachments

Retrieves attachment data from the detected email.

03 — PDF Vector: Parse Document

Processes the document and extracts its textual content.

The current blueprint uses automatic model selection for the PDF Vector parser.

04 — Aggregator

Combines extracted page content into a single document representation.

This allows the AI extraction stage to process the complete document rather than isolated pages.

05 — Make AI Toolkit: Extract

The extracted document content is sent to the AI extraction layer.

The current blueprint configures Gemini through Make AI Toolkit and extracts:

client_company_name
document_type
applicable_period
financial_year
06 — Data Store: Check Duplicate

The workflow checks the generated document key against the:

Client Document Deduplication

Data Store.

07 — Data Store: Store Document Key

If the document is new, its processing key and related metadata are stored.

The workflow only continues when the duplicate check confirms that the document has not already been processed.

08 — Google Sheets: Save Result

The extracted information is written into the configured Google Sheet.

Structured Output

The workflow writes processed document information into the:

Received Documents

Google Sheets worksheet.

The current output schema is:

Column	Description
Client Name	AI-extracted client/company name
Document Type	AI-classified document category
Month / Period	Extracted applicable period
Sender Email	Email sender
Status	Current processing status
File Name	Source email/attachment information
Received At	Email received timestamp

This creates a simple structured record that can later be connected to additional business systems.

Technology Stack
Technology	Role
Make.com	Workflow orchestration
Gmail	Email ingestion
PDF Vector	Document parsing
Make AI Toolkit	AI extraction
Gemini	AI model used by the extraction layer
Make Data Store	Duplicate detection and processing records
Google Sheets	Structured output and tracking
Engineering Concepts Demonstrated

MailFlow AI is intentionally built around practical engineering concepts rather than simply demonstrating an AI model.

Event-Driven Processing

The workflow begins when an email containing an attachment is detected.

Workflow Orchestration

Multiple services are connected into a single automated processing pipeline.

Unstructured → Structured Data

Documents are converted from raw content into structured business metadata.

AI-Assisted Extraction

AI is used for document classification and information extraction instead of relying entirely on manually defined rules.

Idempotent Processing

Duplicate detection prevents previously processed documents from repeatedly entering the output pipeline.

Data Persistence

The Make Data Store maintains processing information between workflow executions.

System Integration

The project integrates multiple external systems:

Email
+
Document Processing
+
AI
+
Persistent Storage
+
Spreadsheet

This makes the project representative of real-world business automation and integration engineering.

Project Structure
MailFlow AI/
│
├── blueprint.json
├── README.md
└── .gitignore
blueprint.json

Contains the Make.com scenario blueprint used to recreate the workflow.

README.md

Contains the technical documentation, architecture, workflow explanation, and setup instructions.

.gitignore

Prevents local configuration files, credentials, environment files, and other sensitive or unnecessary files from being committed.

Setup
Requirements

To recreate the workflow, you will need:

Make.com account
Gmail account with appropriate access
Google Sheets access
PDF Vector access
Make AI Toolkit access
Gemini access through the configured AI integration
Make Data Store
Importing the Workflow
Open Make.com.
Create a new scenario.
Import blueprint.json.
Connect your Gmail account.
Configure the document-processing connection.
Configure the AI connection.
Connect Google Sheets.
Create/configure the required Data Store.
Configure the output spreadsheet.
Run a controlled test using an email containing a supported document.

Connection IDs and account-specific configuration from the original environment must be replaced with your own connections when importing the blueprint.

Potential Business Applications

The architecture can be adapted to business processes where documents regularly arrive through email.

Potential applications include:

Accounting document processing
Invoice intake
Client document collection
Financial document organization
Administrative workflows
Procurement documentation
Internal reporting
Document-based operations

The workflow provides a foundation that can be extended with additional business systems and processing logic.

AmpleTech AI Relevance

MailFlow AI represents the type of practical AI workflow engineering that can be applied to business operations.

The underlying approach is:

Understand the business process
            ↓
Identify repetitive manual work
            ↓
Design an automated workflow
            ↓
Integrate AI where it adds value
            ↓
Connect business systems
            ↓
Deploy a usable operational workflow

This aligns with the broader engineering approach of AmpleTech AI:

Consult. Engineer. Deploy AI systems.

Rather than treating AI as an isolated chatbot or model, the project demonstrates how AI can become part of an operational business process.

Future Improvements

Potential extensions include:

Support for additional document formats
More document classification categories
Confidence scoring for AI extraction
Human review for low-confidence results
Error-handling and retry workflows
Automated notifications
Database integration
CRM integration
Accounting software integration
Analytics dashboards
More advanced document validation
Multi-stage approval workflows
What This Project Demonstrates

MailFlow AI demonstrates the ability to design and implement an AI-enabled workflow that connects:

Business Problem
      ↓
Workflow Design
      ↓
Email Ingestion
      ↓
Document Processing
      ↓
AI Extraction
      ↓
Data Validation
      ↓
Duplicate Detection
      ↓
Structured Output

The project is therefore useful as a practical demonstration of:

AI engineering
Automation engineering
Workflow architecture
API/service integration
Document processing
Data handling
AI-assisted information extraction
Business process automation
License

License

This project is licensed under the MIT License


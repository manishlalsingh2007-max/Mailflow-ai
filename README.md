
# MailFlow AI

### AI-Powered Document Intelligence & Workflow Automation

MailFlow AI is an intelligent document-processing workflow designed to transform business documents received through email into structured, actionable information.

The system connects Gmail, document processing, AI-powered information extraction, duplicate detection, persistent data handling, and Google Sheets into a single automated pipeline.

Rather than treating AI as an isolated capability, MailFlow AI places AI inside a complete operational workflow — from document intake to structured business output.

---

## Overview

Modern businesses receive large amounts of operational information through email. In many workflows, employees still need to open documents, understand their contents, identify important fields, check whether the document has already been processed, and manually enter the information into another system.

MailFlow AI explores how this process can be automated through a combination of document processing, AI extraction, workflow orchestration, and persistent data handling.

The core flow is:

**Email → Document → Content → AI → Validation → Structured Data**

The current implementation is built around Gmail, Make.com, document parsing, AI extraction, a Make Data Store, and Google Sheets.

---

## The Problem

Document-heavy workflows commonly introduce several operational challenges:

* Repetitive manual document review
* Manual extraction of important information
* Repeated data entry
* Unstructured document content
* Duplicate records
* Difficult document tracking
* Time spent moving information between systems

The problem is not simply the presence of documents.

The larger challenge is the gap between **unstructured information** and **usable business data**.

MailFlow AI addresses that gap through an automated processing pipeline.

---

## The Solution

MailFlow AI receives document-based information through Gmail and processes it through a series of dedicated stages.

The workflow:

**1. Detects incoming documents**

**2. Retrieves document content**

**3. Parses the document**

**4. Aggregates extracted content**

**5. Uses AI to identify relevant business information**

**6. Checks for previously processed content**

**7. Maintains processing state**

**8. Creates a structured business record**

This creates a repeatable path from incoming information to operational data.

---

# Architecture

### High-Level System

**Gmail**

Incoming emails and attachments

↓

**Document Processing**

Retrieval and parsing

↓

**Content Aggregation**

Unified document representation

↓

**AI Extraction**

Structured business information

↓

**Duplicate Detection**

Content-based processing check

↓

**Make Data Store**

Persistent processing state

↓

**Google Sheets**

Structured operational output

---

### Architecture Layers

**Input Layer**

Gmail provides the entry point for incoming business documents.

**Processing Layer**

Document content is retrieved and parsed into machine-readable information.

**Intelligence Layer**

The AI extraction stage analyzes the processed content and identifies predefined business fields.

**Validation Layer**

The workflow checks existing processing state to reduce duplicate records.

**Persistence Layer**

The Make Data Store maintains information required for duplicate control.

**Output Layer**

Google Sheets receives the final structured business record.

This separation allows individual stages to evolve independently as the system grows.

---

# Workflow

## 01 — Email Detection

The workflow monitors Gmail for incoming emails matching the configured attachment condition.

The email provides important metadata such as:

* Gmail message ID
* Sender email
* Subject
* Received timestamp
* Attachment-related content

---

## 02 — Document Retrieval

The document content associated with the incoming email is retrieved and passed into the processing pipeline.

This creates the transition from the email layer to the document intelligence layer.

---

## 03 — Document Parsing

The current implementation processes PDF-based document content through the document parsing layer.

The parsed document is converted into machine-readable markdown that can be passed into subsequent processing stages.

---

## 04 — Content Aggregation

Documents may contain multiple pages or extracted content sections.

The workflow aggregates the available content into a unified representation before sending it to the AI extraction stage.

This gives the AI layer broader document context instead of treating individual extracted sections as unrelated inputs.

---

## 05 — AI Information Extraction

The aggregated document content is passed to the AI extraction module.

The current extraction schema contains four primary fields:

| Field               | Purpose                                        |
| ------------------- | ---------------------------------------------- |
| Client Company Name | Identifies the relevant client or organization |
| Document Type       | Identifies the category of document            |
| Applicable Period   | Identifies the relevant month or period        |
| Financial Year      | Identifies the associated financial year       |

The workflow is currently configured with **Gemini 3.8 Flash** as the AI model.

---

## 06 — Duplicate Detection

Duplicate processing is controlled through a dedicated Make Data Store.

The workflow generates a processing key using:

**Gmail Message ID + MD5 Hash of Extracted Document Content**

The content hash provides a way to compare the processed document content against previously recorded processing information.

This is designed to reduce unnecessary duplicate records and provide more controlled workflow execution.

---

## 07 — Data Persistence

The Data Store maintains processing information associated with documents that have entered the workflow.

The stored information includes processing-related fields such as:

**Gmail Message ID**
**Content Hash**
**Document Reference**
**Received Timestamp**
**Processing Status**

This provides persistent state that can be referenced during subsequent processing.

---

## 08 — Structured Output

After extraction and duplicate validation, the workflow records the processed information in Google Sheets.

The current output structure contains:

**Client Name**
**Document Type**
**Month / Period**
**Sender Email**
**Status**
**File Name**
**Received At**

> The current workflow maps the **File Name** field from the email subject rather than the original attachment filename.

---

# Data Flow

The complete processing lifecycle can be viewed as:

**Unstructured Email**

↓

**Document Content**

↓

**Parsed Document**

↓

**Aggregated Content**

↓

**AI-Generated Structured Fields**

↓

**Duplicate Validation**

↓

**Persistent Processing Record**

↓

**Business Spreadsheet**

This data flow is the core of MailFlow AI.

It demonstrates how unstructured information can move through multiple processing stages before becoming structured operational data.

---

# AI Extraction Layer

The AI component is not used simply as a conversational interface.

It acts as an information extraction layer within a larger system.

Its role is to take processed document content and identify predefined business attributes.

This approach creates a clear boundary between:

**Raw Information**

and

**Structured Business Information**

That separation makes the workflow easier to extend with additional fields, validation rules, or downstream integrations.

---

# Data Integrity

A major part of the workflow is ensuring that automation does not simply create more duplicate or inconsistent records.

MailFlow AI therefore includes:

**Persistent processing state**

The Data Store remembers relevant processing information.

**Content hashing**

An MD5 hash is generated from the extracted document content.

**Conditional processing**

The workflow checks whether the relevant processing record already exists before adding a new record.

Together, these mechanisms provide a foundation for controlled document processing.

---

# Technology Stack

### Make.com

Primary workflow orchestration platform connecting the individual processing stages.

### Gmail

Document intake and email monitoring layer.

### Make AI Tools

AI-powered information extraction from processed document content.

### PDF Vector / Document Parser

Document content extraction and parsing.

### Make Data Store

Persistent state and duplicate detection.

### Google Sheets

Structured business output and operational tracking.

### Gemini 3.8 Flash

AI model configured within the current extraction workflow.

---

# Engineering Concepts

MailFlow AI brings together several practical engineering concepts:

**AI Integration**

Embedding AI into a larger software workflow rather than using the model independently.

**Document Intelligence**

Processing unstructured documents and extracting meaningful information.

**Data Transformation**

Converting raw document content into structured business fields.

**Workflow Orchestration**

Coordinating multiple services into a single processing pipeline.

**Persistent State**

Maintaining information across workflow executions.

**Content Hashing**

Using content-derived hashes as part of duplicate detection.

**Conditional Processing**

Allowing workflow execution to change based on previously stored state.

**System Integration**

Connecting AI capabilities with existing business tools.

**Operational Automation**

Replacing repetitive manual processes with an automated system.

---

# Why This Architecture

The workflow is deliberately divided into independent stages instead of placing the entire process into one large automation block.

This provides several advantages.

### Separation of Responsibilities

Each stage has a defined role:

**Intake → Processing → Intelligence → Validation → Output**

### Extensibility

The current Google Sheets output could later be replaced or supplemented with other business systems.

### Maintainability

Individual processing stages can be modified without redesigning the entire workflow.

### Business Adaptability

The same architecture can be adapted for different document types, extraction schemas, and operational environments.

The current implementation therefore serves as a foundation rather than a fixed end-state system.

---

# Business Applications

The architecture can be adapted to several document-heavy business environments.

### Accounting & Finance

Client documents, invoices, statements, recurring financial records, and document collection workflows.

### Professional Services

Recurring client documentation, administrative processing, and structured client records.

### Operations

Internal document intake, operational tracking, and automated record creation.

### Legal & Compliance

Document classification, metadata extraction, and document tracking.

### Human Resources

Application documents, employee documentation, and structured information extraction.

---

# Current Scope

The current implementation focuses on:

**Gmail-based document intake**

**PDF document processing**

**AI-powered information extraction**

**Duplicate detection**

**Make Data Store persistence**

**Google Sheets output**

It is currently implemented as a working automation prototype rather than a fully independent production application.

---

# Future Roadmap

The architecture provides a foundation for future development.

### Document Intelligence

* Additional document formats
* Advanced document classification
* More complex extraction schemas
* Confidence scoring

### Human Oversight

* Human-in-the-loop validation
* Exception handling
* Review queues

### Integrations

* CRM systems
* Databases
* Cloud storage
* Internal business applications

### Operations

* Monitoring dashboards
* Error handling
* Retry mechanisms
* Processing analytics

### AI Systems

* More advanced LLM workflows
* Context-aware extraction
* Automated downstream decision-making
* Multi-stage AI processing

---

# AmpleTech AI

MailFlow AI reflects the engineering philosophy behind **AmpleTech AI — AI Consulting & Engineering**.

AmpleTech AI focuses on identifying meaningful business problems, engineering practical AI systems around existing environments, and deploying those systems into real operational workflows.

MailFlow AI represents that approach through a simple progression:

**Consult**

Identify where repetitive, document-heavy processes create operational friction.

**Engineer**

Design the AI workflow, processing logic, integrations, and data handling required to solve the problem.

**Deploy**

Connect the engineered system to the organization's existing tools and operational processes.

### Consult. Engineer. Deploy AI systems.

---

# Setup

The workflow can be recreated using the exported Make.com blueprint included in this repository.

### Requirements

**Make.com**

**Gmail**

**AI extraction connection**

**Make Data Store**

**Google Sheets**

### Process

Import the blueprint, configure the required service connections, create the required Data Store and Google Sheets structure, and test the workflow using representative documents.

Authentication credentials, API keys, OAuth tokens, and private connection information must be configured independently and should never be committed to the repository.

---

# Project Structure

**Mailflow-ai**

`README.md`
Project documentation and system overview

`blueprint.json`
Exported Make.com workflow blueprint

`.gitignore`
Repository and environment exclusions

`LICENSE`
MIT License

---

# Project Status

### Working Prototype

MailFlow AI currently implements the complete workflow from Gmail document intake through document processing, AI extraction, duplicate detection, and structured Google Sheets output.

The architecture is intentionally designed so that additional document types, integrations, validation layers, and AI capabilities can be introduced over time.

---

# License

This project is licensed under the MIT License.




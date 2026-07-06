# AI Architecture

**Project:** ClinicOS – AI Workflow Platform for Healthcare Clinics

**Document Version:** 1.0

**Status:** Approved

**Owner:** AI Engineering Team

**Sprint:** Sprint 0 – Engineering Foundation

---

# 1. Purpose

This document defines the Artificial Intelligence architecture for ClinicOS.

It specifies how AI services are designed, how Retrieval-Augmented Generation (RAG) operates, how AI agents collaborate, how prompts are managed, how documents are indexed, and how AI integrates with the Business API.

The AI architecture is designed to be modular, provider-independent, observable, and safe for healthcare workflow automation.

---

# 2. AI Philosophy

ClinicOS does not use AI to replace healthcare professionals.

Instead, AI acts as an intelligent assistant that:

* Understands patient requests
* Retrieves clinic knowledge
* Classifies conversations
* Assists workflow execution
* Generates structured recommendations

Business decisions always remain under deterministic business logic.

---

# 3. AI Responsibilities

The AI Service is responsible for:

* Intent Detection
* Entity Extraction
* Specialty Detection
* Urgency Classification
* RAG
* Question Answering
* AI Draft Generation
* Conversation Summarization
* Document Processing
* Embedding Generation
* Prompt Orchestration

The AI Service never:

* Updates the database
* Books appointments
* Changes schedules
* Modifies patient records
* Makes medical decisions

---

# 4. AI Technology Stack

Language

* Python

Framework

* FastAPI

AI Framework

* LangChain

Workflow Engine

* LangGraph

LLM Provider

* Google Gemini

Embedding Model

* Gemini Embeddings

Vector Database

* ChromaDB (Prototype)

Document Processing

* PyMuPDF
* Unstructured

Validation

* Pydantic

Observability

* LangSmith (Future)

---

# 5. AI System Architecture

```text
Patient Message

↓

Business API

↓

AI Gateway

↓

LangGraph Workflow

↓

Intent Agent

↓

RAG Agent

↓

Prompt Builder

↓

Gemini

↓

Response Validator

↓

Structured JSON

↓

Business API
```

The AI Service communicates only through the AI Gateway.

---

# 6. AI Agents

ClinicOS uses specialized AI agents.

## Intake Agent

Responsibilities:

* Detect patient intent
* Identify specialty
* Extract symptoms
* Determine urgency

Output:

```json
{
  "intent": "BOOK_APPOINTMENT",
  "specialty": "Orthopedics",
  "urgency": "Medium",
  "confidence": 0.96
}
```

---

## RAG Agent

Responsibilities:

* Retrieve relevant clinic documents
* Rank search results
* Assemble context
* Reduce hallucinations

---

## Response Agent

Responsibilities:

* Generate patient-friendly responses
* Use only approved context
* Follow clinic tone
* Produce structured output

---

## Emergency Agent

Responsibilities:

Detect:

* Chest pain
* Severe bleeding
* Difficulty breathing
* Stroke symptoms
* Loss of consciousness

If confidence exceeds the configured threshold:

* Stop AI conversation
* Flag emergency
* Return emergency response
* Notify Business API

---

## Summarization Agent

Responsibilities:

* Summarize long conversations
* Prepare summaries for staff dashboard
* Reduce reading time

---

## Document Processing Agent

Responsibilities:

* Extract text
* Split into chunks
* Generate embeddings
* Store vectors
* Maintain metadata

---

# 7. RAG Pipeline

Knowledge Ingestion

↓

Document Cleaning

↓

Chunking

↓

Embedding Generation

↓

Vector Storage

↓

Metadata Storage

↓

Search Index

During Query

Patient Question

↓

Embedding

↓

Vector Search

↓

Top K Documents

↓

Prompt Assembly

↓

Gemini

↓

Answer

---

# 8. Prompt Architecture

Prompt templates are version-controlled.

Structure:

System Prompt

↓

Workflow Prompt

↓

Retrieved Context

↓

User Message

↓

Output Schema

Prompt templates never contain business logic.

---

# 9. AI Workflow Orchestration

LangGraph coordinates all AI workflows.

Example

Patient Question

↓

Intent Detection

↓

Emergency Check

↓

Need RAG?

↓

Yes

↓

Retrieve Context

↓

Generate Response

↓

Validate Response

↓

Return JSON

---

# 10. Response Validation

Every AI response passes through validation.

Checks include:

* JSON schema validation
* Required fields
* Confidence threshold
* Prompt injection detection
* Empty response detection

Invalid responses are rejected.

---

# 11. Knowledge Base

Supported documents:

* Clinic FAQs
* Treatment Guides
* Insurance Policies
* Preparation Instructions
* Doctor Profiles
* Clinic SOPs

Each document includes metadata:

* Tenant ID
* Clinic ID
* Category
* Version
* Upload Date
* Source

---

# 12. Model Abstraction

ClinicOS is provider-independent.

Current Provider:

* Gemini

Future Providers:

* OpenAI
* Claude
* Azure OpenAI
* Local LLMs

All providers implement a common interface.

Changing providers does not affect business logic.

---

# 13. AI Safety

The AI must never:

* Diagnose diseases
* Prescribe medication
* Override emergency workflows
* Invent clinic policies
* Invent appointment slots
* Hallucinate insurance coverage

If required information is unavailable, the AI must explicitly indicate uncertainty.

---

# 14. AI Observability

Metrics collected:

* Prompt latency
* Model latency
* Token usage
* Retrieval latency
* Confidence scores
* Error rates
* AI request volume

Future integration:

* LangSmith
* OpenTelemetry

---

# 15. AI Security

Security measures:

* Tenant-aware retrieval
* Prompt injection protection
* Context isolation
* Input sanitization
* Output validation
* Rate limiting

Sensitive business rules remain outside the AI service.

---

# 16. AI Failure Handling

If the AI Service fails:

Business API:

* Logs the error
* Returns a safe fallback
* Preserves workflow state
* Alerts administrators (if required)

Business operations continue without corruption.

---

# 17. Future AI Roadmap

Planned additions:

* Voice AI Receptionist
* OCR Medical Reports
* Medical Timeline Generation
* Follow-up Agent
* Prescription Refill Assistant
* Multilingual Support
* AI Quality Evaluation
* Human Feedback Loop

---

# 18. AI Architecture Summary

ClinicOS treats Artificial Intelligence as an independent intelligence layer rather than embedding AI directly into the Business API.

The AI Service performs understanding, retrieval, summarization, and recommendation while deterministic business logic remains inside the Node.js application.

This separation creates a scalable, provider-independent architecture that minimizes hallucinations, improves maintainability, and enables future AI enhancements without impacting core business workflows.

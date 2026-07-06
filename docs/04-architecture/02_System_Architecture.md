# System Architecture

**Project:** ClinicOS – AI Workflow Platform for Healthcare Clinics

**Document Version:** 1.0

**Status:** Approved

**Owner:** Architecture Team

**Sprint:** Sprint 0 – Engineering Foundation

---

# 1. Purpose

This document defines the complete system architecture of ClinicOS.

It describes how every major component communicates, how data flows through the platform, how requests are processed, and how the system scales from a local prototype to an enterprise SaaS deployment.

This document serves as the master engineering reference for backend, frontend, AI, DevOps, QA, and infrastructure teams.

---

# 2. System Overview

ClinicOS is a multi-tenant healthcare workflow platform composed of independently deployable services.

The system separates presentation, business logic, artificial intelligence, storage, and infrastructure into clearly defined layers.

Each layer owns a single responsibility and communicates only through well-defined interfaces.

---

# 3. Architectural Layers

```text
Presentation Layer
│
├── Patient Portal
├── Receptionist Dashboard
├── Doctor Dashboard
└── Admin Dashboard

↓

Business Layer

↓

Workflow Layer

↓

AI Layer

↓

Data Layer

↓

Infrastructure Layer
```

Each layer is replaceable without affecting the others.

---

# 4. Complete System Architecture

```text
                     Internet
                         │
                    HTTPS / TLS
                         │
                    Nginx / Ingress
                         │
                         ▼
               React Frontend (Vite)
                         │
                  REST / WebSocket
                         │
                         ▼
           Node.js + Express Business API
                         │
     ┌───────────┬────────────┬────────────┐
     │           │            │            │
     ▼           ▼            ▼            ▼
 Authentication Workflow   Notifications Analytics
     │
     ▼
 Workflow Engine
     │
 ┌───┴───────────────┐
 ▼                   ▼
 PostgreSQL      FastAPI AI Service
                     │
          ┌──────────┼───────────┐
          ▼          ▼           ▼
      LangChain  LangGraph   Prompt Engine
                     │
                     ▼
                 Gemini API
                     │
                     ▼
                 ChromaDB
```

---

# 5. Frontend Layer

The frontend is responsible only for user interaction.

Responsibilities include:

* Rendering UI
* Form validation
* API communication
* Authentication state
* Dashboard rendering
* Real-time updates
* File uploads

The frontend contains no business logic.

---

# 6. Business Layer

The Business API is the operational brain of ClinicOS.

Responsibilities include:

* Authentication
* Authorization
* Multi-tenancy
* Clinics
* Doctors
* Patients
* Appointments
* Insurance
* Notifications
* Analytics
* Workflow orchestration

Only this layer can modify business data.

---

# 7. Workflow Layer

The Workflow Engine coordinates all business processes.

Examples include:

* Patient Intake
* Appointment Scheduling
* Insurance Verification
* Emergency Escalation
* Notification Dispatch
* Audit Logging

Each workflow is deterministic and controlled by business rules.

Artificial intelligence assists but does not execute business decisions.

---

# 8. AI Layer

The AI Service is implemented as an independent FastAPI application.

Responsibilities:

* Intent Detection
* Symptom Extraction
* RAG
* Prompt Engineering
* LLM Communication
* AI Agents
* Summarization
* Classification
* Knowledge Retrieval

The AI layer never updates the database directly.

It returns structured recommendations to the Business API.

---

# 9. Data Layer

## PostgreSQL

Primary transactional database.

Stores:

* Clinics
* Users
* Patients
* Doctors
* Appointments
* Conversations
* Insurance Plans
* Audit Logs

---

## Redis

Stores:

* Session cache
* Rate limiting
* Background jobs
* Temporary workflow state

---

## ChromaDB

Stores:

* Vector embeddings
* Knowledge documents
* Clinic manuals
* FAQs

---

## Object Storage

Stores:

* Medical reports
* Images
* PDFs
* Knowledge documents

Development:

* MinIO

Production:

* AWS S3 compatible storage

---

# 10. Request Lifecycle

Every request follows the same lifecycle.

```text
User Request

↓

Frontend

↓

Business API

↓

Authentication

↓

Authorization

↓

Validation

↓

Workflow Selection

↓

Business Rule Evaluation

↓

AI Service (optional)

↓

Database Operations

↓

Notifications

↓

Audit Logging

↓

Response

↓

Frontend
```

---

# 11. AI Request Flow

```text
Patient Message

↓

Business API

↓

AI Service

↓

Intent Detection

↓

Retrieve Documents

↓

Generate Prompt

↓

Gemini

↓

Structured JSON

↓

Business Validation

↓

Workflow Execution
```

AI recommendations are always validated before execution.

---

# 12. Appointment Workflow

```text
Patient

↓

Appointment Request

↓

Intent Detection

↓

Doctor Matching

↓

Insurance Verification

↓

Available Slots

↓

Business Validation

↓

Appointment Created

↓

Notification Sent

↓

Dashboard Updated
```

---

# 13. Emergency Workflow

```text
Patient Message

↓

Intent Detection

↓

Emergency Classification

↓

Critical?

↓

Yes

↓

Stop AI Conversation

↓

Create Emergency Case

↓

Notify Receptionist

↓

Highlight Dashboard

↓

Manual Intervention
```

The platform never provides emergency medical advice.

---

# 14. Authentication Flow

```text
Login

↓

Credential Validation

↓

JWT Generation

↓

RBAC Validation

↓

Protected Routes

↓

Business Operations
```

Every request is authenticated before entering business services.

---

# 15. Multi-Tenant Architecture

Each clinic operates independently.

Every record contains a Tenant ID.

Data isolation is enforced at:

* API layer
* Service layer
* Repository layer
* Database queries

No tenant can access another tenant's data.

---

# 16. Communication Contracts

Frontend ↔ Business API

* REST
* WebSocket

Business API ↔ AI Service

* REST

Business API ↔ PostgreSQL

* Prisma ORM

Business API ↔ Redis

* Cache

Business API ↔ Object Storage

* Storage SDK

AI Service ↔ Gemini

* Provider SDK

AI Service ↔ ChromaDB

* Vector Search

---

# 17. Security Architecture

Security controls include:

* HTTPS
* JWT Authentication
* RBAC
* bcrypt Password Hashing
* Input Validation
* Rate Limiting
* Helmet
* CORS
* Audit Logging

Future support includes:

* OAuth2
* OpenID Connect
* SSO
* MFA

---

# 18. Scalability Strategy

Every service scales independently.

Examples:

* Scale Frontend replicas
* Scale Business API
* Scale AI Service
* Scale Redis
* Scale PostgreSQL read replicas

Container orchestration is managed through Kubernetes.

---

# 19. Observability

Monitoring:

* Prometheus

Visualization:

* Grafana

Logging:

* Pino
* Loki

Tracing:

* OpenTelemetry

Error Tracking:

* Sentry

Every request is traceable across services.

---

# 20. CI/CD Pipeline

```text
Developer Push

↓

GitHub

↓

GitHub Actions

↓

Install Dependencies

↓

Lint

↓

Unit Tests

↓

Integration Tests

↓

Docker Build

↓

Security Scan

↓

Build Artifacts

↓

Deploy (Development)

↓

Manual Approval

↓

Production Deployment
```

---

# 21. Disaster Recovery

The system supports:

* Automated database backups
* Container recreation
* Infrastructure as Code
* Stateless application services
* Object storage replication

Future enhancements:

* Multi-region deployment
* Cross-region backups

---

# 22. Future Evolution

The architecture is designed for future expansion.

Planned additions include:

* API Gateway
* Kafka/RabbitMQ
* Event-Driven Architecture
* Service Mesh
* Multi-region Kubernetes
* FHIR Integration
* Insurance APIs
* OCR Pipelines
* Voice AI
* Multi-model AI providers

---

# 23. Architecture Summary

ClinicOS is designed as a modular, cloud-native, multi-tenant SaaS platform.

Business logic, artificial intelligence, storage, workflows, and infrastructure are isolated into independently scalable services.

This architecture enables rapid prototype development while providing a clear path toward enterprise deployment without requiring fundamental redesign.

The system is designed to prioritize maintainability, scalability, security, and operational reliability while keeping healthcare professionals in control of all clinical decisions.

# High-Level Architecture (HLD)

**Project:** ClinicOS – AI Workflow Platform for Healthcare Clinics

**Document Version:** 1.0

**Status:** Approved

**Owner:** System Architecture Team

**Sprint:** Sprint 0 – Engineering Foundation

---

# 1. Purpose

This document defines the High-Level Architecture (HLD) for ClinicOS.

It describes the major system components, service boundaries, communication patterns, deployment architecture, technology stack, and overall software design.

This document acts as the blueprint for every engineering team involved in developing ClinicOS.

---

# 2. Architecture Goals

The architecture is designed to achieve the following goals:

* Modular architecture
* Multi-tenant SaaS
* Production-ready design
* Cloud-native deployment
* AI-first workflow automation
* High scalability
* Security by design
* Service isolation
* Easy maintainability
* Independent deployment of services

---

# 3. Architectural Principles

ClinicOS follows these engineering principles:

## Domain Separation

Business logic and Artificial Intelligence are independent services.

Business decisions belong to the Business API.

AI provides recommendations and intelligence.

---

## API First

Every feature is exposed through well-defined REST APIs.

Internal services communicate through versioned APIs.

---

## Cloud Native

Every service runs independently inside Docker containers.

Services are orchestrated using Kubernetes.

---

## Stateless Services

Application services remain stateless.

Persistent data is stored in PostgreSQL, Redis, object storage, and vector databases.

---

## Security by Design

Authentication, authorization, encryption, audit logging, and role-based access control are integrated into every layer.

---

# 4. High-Level System Overview

```text
                        Internet
                            │
                    HTTPS / TLS
                            │
                            ▼
                    Nginx Reverse Proxy
                            │
        ┌───────────────────┼───────────────────┐
        │                                       │
        ▼                                       ▼
Frontend (React)                    Business API (Node.js)
                                            │
        ┌───────────────────────────────────┼──────────────────────────┐
        │                  │                │                          │
        ▼                  ▼                ▼                          ▼
 Authentication      Appointment      Patient                  Notification
    Service            Service         Service                    Service
        │                  │                │                          │
        └──────────────────┼────────────────┴──────────────────────────┘
                           │
                    Workflow Engine
                           │
            ┌──────────────┼──────────────┐
            ▼                             ▼
     PostgreSQL                     AI Service
                                      (FastAPI)
                                          │
                     ┌────────────────────┼─────────────────────┐
                     ▼                    ▼                     ▼
              LangChain            LangGraph            Gemini API
                     │
                     ▼
                 ChromaDB
```

---

# 5. Major Components

The platform consists of the following major subsystems.

## Frontend

Technology:

* React
* Vite
* JavaScript
* Tailwind CSS

Responsibilities:

* Patient Portal
* Staff Dashboard
* Doctor Dashboard
* Admin Dashboard
* Authentication UI
* Real-time updates

---

## Business API

Technology:

* Node.js
* Express.js
* JavaScript

Responsibilities:

* Authentication
* Authorization
* Clinic management
* Patient management
* Doctor management
* Appointment management
* Insurance management
* Workflow execution
* Notification orchestration
* Dashboard APIs
* Analytics

The Business API is the single source of truth for business rules.

---

## AI Service

Technology:

* Python
* FastAPI

Responsibilities:

* Intent Detection
* Symptom Extraction
* RAG
* Prompt Engineering
* LLM Integration
* AI Agents
* Summarization
* Knowledge Retrieval
* Document Processing

The AI Service never performs business operations directly.

It returns structured recommendations to the Business API.

---

## Workflow Engine

The Workflow Engine coordinates business processes such as:

* Patient Intake
* Appointment Booking
* Emergency Escalation
* Insurance Verification
* Notification Flow

Each workflow is deterministic and controlled by business rules.

---

# 6. Data Layer

## PostgreSQL

Primary transactional database.

Stores:

* Clinics
* Users
* Patients
* Doctors
* Appointments
* Insurance Plans
* Conversations
* Audit Logs

---

## Redis

Stores:

* Cache
* Sessions
* Rate limiting
* Job queues

---

## ChromaDB

Stores:

* Vector embeddings
* Clinic knowledge
* Medical documents

Prototype implementation.

Can later be replaced by Qdrant with minimal code changes.

---

## Object Storage

Stores:

* Medical reports
* PDFs
* Images
* Knowledge documents

Development:

* MinIO

Production:

* AWS S3 compatible storage

---

# 7. Communication Pattern

Frontend communicates only with the Business API.

The Business API communicates with:

* PostgreSQL
* Redis
* AI Service
* Notification Service
* Object Storage

The AI Service communicates with:

* Gemini
* LangChain
* LangGraph
* ChromaDB

This separation ensures loose coupling and independent scalability.

---

# 8. Security Architecture

Authentication:

* JWT

Authorization:

* Role-Based Access Control (RBAC)

Passwords:

* bcrypt hashing

Transport Security:

* HTTPS/TLS

Application Security:

* Helmet
* Rate limiting
* Input validation
* CORS
* Audit logging

Future enhancements include OAuth2, OpenID Connect, and SSO.

---

# 9. Deployment Architecture

Every component runs inside its own Docker container.

Development:

* Docker Compose

Production:

* Kubernetes

CI/CD:

* GitHub Actions

Ingress:

* Nginx

Monitoring:

* Prometheus
* Grafana

Logging:

* Pino
* Loki

Tracing:

* OpenTelemetry

---

# 10. Scalability Strategy

Each service can scale independently.

Examples:

* Increase AI Service replicas during heavy AI workloads.
* Scale Business API during peak appointment hours.
* Scale Frontend independently for increased traffic.

Horizontal scaling is supported through Kubernetes.

---

# 11. High-Level Request Flow

1. Patient opens ClinicOS.
2. Frontend sends request to Business API.
3. Business API authenticates the request.
4. Workflow Engine determines the required workflow.
5. If AI processing is required, the request is sent to the AI Service.
6. AI Service processes the request using LangChain, LangGraph, Gemini, and ChromaDB.
7. Structured AI output is returned to the Business API.
8. Business API validates business rules.
9. PostgreSQL is updated.
10. Notifications are sent if required.
11. Response is returned to the Frontend.

Business rules always execute after AI recommendations.

---

# 12. Architecture Decisions

The following engineering decisions are adopted:

* Frontend and backend are independently deployable.
* AI is isolated from business logic.
* PostgreSQL is the system of record.
* REST APIs are used for inter-service communication during MVP.
* Every external dependency is abstracted behind service interfaces.
* Docker is mandatory for all services.
* Kubernetes is the production deployment platform.
* CI/CD is implemented using GitHub Actions.

---

# 13. Future Evolution

The architecture supports future expansion without major redesign.

Planned additions include:

* Event-driven messaging (Kafka/RabbitMQ)
* API Gateway
* Service Mesh
* Multi-region deployment
* Real EHR integrations (FHIR)
* Insurance provider APIs
* Voice AI
* OCR pipeline
* Multi-model AI provider support

---

# Conclusion

The ClinicOS High-Level Architecture establishes a modular, scalable, cloud-native foundation for building an enterprise-grade AI workflow platform for healthcare.

The architecture separates business logic from artificial intelligence, enables independent service scaling, supports modern DevOps practices, and provides a clear migration path from prototype to production without requiring architectural redesign.

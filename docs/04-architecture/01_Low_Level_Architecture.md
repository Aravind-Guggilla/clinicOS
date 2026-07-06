# Low-Level Architecture (LLD)

**Project:** ClinicOS – AI Workflow Platform for Healthcare Clinics

**Document Version:** 1.0

**Status:** Approved

**Owner:** Engineering Team

**Sprint:** Sprint 0 – Engineering Foundation

---

# 1. Purpose

This document defines the internal software architecture of ClinicOS.

It specifies the internal modules, responsibilities, service boundaries, communication patterns, folder organization, dependency flow, and coding standards for all major components.

This document acts as the implementation blueprint for backend, frontend, AI, and DevOps engineers.

---

# 2. Engineering Philosophy

ClinicOS follows a modular, layered architecture.

Each service owns a specific business domain and communicates only through well-defined interfaces.

Core principles:

* Separation of Concerns
* Single Responsibility Principle
* Dependency Inversion
* API First
* Domain-Driven Design (DDD)
* SOLID Principles
* Twelve-Factor App methodology

---

# 3. System Modules

ClinicOS is divided into four major domains:

```text
ClinicOS

├── Frontend
├── Business API
├── AI Service
└── Infrastructure
```

Each domain is independently deployable.

---

# 4. Frontend Architecture

Technology:

* React
* Vite
* JavaScript
* Tailwind CSS

Feature-based organization:

```text
frontend/

src/

components/

features/

auth/

patients/

appointments/

doctors/

dashboard/

analytics/

insurance/

chat/

layouts/

pages/

services/

hooks/

contexts/

utils/

assets/
```

### Responsibilities

* Render UI
* Form validation
* State management
* API communication
* Authentication
* Realtime updates
* File uploads

Business logic is never implemented in the frontend.

---

# 5. Business API Architecture

Technology:

* Node.js
* Express.js

Folder Structure

```text
backend/node-api/

src/

config/

routes/

controllers/

services/

repositories/

middleware/

validators/

workflows/

events/

notifications/

database/

utils/

server.js
```

---

### Layered Flow

```text
HTTP Request

↓

Route

↓

Controller

↓

Validation

↓

Service

↓

Repository

↓

Prisma ORM

↓

PostgreSQL
```

Controllers remain thin.

Business logic belongs only in Services.

Repositories manage database access.

---

# 6. AI Service Architecture

Technology:

* Python
* FastAPI

Folder Structure

```text
backend/ai-service/

app/

api/

agents/

rag/

prompts/

embeddings/

llm/

workflows/

models/

services/

vectorstore/

utils/

main.py
```

---

### Responsibilities

* Intent Detection
* RAG
* LangChain
* LangGraph
* Prompt Engineering
* LLM Calls
* Embedding Generation
* Document Processing
* AI Evaluation

The AI Service never updates business data.

It returns structured outputs only.

---

# 7. Workflow Engine

The Workflow Engine coordinates business processes.

Examples:

Patient Intake

↓

Insurance Verification

↓

Doctor Matching

↓

Appointment Booking

↓

Notification

↓

Audit Log

Each workflow is implemented as an independent service.

Example:

```text
workflows/

appointment.workflow.js

emergency.workflow.js

insurance.workflow.js

patient.workflow.js
```

---

# 8. Repository Pattern

Every business entity follows the same pattern.

Example:

Patient

↓

PatientController

↓

PatientService

↓

PatientRepository

↓

Prisma

↓

PostgreSQL

No controller communicates directly with the database.

---

# 9. AI Integration Flow

Patient submits a message.

↓

Business API validates request.

↓

Workflow Engine selects AI workflow.

↓

HTTP request sent to FastAPI.

↓

FastAPI performs:

* Intent Detection
* Knowledge Retrieval
* Prompt Assembly
* Gemini Call

↓

Structured JSON returned.

↓

Business API validates business rules.

↓

Database updated.

↓

Frontend notified.

Business logic always executes after AI recommendations.

---

# 10. Database Layer

Database:

PostgreSQL

Access:

Prisma ORM

Every table has:

* UUID Primary Key
* CreatedAt
* UpdatedAt
* Soft Delete Support
* Audit Information

Future support:

* Read Replicas
* Partitioning
* Backup Strategy

---

# 11. Caching Layer

Redis stores:

* Session cache
* Frequently accessed data
* Rate limiting
* Job queues
* Temporary AI context

Cache invalidation follows a cache-aside strategy.

---

# 12. File Storage

Uploads include:

* Medical Reports
* PDFs
* Images
* Clinic Documents

Development:

MinIO

Production:

AWS S3

Only metadata is stored in PostgreSQL.

Files remain in object storage.

---

# 13. Notification Layer

Notification Service abstracts communication.

Supported channels:

* Email
* In-App Notifications

Future:

* SMS
* WhatsApp
* Push Notifications

All notification providers implement the same interface.

---

# 14. Authentication & Authorization

Authentication

* JWT
* bcrypt

Authorization

RBAC

Roles

* Patient
* Receptionist
* Doctor
* Clinic Admin
* Platform Admin

Permissions are enforced through middleware.

---

# 15. Error Handling

Every service returns a standardized response.

Example

```json
{
  "success": true,
  "message": "Appointment booked successfully.",
  "data": {},
  "errors": null
}
```

Errors follow a consistent structure across all services.

---

# 16. Logging

Logging is centralized.

Levels:

* INFO
* WARN
* ERROR
* DEBUG

Every request includes:

* Request ID
* User ID
* Tenant ID
* Timestamp

---

# 17. Event Flow

Future architecture supports asynchronous events.

Examples:

AppointmentBooked

↓

Notification

↓

Analytics

↓

Audit Log

↓

Future Integrations

Current MVP uses synchronous REST.

Architecture allows migration to RabbitMQ or Kafka.

---

# 18. Dependency Rules

Frontend depends only on Business API.

Business API depends on:

* PostgreSQL
* Redis
* AI Service
* Notification Service

AI Service depends on:

* Gemini
* LangChain
* LangGraph
* ChromaDB

Infrastructure is independent.

No circular dependencies are allowed.

---

# 19. Coding Standards

All services must:

* Follow SOLID principles
* Use dependency injection where applicable
* Validate all inputs
* Return consistent API responses
* Use centralized error handling
* Be fully documented
* Include unit tests

---

# 20. Deployment Units

Each module is independently containerized.

Containers include:

* frontend
* node-api
* ai-service
* postgres
* redis
* chromadb
* minio
* nginx

Docker Compose is used for local development.

Kubernetes is used for production deployments.

---

# 21. Low-Level Architecture Summary

ClinicOS is designed as a modular, service-oriented platform with clear ownership boundaries.

Frontend handles presentation.

Node.js handles business rules.

Python handles intelligence.

PostgreSQL stores transactional data.

Redis provides caching and queues.

ChromaDB powers knowledge retrieval.

Docker and Kubernetes provide deployment portability.

This architecture enables rapid MVP development while remaining scalable enough to evolve into an enterprise-grade SaaS platform without requiring architectural redesign.

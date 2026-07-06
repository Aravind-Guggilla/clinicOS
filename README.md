                          React Frontend
                                │
                           REST API
                                │
                     Authentication Middleware
                                │
                       Tenant Context Middleware
                                │
                          RBAC Middleware
                                │
                            Controllers
                                │
                       Workflow Engine ⭐⭐⭐⭐⭐
                                │
        ┌──────────────┬──────────────┬──────────────┐
        ▼              ▼              ▼
 AppointmentService  PatientService  DoctorService
        ▼              ▼              ▼
 InsuranceService KnowledgeService NotificationService
        │
        ▼
      Repositories
        │
        ▼
 Prisma ORM
        │
 PostgreSQL



 ClinicOS/

frontend/

backend/
│
├── node-api/
│
├── src/
│
│   ├── config/
│   ├── middleware/
│   ├── controllers/
│   ├── routes/
│   ├── workflows/      ⭐
│   ├── services/
│   ├── repositories/
│   ├── validators/
│   ├── modules/
│   │
│   ├── auth/
│   ├── tenant/
│   ├── clinic/
│   ├── doctor/
│   ├── patient/
│   ├── appointment/
│   ├── insurance/
│   ├── notification/
│   │
│   ├── prisma/
│   ├── utils/
│   └── server.js
│
└── ai-service/
    │
    ├── app/
    ├── api/
    ├── orchestrator/ ⭐
    ├── agents/
    │   ├── intake/
    │   ├── emergency/
    │   ├── routing/
    │   ├── rag/
    │   ├── insurance/
    │   ├── scheduling/
    │   └── response/
    ├── prompts/
    ├── vectorstore/
    ├── services/
    ├── models/
    └── main.py

database/
│
└── prisma/
    └── schema.prisma


Phase 1 — Core Platform Tables

These tables have zero or minimal dependencies.

Tenant

User

Role

Permission

UserRole

Session

These enable authentication and multi-tenancy.

Phase 2 — Organization
Clinic

Department

Specialty

ClinicSpecialty

Now the organization exists.

Phase 3 — Doctor
Doctor

DoctorProfile

DoctorQualification

DoctorAvailability

DoctorLeave

DoctorClinicAssignment

DoctorSpecialtyAssignment

Now doctors exist.

Phase 4 — Patient
Patient

PatientProfile

EmergencyContact

PatientInsurance

PatientDocument
Phase 5 — Appointment
Appointment

AppointmentStatusHistory

AppointmentNote

AppointmentAttachment

Now booking works.

Phase 6 — Conversation
Conversation

Message

ConversationAttachment

Now AI chat works.

Phase 7 — Knowledge
KnowledgeDocument

KnowledgeChunk

EmbeddingMetadata

Remember:

Vectors live in ChromaDB.

Metadata lives in PostgreSQL.

Phase 8 — AI
AIRequest

WorkflowExecution

PromptLog
Phase 9 — Notification
Notification

NotificationTemplate

NotificationLog
Phase 10 — Analytics
Event

Metric

DashboardSnapshot



Before Writing schema.prisma

I want us to produce this:

Tenant
│
├── Clinics
│
│   ├── Departments
│   ├── Specialties
│   ├── Doctors
│   ├── Patients
│   ├── Appointments
│   ├── Conversations
│   └── Knowledge
│
├── Users
│
├── Notifications
│
└── Analytics

One complete ER diagram.
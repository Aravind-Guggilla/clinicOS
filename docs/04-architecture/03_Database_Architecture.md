# Database Architecture

**Project:** ClinicOS – AI Workflow Platform for Healthcare Clinics

**Document Version:** 1.0

**Status:** Approved

**Owner:** Database Architecture Team

**Sprint:** Sprint 0 – Engineering Foundation

---

# 1. Purpose

This document defines the database architecture for ClinicOS.

It describes the logical data model, entity relationships, multi-tenant strategy, indexing strategy, migration policy, auditing model, scalability considerations, and database standards.

The database architecture is designed to support rapid MVP development while remaining suitable for enterprise-scale deployment.

---

# 2. Database Technology Stack

## Primary Database

* PostgreSQL

Reason:

* ACID compliant
* Strong relational modeling
* JSONB support
* Full-text search
* Excellent indexing
* Mature ecosystem
* High scalability

---

## ORM

Prisma ORM

Responsibilities:

* Schema management
* Database migrations
* Type-safe queries
* Relationship mapping
* Seed data

---

## Cache

Redis

Used for:

* Sessions
* Rate limiting
* Job queues
* Frequently accessed data

Redis is not a source of truth.

---

## Vector Database

Prototype

* ChromaDB

Future

* Qdrant

Stores:

* Document embeddings
* Knowledge vectors
* Semantic search indexes

---

# 3. Database Design Principles

The database follows these principles:

* Multi-tenant by design
* UUID primary keys
* Soft deletes
* Audit fields
* Referential integrity
* Optimized indexing
* Minimal redundancy
* Normalized transactional model
* Extensible schema

---

# 4. Multi-Tenant Strategy

ClinicOS is a SaaS platform.

Every clinic is an independent tenant.

Every business table includes:

* tenant_id

Examples:

* doctors
* patients
* appointments
* conversations
* insurance_plans

Every query must filter by tenant_id.

Tenant isolation is enforced in:

* Repository layer
* Service layer
* Database queries

No tenant can access another tenant's data.

---

# 5. Core Database Domains

The database is organized into logical domains.

## Tenant Management

* tenants
* tenant_settings
* tenant_subscriptions (future)

---

## Identity

* users
* roles
* permissions
* user_sessions

---

## Clinic Operations

* clinics
* doctors
* doctor_specialties
* doctor_availability

---

## Patient Management

* patients
* patient_profiles
* patient_documents
* emergency_contacts

---

## Appointment Management

* appointments
* appointment_status_history
* appointment_notes

---

## AI

* conversations
* conversation_messages
* ai_requests
* ai_responses
* ai_feedback

---

## Knowledge Base

* documents
* document_chunks
* embeddings_metadata

Vectors remain inside ChromaDB.

Only metadata is stored in PostgreSQL.

---

## Insurance

* insurance_providers
* insurance_plans
* patient_insurance

---

## Notifications

* notifications
* notification_templates
* notification_logs

---

## Analytics

* dashboard_metrics
* audit_logs
* activity_logs

---

# 6. Common Entity Standards

Every transactional table contains:

* id (UUID)
* tenant_id
* created_at
* updated_at
* deleted_at (Soft Delete)
* created_by
* updated_by

This enables:

* Auditing
* Traceability
* Soft deletion
* Multi-tenancy

---

# 7. Relationships

Examples:

Tenant

↓

Clinic

↓

Doctor

↓

Appointment

↓

Patient

Another example:

Patient

↓

Conversation

↓

Conversation Messages

↓

AI Request

↓

AI Response

↓

Appointment

All relationships use foreign keys.

---

# 8. Indexing Strategy

Indexes will be created for:

Primary Keys

* id

Tenant Isolation

* tenant_id

Authentication

* email
* username

Appointments

* doctor_id
* patient_id
* appointment_date
* status

Patients

* phone_number
* email

Doctors

* specialty
* clinic_id

Knowledge

* document_id

Audit

* created_at

Composite indexes will be added where appropriate.

---

# 9. Constraints

Examples:

Email

UNIQUE

Doctor Availability

No overlapping appointment slots.

Appointments

Must reference existing patient and doctor.

Insurance

Patient insurance must belong to the same tenant.

Cascade rules are explicitly defined to prevent orphan records.

---

# 10. Migration Strategy

Schema changes are managed through Prisma Migrations.

Rules:

* Version-controlled
* Reviewed before deployment
* Backward compatible where possible
* Rollback plan required

No manual schema changes in production.

---

# 11. Seed Data

Development uses seed data.

Includes:

* Sample Clinics
* Sample Doctors
* Sample Patients
* Sample Insurance Providers
* Sample Knowledge Documents
* Sample Appointments

Production uses real customer data.

---

# 12. Audit Logging

Critical actions are recorded.

Examples:

* Login
* Appointment Created
* Appointment Cancelled
* Doctor Updated
* Patient Updated
* Document Uploaded
* Role Changed

Each audit record contains:

* User
* Tenant
* Timestamp
* Action
* Entity
* Previous Value
* New Value

---

# 13. Backup Strategy

Development

* Manual backups

Production

* Automated PostgreSQL backups
* Daily snapshots
* Point-in-time recovery
* Encrypted storage

---

# 14. Performance Strategy

Techniques include:

* Proper indexing
* Query optimization
* Pagination
* Connection pooling
* Read replicas (future)
* Redis caching

Large datasets must never be fully loaded into memory.

---

# 15. Security

Database security includes:

* Encrypted connections
* Principle of least privilege
* Parameterized queries
* ORM validation
* Secret management
* Audit logging

No credentials are stored in source code.

---

# 16. Scalability Strategy

The schema supports:

* Horizontal application scaling
* Read replicas
* Partitioning (future)
* Sharding (future)
* Multi-region deployment (future)

Business entities remain independent to support future microservices.

---

# 17. Future Database Evolution

Planned enhancements include:

* Event Store
* Data Warehouse
* OLAP Analytics
* Read Models
* Billing Database
* Reporting Database

These additions do not require redesign of the transactional schema.

---

# 18. Database Summary

ClinicOS uses PostgreSQL as the primary transactional database with Prisma ORM for schema management.

The architecture is multi-tenant, normalized, auditable, and designed for long-term scalability.

Redis complements PostgreSQL for caching and queues, while ChromaDB manages vector embeddings for AI-powered retrieval.

This database architecture supports the MVP while providing a clear path toward enterprise-scale deployment without structural redesign.

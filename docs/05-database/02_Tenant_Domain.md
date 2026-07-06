# Tenant Domain

**Project:** ClinicOS – AI Workflow Platform for Healthcare Clinics

**Version:** 1.0

**Status:** Approved

**Owner:** Platform Engineering Team

---

# 1. Purpose

The Tenant Domain is responsible for enabling ClinicOS to operate as a secure multi-tenant Software-as-a-Service (SaaS) platform.

A tenant represents an independent customer organization (such as a healthcare clinic, hospital group, dental chain, or specialty practice) that uses ClinicOS.

Each tenant has isolated data, users, configuration, branding, and operational settings.

The Tenant Domain ensures complete separation between customer organizations while allowing the platform to scale to thousands of clinics.

---

# 2. Responsibilities

The Tenant Domain is responsible for:

* Tenant registration
* Tenant configuration
* Tenant isolation
* Tenant lifecycle management
* Subscription management
* Branding and customization
* Feature management
* Usage tracking
* Organization settings

---

# 3. Business Rules

* Every clinic belongs to exactly one tenant.
* Every user belongs to exactly one tenant.
* Every doctor belongs to exactly one tenant.
* Every patient belongs to exactly one tenant.
* Every appointment belongs to exactly one tenant.
* Tenants cannot access each other's data.
* Every authenticated request must include tenant context.
* Deleting a tenant does not immediately remove business data; it follows a deactivation and retention policy.

---

# 4. Core Entities

## Tenant

Represents a customer organization.

Examples:

* Sunrise Orthopedic Clinic
* City Dental Care
* Women's Fertility Center
* Smile Dental Network
* ABC Hospital Group

A tenant owns all business resources within its organization.

---

## TenantSettings

Stores configurable settings for a tenant.

Examples:

* Timezone
* Default Language
* Currency
* Business Hours
* Appointment Duration
* Branding Theme
* Notification Preferences

These settings allow each tenant to operate independently without changing application code.

---

## Subscription

Represents the commercial relationship between ClinicOS and the tenant.

Stores:

* Subscription Plan
* Billing Cycle
* Subscription Status
* Trial Period
* Renewal Date
* Feature Limits

Although billing is outside the MVP scope, this entity prepares the platform for commercial SaaS deployment.

---

## TenantBranding

Stores branding assets and visual customization.

Examples:

* Clinic Logo
* Primary Color
* Secondary Color
* Email Header
* Login Background
* Custom Domain (Future)

This enables white-label deployments.

---

## TenantFeatureFlag

Defines which platform features are enabled for a tenant.

Examples:

* AI Chat
* Analytics Dashboard
* OCR
* Voice Assistant
* Multi-language Support

This supports gradual feature rollout and subscription-based capabilities.

---

# 5. Relationships

```text
Tenant
│
├── TenantSettings (1:1)
├── Subscription (1:1)
├── TenantBranding (1:1)
├── TenantFeatureFlags (1:N)
│
├── Users (1:N)
├── Clinics (1:N)
├── Doctors (1:N)
├── Patients (1:N)
├── Appointments (1:N)
├── Conversations (1:N)
├── Documents (1:N)
└── Notifications (1:N)
```

The Tenant is the root aggregate for almost every business entity in ClinicOS.

---

# 6. Tenant Lifecycle

Tenant Created

↓

Default Configuration

↓

Administrator Invited

↓

Subscription Activated

↓

Clinic Setup

↓

Doctors Added

↓

Patients Registered

↓

Platform Operational

↓

Subscription Renewal

↓

Suspended (if required)

↓

Archived

---

# 7. Tenant Isolation

Tenant isolation is mandatory.

Isolation is enforced at multiple layers.

## API Layer

Every authenticated request contains tenant information derived from the authenticated user.

---

## Service Layer

Business services automatically scope operations to the current tenant.

No service may query data across tenants unless explicitly designed for platform administration.

---

## Repository Layer

Every database query includes the tenant identifier.

Example:

```sql
SELECT * FROM patients
WHERE tenant_id = ?
```

---

## Database Layer

Every business table includes:

* tenant_id

This enables strict data segregation.

---

# 8. Subscription Model

The platform supports multiple subscription plans.

Examples:

Starter

* Single Clinic
* Limited Doctors
* Basic AI

Professional

* Multiple Clinics
* Advanced AI
* Analytics

Enterprise

* Unlimited Clinics
* White Label
* SSO
* Custom Integrations

Subscription enforcement occurs in the Business API, not the frontend.

---

# 9. Feature Management

Tenant features are configurable.

Examples:

* AI Chat
* RAG Knowledge Base
* Insurance Verification
* OCR Processing
* Voice Assistant
* WhatsApp Integration

Feature flags enable gradual releases without requiring code changes.

---

# 10. Branding

Each tenant can customize:

* Logo
* Brand Colors
* Email Templates
* Dashboard Theme
* Login Page
* Notification Footer

Branding data is loaded during authentication and cached for performance.

---

# 11. Security Requirements

Tenant data isolation is enforced through:

* Authentication
* Authorization
* Tenant Context
* Repository Filtering
* Audit Logging

Platform administrators may access multiple tenants only through dedicated administration interfaces with explicit permissions.

---

# 12. API Ownership

The Tenant Domain owns:

* Create Tenant
* Update Tenant
* Get Tenant Details
* Update Tenant Settings
* Update Branding
* Manage Feature Flags
* Manage Subscription
* Suspend Tenant
* Archive Tenant

---

# 13. Database Ownership

Primary tables:

* tenants
* tenant_settings
* subscriptions
* tenant_branding
* tenant_feature_flags

Every table follows the shared database standards for audit fields, UUIDs, timestamps, and soft deletion.

---

# 14. Future Evolution

The Tenant Domain is designed to support:

* Multi-region deployments
* White-label SaaS
* Marketplace integrations
* Organization hierarchies
* Multiple brands per tenant
* Enterprise SSO
* Usage-based billing
* Audit and compliance reporting

These capabilities can be added without redesigning the core tenant model.

---

# 15. Engineering Impact

The Tenant Domain affects nearly every subsystem:

* Authentication
* Authorization
* Clinic Management
* Patient Management
* Doctor Management
* Appointment Scheduling
* AI Context Isolation
* Knowledge Base Retrieval
* Notifications
* Analytics
* Audit Logging

Every new business module must be evaluated for tenant ownership before implementation.

---

# Conclusion

The Tenant Domain establishes the multi-tenant foundation of ClinicOS. It guarantees secure data isolation, configurable organization settings, subscription readiness, and scalable SaaS architecture. By making the tenant the root organizational boundary, ClinicOS can grow from serving a single clinic to supporting thousands of independent healthcare organizations without architectural changes.

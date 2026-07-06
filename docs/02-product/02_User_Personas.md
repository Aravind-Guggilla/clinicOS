# User Personas

**Document Version:** 1.0
**Status:** Approved
**Owner:** Product Team / UX Team
**Sprint:** Sprint 0 – Product Foundation

---

# Purpose

This document defines the primary users of ClinicOS.

Understanding each user is essential for designing workflows, interfaces, permissions, APIs, and AI interactions.

Every feature in ClinicOS must serve one or more of these personas.

---

# Persona 1 – Patient

## Description

The patient is the primary user who visits the clinic website seeking medical assistance or an appointment.

Patients interact with ClinicOS before they interact with clinic staff.

---

## Goals

* Find the right doctor
* Book appointments quickly
* Ask medical preparation questions
* Upload required documents
* Verify insurance compatibility
* Receive appointment confirmations
* Track appointment status

---

## Pain Points

* Long waiting times
* Difficulty contacting clinics
* Repeating information multiple times
* Unclear appointment availability
* Confusing insurance requirements

---

## Permissions

Patients can:

* Register/Login
* Chat with AI
* Book appointments
* Cancel appointments
* Upload reports
* View appointment history
* Update personal profile

Patients cannot:

* View other patients
* Access staff dashboard
* Modify doctor schedules
* Change clinic settings

---

# Persona 2 – Receptionist

## Description

Receptionists manage day-to-day patient operations.

ClinicOS is designed primarily to reduce their repetitive administrative workload.

---

## Goals

* Handle more patients with less manual effort
* Respond faster
* Approve appointments
* Review AI conversations
* Resolve urgent requests

---

## Responsibilities

* View patient requests
* Confirm appointments
* Manage schedules
* Review emergency alerts
* Upload patient documents
* Contact patients when required

---

## Permissions

Receptionists can:

* View patient records
* Manage appointments
* View AI-generated responses
* Override appointment suggestions
* Access operational dashboard

They cannot:

* Modify clinic configuration
* Add doctors
* Manage billing settings
* Delete clinics

---

# Persona 3 – Doctor

## Description

Doctors focus on clinical care rather than administration.

ClinicOS provides them with relevant patient information and schedules.

---

## Goals

* View daily appointments
* Access uploaded reports
* Review patient summaries
* Manage availability
* Update consultation status

---

## Permissions

Doctors can:

* View assigned appointments
* View patient details
* Update appointment status
* Set availability
* Add consultation notes (future)

Doctors cannot:

* Configure AI
* Manage insurance
* Access other doctors' schedules (unless permitted)
* Modify clinic settings

---

# Persona 4 – Clinic Administrator

## Description

Clinic administrators configure and manage their clinic within ClinicOS.

---

## Goals

* Configure doctors
* Manage specialties
* Upload knowledge base documents
* Configure insurance providers
* View analytics
* Manage staff accounts

---

## Responsibilities

* Add/Edit doctors
* Add/Edit receptionists
* Configure workflows
* Upload clinic policies
* Manage operational settings

---

## Permissions

Clinic Administrators can:

* Manage doctors
* Manage staff
* Configure insurance
* Upload RAG documents
* Access analytics
* Configure workflows

They cannot:

* Manage other clinics
* Access platform-wide settings

---

# Persona 5 – Platform Administrator

## Description

Platform administrators manage the entire ClinicOS platform.

This role exists only for the SaaS provider.

---

## Goals

* Monitor platform health
* Manage tenants
* Support clinics
* Configure platform settings
* Monitor security

---

## Responsibilities

* Create clinics
* Suspend clinics
* View platform analytics
* Manage subscriptions (future)
* Monitor system performance

---

## Permissions

Platform Administrators have full system access.

Their responsibilities include platform operations rather than clinic operations.

---

# Persona Relationships

Platform Administrator

↓

Clinic Administrator

↓

Receptionist

↓

Doctor

↓

Patient

Each role has increasing operational responsibility while maintaining strict permission boundaries.

---

# Design Principles

ClinicOS follows the principle of least privilege.

Each user receives only the permissions required to perform their responsibilities.

This improves security, usability, and maintainability.

---

# Future Personas

The platform is designed to support additional personas in future releases, including:

* Nurse
* Insurance Coordinator
* Billing Specialist
* Laboratory Technician
* Referral Coordinator
* Care Manager
* Telemedicine Operator

These roles are intentionally excluded from Version 1 to maintain MVP focus.

---

# Impact on Engineering

This document directly influences:

* Authentication
* Authorization (RBAC)
* Database schema
* Navigation menus
* Dashboard layouts
* API authorization
* Workflow ownership
* Audit logging

Every engineering component must respect the permissions and responsibilities defined in this document.

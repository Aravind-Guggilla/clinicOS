# Doctor Domain

**Project:** ClinicOS – AI Workflow Platform for Healthcare Clinics

**Version:** 1.0

**Status:** Approved

**Owner:** Clinical Operations Engineering Team

---

# 1. Purpose

The Doctor Domain represents healthcare professionals who provide medical services through ClinicOS.

It manages doctor profiles, qualifications, specialties, clinic assignments, schedules, availability, consultation types, and operational status.

This domain is the foundation for appointment scheduling, AI doctor recommendations, workload balancing, calendar management, analytics, and future telemedicine capabilities.

---

# 2. Responsibilities

The Doctor Domain is responsible for:

* Doctor profile management
* Clinic assignment
* Department assignment
* Specialty assignment
* Availability management
* Working schedules
* Leave management
* Consultation configuration
* Appointment capacity
* Doctor search
* Doctor activation/deactivation

---

# 3. Business Rules

* Every doctor belongs to one tenant.
* A doctor may work in one or more clinics.
* A doctor may belong to multiple departments.
* A doctor may have multiple specialties.
* Every appointment is assigned to exactly one doctor.
* Doctors cannot be booked outside their availability.
* Leave periods automatically block appointment slots.
* Doctors can be temporarily unavailable without deleting their profile.

---

# 4. Aggregate Root

The **Doctor** entity is the aggregate root.

```text
Doctor
│
├── DoctorProfile
├── DoctorQualification
├── DoctorLicense
├── DoctorClinicAssignment
├── DoctorDepartmentAssignment
├── DoctorSpecialtyAssignment
├── DoctorAvailability
├── DoctorLeave
├── ConsultationType
└── DoctorDocument
```

All modifications to doctor-related data should occur through the Doctor aggregate.

---

# 5. Core Entities

## Doctor

Stores the primary identity of a healthcare professional.

Example attributes:

* Doctor Code
* First Name
* Last Name
* Email
* Phone
* Status
* Experience
* Consultation Fee
* Years of Practice

---

## DoctorProfile

Stores additional profile information.

Examples:

* Biography
* Languages Spoken
* Profile Photo
* Professional Summary

---

## DoctorQualification

Stores educational qualifications.

Examples:

* MBBS
* MD
* MS
* DNB
* Fellowship

A doctor may have multiple qualifications.

---

## DoctorLicense

Stores professional registration details.

Examples:

* Medical Council Number
* Issuing Authority
* Issue Date
* Expiry Date

---

## DoctorClinicAssignment

Maps doctors to clinics.

Supports doctors working across multiple clinic locations.

---

## DoctorDepartmentAssignment

Maps doctors to departments.

Examples:

* Orthopedics
* Cardiology
* Pediatrics

---

## DoctorSpecialtyAssignment

Maps doctors to specialties.

Examples:

* Sports Medicine
* Joint Replacement
* Interventional Cardiology

The AI Service uses specialties during doctor matching.

---

## DoctorAvailability

Defines recurring working hours.

Example:

Monday

09:00 – 17:00

Tuesday

10:00 – 18:00

Supports recurring weekly schedules.

---

## DoctorLeave

Stores planned leave.

Examples:

* Vacation
* Conference
* Medical Leave
* Emergency Leave

Appointment booking automatically excludes these periods.

---

## ConsultationType

Defines supported consultation modes.

Examples:

* In-Person
* Video Consultation
* Follow-up
* Emergency Consultation

---

## DoctorDocument

Stores professional documents.

Examples:

* Medical License
* Degree Certificate
* Identity Proof
* Certifications

Files are stored in object storage.

Only metadata is stored in PostgreSQL.

---

# 6. Relationships

```text
Tenant
│
└── Doctor
      │
      ├── Profile (1:1)
      ├── Qualifications (1:N)
      ├── Licenses (1:N)
      ├── Clinics (N:M)
      ├── Departments (N:M)
      ├── Specialties (N:M)
      ├── Availability (1:N)
      ├── Leave (1:N)
      ├── Consultation Types (1:N)
      ├── Documents (1:N)
      └── Appointments (1:N)
```

---

# 7. Doctor Lifecycle

Doctor Created

↓

Professional Verification

↓

Clinic Assignment

↓

Department Assignment

↓

Specialty Assignment

↓

Availability Configured

↓

Appointments Enabled

↓

Active Practice

↓

Temporary Leave (Optional)

↓

Retired / Archived

---

# 8. AI Integration

The AI Service uses doctor information for:

* Specialty matching
* Doctor recommendation
* Appointment suggestions
* Availability lookup
* Knowledge retrieval
* Response generation

The AI Service can recommend doctors but cannot assign appointments directly.

---

# 9. Scheduling Rules

Appointment slots are generated from:

* Clinic operating hours
* Doctor availability
* Doctor leave
* Consultation duration
* Existing appointments

The scheduling engine is deterministic and does not rely on LLM-generated availability.

---

# 10. Security Requirements

Only authorized users may:

* Create doctors
* Update profiles
* Modify schedules
* Configure leave
* Upload documents

All changes are recorded in audit logs.

---

# 11. API Ownership

The Doctor Domain owns:

* Create Doctor
* Update Doctor
* Get Doctor Details
* Assign Clinic
* Assign Department
* Assign Specialty
* Configure Availability
* Manage Leave
* Upload Documents
* Search Doctors

---

# 12. Database Ownership

Primary tables:

* doctors
* doctor_profiles
* doctor_qualifications
* doctor_licenses
* doctor_clinic_assignments
* doctor_department_assignments
* doctor_specialty_assignments
* doctor_availability
* doctor_leaves
* consultation_types
* doctor_documents

---

# 13. Future Evolution

The Doctor Domain is designed to support:

* Telemedicine
* Multi-clinic scheduling
* AI workload balancing
* Skill-based doctor matching
* Procedure scheduling
* Calendar synchronization
* External provider directories
* Performance analytics

---

# 14. Engineering Deliverables

This domain will generate:

## Database

* Prisma models
* PostgreSQL tables
* Foreign keys
* Indexes

## Backend

* Express module
* Routes
* Controllers
* Services
* Repositories
* Validators
* Workflow handlers

## Frontend

* Doctor Management
* Availability Calendar
* Leave Management
* Profile Management
* Qualification Management

## AI

* Doctor Matching
* Specialty Retrieval
* Scheduling Context
* Knowledge Context

## Testing

* Unit Tests
* Integration Tests
* API Tests

---

# Conclusion

The Doctor Domain models healthcare professionals as a complete aggregate rather than a single table. It provides the operational foundation for appointment scheduling, AI recommendations, analytics, and future healthcare workflows while remaining flexible enough to support multi-clinic organizations and enterprise-scale deployments.

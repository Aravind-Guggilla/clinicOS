# User Journeys

**Document Version:** 1.0
**Status:** Approved
**Owner:** Product Team / UX Team
**Sprint:** Sprint 0 – Product Foundation

---

# Purpose

This document defines the end-to-end journeys for each user role within ClinicOS.

A user journey describes how a user interacts with the platform to achieve a goal. These journeys serve as the blueprint for user interface design, backend APIs, AI workflows, database relationships, and testing.

Every major feature in ClinicOS should support one or more of these journeys.

---

# Journey 1 – Patient Journey

## Goal

Book an appointment quickly while receiving accurate guidance from the AI assistant.

---

## Workflow

1. Patient visits the clinic website.
2. Opens the AI chat.
3. Describes symptoms or asks a question.
4. AI identifies intent and specialty.
5. AI checks the clinic knowledge base (RAG).
6. AI asks for any missing information.
7. Patient provides insurance details.
8. System verifies insurance compatibility.
9. System retrieves available doctors and appointment slots.
10. Patient selects a preferred slot.
11. Appointment is created.
12. Confirmation is displayed and stored.
13. Staff dashboard is updated automatically.

---

## Happy Path

Patient → AI → Insurance → Doctor Match → Appointment → Confirmation

---

## Alternative Paths

* Insurance not accepted.
* No appointment slots available.
* Emergency symptoms detected.
* Patient exits before completing the booking.

---

# Journey 2 – Receptionist Journey

## Goal

Manage patient operations efficiently with AI assistance.

---

## Workflow

1. Receptionist logs in.
2. Opens the Staff Dashboard.
3. Reviews new conversations.
4. Reviews AI-generated summaries.
5. Reviews emergency alerts.
6. Confirms or adjusts appointments.
7. Contacts patients if needed.
8. Updates appointment status.
9. Tracks daily workload and metrics.

---

## Happy Path

Dashboard → Review → Approve → Schedule → Complete

---

## Alternative Paths

* Manual appointment override.
* Escalate urgent cases.
* Reject appointment requests.
* Modify booking details.

---

# Journey 3 – Doctor Journey

## Goal

View scheduled consultations and relevant patient information.

---

## Workflow

1. Doctor logs in.
2. Opens today's schedule.
3. Reviews patient summaries.
4. Views uploaded reports.
5. Conducts consultation.
6. Updates appointment status.
7. Marks consultation complete.

---

## Happy Path

Schedule → Patient Review → Consultation → Complete

---

## Alternative Paths

* Appointment cancelled.
* Patient no-show.
* Doctor unavailable.

---

# Journey 4 – Clinic Administrator Journey

## Goal

Configure and manage the clinic platform.

---

## Workflow

1. Administrator logs in.
2. Manages doctors.
3. Configures specialties.
4. Updates insurance providers.
5. Uploads knowledge base documents.
6. Reviews analytics.
7. Monitors workflow performance.

---

## Happy Path

Manage → Configure → Monitor → Optimize

---

## Alternative Paths

* Add a new doctor.
* Remove staff access.
* Update clinic policies.
* Upload new RAG documents.

---

# Journey 5 – Platform Administrator Journey

## Goal

Manage multiple clinics within the SaaS platform.

---

## Workflow

1. Platform Admin logs in.
2. Views tenant dashboard.
3. Creates or manages clinics.
4. Monitors system health.
5. Reviews platform analytics.
6. Supports tenant issues.
7. Maintains platform configuration.

---

# AI Workflow Journey

This represents the internal orchestration performed by ClinicOS.

Patient Message

↓

Intent Detection

↓

Workflow Selection

↓

Knowledge Retrieval (RAG)

↓

Insurance Verification

↓

Doctor Matching

↓

Appointment Availability

↓

Business Rule Validation

↓

Response Generation

↓

Appointment Creation

↓

Dashboard Update

↓

Analytics Logging

---

# Emergency Workflow

Patient Message

↓

Emergency Detection

↓

Critical Classification

↓

Stop AI Conversation

↓

Notify Receptionist

↓

Highlight Emergency Dashboard

↓

Manual Staff Intervention

---

# Insurance Workflow

Patient Insurance

↓

Insurance Validation

↓

Accepted?

↓

Yes → Continue Booking

↓

No → Inform Patient & Suggest Alternatives

---

# Appointment Workflow

Patient Request

↓

Doctor Selection

↓

Available Slot Search

↓

Slot Validation

↓

Appointment Creation

↓

Confirmation

↓

Calendar Update

↓

Staff Notification

---

# Knowledge Base Workflow (RAG)

Patient Question

↓

Intent Detection

↓

Retrieve Relevant Documents

↓

Context Assembly

↓

AI Response Generation

↓

Display Verified Answer

---

# End-to-End MVP Workflow

Patient Opens Website

↓

AI Conversation

↓

Intent Detection

↓

Knowledge Retrieval

↓

Insurance Verification

↓

Doctor Matching

↓

Appointment Scheduling

↓

Appointment Confirmation

↓

Dashboard Update

↓

Analytics

This is the primary workflow that demonstrates the value of ClinicOS.

---

# Engineering Impact

These journeys directly influence:

* Screen navigation
* UI wireframes
* Backend APIs
* Database schema
* Workflow engine
* AI orchestration
* Notification service
* RBAC permissions
* Test scenarios
* Acceptance criteria

Every module implemented in ClinicOS must support one or more of these documented user journeys.

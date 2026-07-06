# Identity Domain

**Project:** ClinicOS – AI Workflow Platform for Healthcare Clinics

**Version:** 1.0

**Status:** Approved

**Owner:** Platform Engineering Team

---

# 1. Purpose

The Identity Domain is responsible for authentication, authorization, user lifecycle management, and access control across the ClinicOS platform.

It provides a secure identity layer that supports multiple clinics (tenants), multiple user roles, and future enterprise authentication mechanisms.

This domain is independent of any specific business workflow and acts as the foundation for all protected operations.

---

# 2. Responsibilities

The Identity Domain is responsible for:

* User registration
* User authentication
* User authorization
* Password management
* Session management
* Refresh token management
* Role management
* Permission management
* Account status management
* Login history
* Security auditing

---

# 3. Business Rules

* Every user belongs to exactly one tenant.
* Every user can have one or more roles.
* Every role contains one or more permissions.
* Authentication must occur before any protected operation.
* Authorization must be validated for every request.
* Users cannot access data belonging to another tenant.
* Passwords are never stored in plain text.
* Sessions can be revoked at any time.
* Soft delete is used for user accounts.

---

# 4. Core Entities

## User

Represents any authenticated person using ClinicOS.

Examples:

* Patient
* Receptionist
* Doctor
* Clinic Administrator
* Platform Administrator

---

## Role

Defines a collection of permissions.

Examples:

* PATIENT
* RECEPTIONIST
* DOCTOR
* CLINIC_ADMIN
* PLATFORM_ADMIN

---

## Permission

Represents a single capability.

Examples:

* CREATE_APPOINTMENT
* UPDATE_PATIENT
* VIEW_ANALYTICS
* MANAGE_DOCTORS
* UPLOAD_DOCUMENT

---

## UserRole

Maps users to roles.

Supports future multi-role users.

---

## RefreshToken

Stores active refresh tokens.

Supports:

* Logout
* Token rotation
* Session expiration
* Multiple devices

---

## LoginHistory

Tracks authentication activity.

Examples:

* Login
* Logout
* Failed Login
* Password Reset

Useful for security auditing.

---

# 5. Relationships

```text
Tenant

↓

Users

↓

UserRoles

↓

Roles

↓

Permissions
```

A User may have multiple Roles.

A Role may contain multiple Permissions.

---

# 6. User Lifecycle

User Created

↓

Email Verification (Future)

↓

Active

↓

Login

↓

Refresh Token

↓

Protected Operations

↓

Logout

↓

Token Revoked

↓

Archived

---

# 7. Authentication Flow

Login Request

↓

Validate Credentials

↓

Generate JWT

↓

Generate Refresh Token

↓

Store Refresh Token

↓

Return Tokens

↓

Access Protected APIs

---

# 8. Authorization Flow

Incoming Request

↓

JWT Validation

↓

Load User

↓

Load Roles

↓

Load Permissions

↓

RBAC Validation

↓

Allow / Deny

---

# 9. Account Status

Possible values:

* ACTIVE
* INACTIVE
* LOCKED
* PENDING_VERIFICATION
* SUSPENDED
* DELETED

---

# 10. Security Requirements

Passwords:

* bcrypt hashing

Sessions:

* Refresh token rotation

JWT:

* Short expiration

Rate Limiting:

* Login endpoints

Audit Logging:

* Every authentication event

Future:

* MFA
* OAuth2
* OpenID Connect
* SSO

---

# 11. API Ownership

This domain owns:

* Login
* Logout
* Register
* Refresh Token
* Forgot Password
* Reset Password
* Change Password
* Profile
* Role Management
* Permission Management

---

# 12. Database Ownership

Primary tables:

* users
* roles
* permissions
* user_roles
* refresh_tokens
* login_history

---

# 13. Engineering Impact

This domain directly influences:

* Authentication middleware
* RBAC middleware
* API security
* Session handling
* Audit logging
* Frontend authentication flow
* Protected routes
* User management dashboard

---

# Conclusion

The Identity Domain provides the security foundation for ClinicOS. It centralizes authentication and authorization while remaining isolated from business workflows, ensuring that every request is securely validated before interacting with platform resources.

# Campus Parking Management System
## Product Requirements Document (PRD)

**Version:** 1.0  
**Status:** Draft for Intern Project  
**Audience:** Intern, Mentor, Reviewer  
**Primary Goal:** Teach end-to-end full-stack software development and engineering process using a realistic campus parking domain.  
**Primary Stack:** PostgreSQL, Redis, Node.js/TypeScript, Flutter

---

## 1. Executive Summary

The Campus Parking Management System is a learning-oriented full-stack application that models a real campus parking workflow.

The system allows students, lecturers, employees, and parking staff to interact with campus parking facilities. End users can register vehicles, discover parking areas, inspect live availability, start a parking session, leave a parking area, and view parking history. Administrative users can manage parking areas, capacity, availability, active sessions, and operational records.

The product is intentionally constrained so that an intern can complete meaningful slices of functionality while learning how product requirements are transformed into architecture, database design, API contracts, frontend behavior, testing strategies, deployment plans, and operational procedures.

The project should be treated as a miniature production system rather than a CRUD exercise.

---

## 2. Problem Statement

Campus parking information is often fragmented or unavailable in real time. Drivers may not know:

- which parking areas are open;
- how many spaces remain;
- whether their vehicle is already registered;
- whether they currently have an active parking session;
- how long they have been parked;
- their previous parking history.

Parking administrators may lack a single operational view of:

- current occupancy;
- active parking sessions;
- parking usage by area;
- manually corrected sessions;
- user and vehicle activity;
- capacity configuration.

This project provides a simplified digital workflow for those operations.

---

## 3. Product Vision

Build a small but technically realistic parking platform in which:

1. PostgreSQL is the authoritative data store.
2. Redis improves responsiveness and supports transient state or event distribution.
3. Node.js exposes a versioned backend API.
4. Flutter provides the end-user and administrator application experience.
5. Core parking operations remain correct under concurrent requests.
6. Every feature is designed, implemented, tested, reviewed, and documented as part of a realistic engineering workflow.

---

## 4. Learning Outcomes

The intern should finish the project able to explain and demonstrate:

### Product and Requirement Skills
- translating requirements into use cases;
- identifying scope and non-scope;
- writing acceptance criteria;
- identifying edge cases before implementation;
- distinguishing user-facing behavior from implementation details.

### Backend Engineering
- Node.js service structure;
- TypeScript domain modeling;
- REST API design;
- DTO validation;
- authentication;
- authorization;
- persistence boundaries;
- service/repository separation;
- error handling;
- request tracing.

### Database Engineering
- relational modeling;
- primary and foreign keys;
- unique constraints;
- check constraints;
- indexing;
- transactions;
- row locking;
- concurrency protection;
- migration strategy;
- soft deletion;
- historical data retention.

### Redis
- caching;
- cache invalidation;
- TTL;
- pub/sub or streams;
- transient vs durable state;
- failure handling;
- eventual consistency.

### Flutter
- navigation;
- form handling;
- authentication state;
- API integration;
- state management;
- loading/error/empty states;
- optimistic vs pessimistic updates;
- widget testing.

### Software Delivery
- branching strategy;
- pull requests;
- reviews;
- CI;
- migrations;
- tests;
- environment configuration;
- deployment;
- logging;
- monitoring;
- incident thinking.

---

## 5. Personas

### 5.1 Campus Driver

Examples:
- student;
- lecturer;
- staff member.

Needs:
- register one or more vehicles;
- check parking availability;
- start parking;
- leave parking;
- view current session;
- view parking history.

### 5.2 Parking Staff

Needs:
- search active sessions;
- inspect vehicle and driver information;
- manually close invalid sessions;
- inspect occupancy;
- investigate operational issues.

### 5.3 Parking Administrator

Needs:
- create and update parking areas;
- configure capacity;
- open/close parking areas;
- view operational dashboard;
- inspect parking activity;
- review audit history.

### 5.4 Mentor / Reviewer

Not a product user, but an important project stakeholder.

Needs:
- inspect the intern's design;
- review technical decisions;
- validate implementation quality;
- challenge reasoning;
- verify that requirements are met.

---

## 6. Scope

### 6.1 MVP Scope

The MVP includes:

- user registration and login;
- role-based access control;
- vehicle registration;
- vehicle management;
- parking-area browsing;
- live capacity information;
- parking check-in;
- parking checkout;
- active-session display;
- parking history;
- administrator parking-area management;
- parking session search;
- audit logs;
- API documentation;
- automated tests;
- Docker-based local environment.

### 6.2 Phase 2 Scope

Potential extensions:

- real-time occupancy updates;
- reservation;
- parking permits;
- QR code check-in;
- notifications;
- reporting;
- metrics dashboard.

### 6.3 Explicit Non-Goals

Do not implement in the initial project:

- payments;
- license plate recognition;
- physical barriers;
- production-grade IoT devices;
- per-space navigation;
- dynamic pricing;
- machine-learning parking forecasts;
- multi-region deployment;
- external identity provider integration;
- government vehicle registries;
- complex permit enforcement.

---

## 7. Product Principles

### 7.1 PostgreSQL Is the Source of Truth

Redis must never be the only source of persistent parking state.

If Redis is lost, the system must still reconstruct correct parking information from PostgreSQL.

### 7.2 Correctness Before Optimization

A parking session must be correct in PostgreSQL before cache optimization is introduced.

### 7.3 Backend Enforces Business Rules

Flutter may validate input for user experience, but business rules must be enforced by the backend.

### 7.4 Historical Data Must Remain Explainable

Completed parking sessions should not be destructively mutated without auditability.

### 7.5 Errors Are Part of the Contract

Error responses must be predictable, structured, and documented.

---

## 8. Core Domain Model

Core entities:

- User
- Vehicle
- ParkingArea
- ParkingSession
- AuditLog
- RefreshToken or SessionToken

Optional future entities:

- Reservation
- Permit
- Violation
- ParkingZone
- ParkingSpace
- Notification

### 8.1 User

Key fields:

- id
- email
- password_hash
- display_name
- role
- status
- created_at
- updated_at

Roles:

- USER
- STAFF
- ADMIN

User status:

- ACTIVE
- DISABLED

### 8.2 Vehicle

Key fields:

- id
- user_id
- license_plate
- vehicle_type
- brand
- model
- color
- created_at
- updated_at
- deleted_at

Vehicle type examples:

- CAR
- MOTORCYCLE
- OTHER

### 8.3 ParkingArea

Key fields:

- id
- name
- description
- capacity
- status
- location_description
- created_at
- updated_at

Status:

- OPEN
- CLOSED
- MAINTENANCE

### 8.4 ParkingSession

Key fields:

- id
- user_id
- vehicle_id
- parking_area_id
- started_at
- ended_at
- status
- checkout_reason
- created_at
- updated_at

Status:

- ACTIVE
- COMPLETED
- CANCELLED
- MANUALLY_CLOSED

### 8.5 AuditLog

Key fields:

- id
- actor_user_id
- action
- resource_type
- resource_id
- metadata
- request_id
- created_at

---

## 9. User Journeys

### 9.1 First-Time Driver

1. Register account.
2. Login.
3. Register vehicle.
4. View parking areas.
5. Select an open area.
6. View available capacity.
7. Start parking.
8. App displays active parking session.
9. User leaves parking.
10. User checks out.
11. Session moves to history.

### 9.2 Returning Driver

1. Login.
2. App restores session.
3. If an active parking session exists, it is shown prominently.
4. User checks out when leaving.
5. History is updated.

### 9.3 Parking Staff Investigation

1. Login as STAFF.
2. Search license plate.
3. Inspect active or completed sessions.
4. If a session is stale or invalid, manually close it.
5. An audit log is generated.

### 9.4 Administrator Capacity Update

1. Login as ADMIN.
2. Open parking area management.
3. Select parking area.
4. Update capacity.
5. System validates that new capacity is not lower than current occupancy unless an explicit override policy is supported.
6. Change is persisted.
7. Cache is invalidated.
8. Audit record is created.

---

## 10. Functional Requirements

### FR-001 Register Account

The system shall allow a user to register with:

- email;
- password;
- display name.

Acceptance criteria:

- duplicate email returns a deterministic conflict error;
- password is hashed before persistence;
- password hash is never returned by an API;
- invalid email format is rejected;
- password policy is validated server-side.

### FR-002 Login

The system shall allow a user to login using valid credentials.

Acceptance criteria:

- valid credentials return an access token;
- invalid credentials return a generic authentication error;
- disabled users cannot login;
- authentication failures must not reveal whether an email exists.

### FR-003 Authorization

The backend shall enforce role-based access.

Examples:

- USER cannot create parking areas;
- STAFF cannot change administrator-only configuration;
- ADMIN can perform all staff operations.

### FR-010 Register Vehicle

A user may register a vehicle.

Acceptance criteria:

- vehicle ownership is linked to the authenticated user;
- duplicate active license plates are rejected;
- invalid fields are rejected;
- normalized plate value may be stored for search.

### FR-011 View Vehicles

A user may view only their own vehicles.

### FR-012 Update Vehicle

A user may update permitted fields.

Constraints:

- active parking may restrict plate changes;
- ownership cannot be changed by normal users.

### FR-013 Delete Vehicle

Deleting a vehicle should be implemented as soft deletion if historical sessions reference that vehicle.

The operation must fail when the vehicle has an active parking session.

### FR-020 View Parking Areas

Users can list parking areas.

Each item should include:

- id;
- name;
- status;
- capacity;
- occupied;
- available;
- location description.

### FR-021 View Parking Area Detail

The detail view should include:

- name;
- description;
- status;
- capacity;
- occupied;
- available;
- location description.

### FR-022 Create Parking Area

ADMIN only.

Required:

- name;
- capacity;
- status;
- location description.

### FR-023 Update Parking Area

ADMIN only.

Editable:

- name;
- description;
- capacity;
- status;
- location description.

### FR-024 Close Parking Area

ADMIN may mark an area CLOSED.

CLOSED areas:

- remain visible;
- reject new parking sessions;
- may still contain active sessions that need to leave.

### FR-030 Start Parking

A user starts parking by selecting:

- one owned vehicle;
- one parking area.

Validation:

- parking area exists;
- parking area is OPEN;
- vehicle belongs to user;
- vehicle is not deleted;
- vehicle has no active parking session;
- parking area has capacity;
- operation is concurrency-safe.

Expected success:

- one ACTIVE parking session is created;
- occupancy increases logically;
- availability cache is invalidated or updated;
- response contains session details.

### FR-031 Prevent Over-Capacity

The system must guarantee:

`active_sessions(parking_area) <= capacity`

This invariant must hold under concurrent requests.

### FR-032 Prevent Duplicate Active Session Per Vehicle

The system must guarantee:

`active_session_count(vehicle) <= 1`

This should be protected by database constraints where practical, not only application checks.

### FR-033 Checkout

A user may checkout their own active session.

Expected:

- ended_at is set;
- status changes from ACTIVE to COMPLETED;
- operation is atomic;
- parking availability changes;
- cache is invalidated or updated.

### FR-034 Idempotent Checkout Behavior

Repeated checkout requests should not create inconsistent state.

Recommended behavior:

- return the existing completed state when the same session is already completed; or
- return a deterministic `SESSION_ALREADY_COMPLETED` conflict.

The project team must choose and document one behavior.

### FR-035 Current Session

A user can retrieve their active parking session.

### FR-036 Parking History

A user can view completed sessions with pagination.

Filters may include:

- date range;
- vehicle;
- parking area.

### FR-040 Staff Search

STAFF and ADMIN can search sessions using:

- license plate;
- user email;
- parking area;
- status;
- date range.

### FR-041 Manual Close

STAFF and ADMIN can manually close an active session.

Required input:

- reason.

The action must create an audit record.

### FR-050 Admin Dashboard

Dashboard metrics:

- parking areas;
- total capacity;
- occupied;
- available;
- active sessions;
- today's completed sessions.

Metric definitions must be documented.

### FR-060 Audit Logging

Generate audit logs for:

- parking area create;
- parking area update;
- capacity update;
- parking area status update;
- manual session close;
- user disable;
- user role change if implemented.

---

## 11. Business Rules and Invariants

### BR-001 Active Session Uniqueness

A vehicle can have at most one active parking session.

### BR-002 Capacity Invariant

The number of active sessions cannot exceed configured capacity.

### BR-003 Parking Area Status

Only OPEN areas accept new sessions.

### BR-004 Historical Integrity

Completed sessions remain queryable after a vehicle is soft-deleted.

### BR-005 Ownership

A user may operate only on vehicles they own.

### BR-006 Capacity Reduction

An administrator must not reduce capacity below current occupancy unless a special override workflow is explicitly implemented.

### BR-007 Checkout Ownership

A user may checkout only their own parking session.

STAFF and ADMIN use a separate manual-close action.

### BR-008 Auditability

Administrative corrections must be attributable to an authenticated actor.

---

## 12. API Requirements

### 12.1 Base Path

`/api/v1`

### 12.2 Response Conventions

Successful responses should use consistent JSON naming.

Error responses:

```json
{
  "error": {
    "code": "PARKING_FULL",
    "message": "The selected parking area is full.",
    "requestId": "req_123"
  }
}
```

### 12.3 Expected Endpoints

Authentication:

- `POST /auth/register`
- `POST /auth/login`
- `POST /auth/refresh`
- `POST /auth/logout`

Current user:

- `GET /me`

Vehicles:

- `GET /vehicles`
- `POST /vehicles`
- `GET /vehicles/:id`
- `PATCH /vehicles/:id`
- `DELETE /vehicles/:id`

Parking:

- `GET /parking-areas`
- `GET /parking-areas/:id`

Sessions:

- `POST /parking-sessions`
- `GET /parking-sessions`
- `GET /parking-sessions/:id`
- `GET /me/active-parking-session`
- `POST /parking-sessions/:id/checkout`

Admin/staff:

- `GET /admin/parking-sessions`
- `POST /admin/parking-sessions/:id/manual-close`
- `GET /admin/dashboard`
- `POST /admin/parking-areas`
- `PATCH /admin/parking-areas/:id`

---

## 13. Error Model

Recommended errors:

- INVALID_REQUEST
- INVALID_CREDENTIALS
- UNAUTHORIZED
- FORBIDDEN
- USER_DISABLED
- VEHICLE_NOT_FOUND
- VEHICLE_ALREADY_REGISTERED
- PARKING_AREA_NOT_FOUND
- PARKING_AREA_CLOSED
- PARKING_FULL
- ACTIVE_SESSION_EXISTS
- SESSION_NOT_FOUND
- SESSION_ALREADY_COMPLETED
- CAPACITY_BELOW_OCCUPANCY
- INTERNAL_ERROR

Recommended HTTP mapping:

- 400: malformed or invalid input;
- 401: missing/invalid authentication;
- 403: authenticated but forbidden;
- 404: resource not found;
- 409: state conflict;
- 422: optional if validation policy distinguishes semantic validation;
- 500: unexpected server error.

---

## 14. Data Requirements

### 14.1 Persistence

PostgreSQL stores:

- accounts;
- vehicles;
- parking areas;
- parking sessions;
- audit logs;
- refresh tokens if refresh tokens are used.

### 14.2 Redis

Redis may store:

- parking availability cache;
- rate-limiting counters;
- short-lived authentication metadata;
- pub/sub events;
- ephemeral locks only if justified.

Redis must not be the sole store for:

- users;
- vehicles;
- parking sessions;
- parking-area configuration.

### 14.3 Data Retention

For the intern project:

- completed parking sessions are retained indefinitely;
- audit logs are retained indefinitely;
- deleted vehicles remain logically deleted;
- access tokens should be short-lived.

---

## 15. Concurrency Requirements

The intern must demonstrate correctness for the following case:

Parking area:
- capacity = 1;
- active sessions = 0.

Two clients concurrently request parking.

Expected:
- exactly one succeeds;
- one receives a conflict;
- database remains valid.

The implementation should consider:

- transaction isolation;
- row-level locking;
- atomic updates;
- unique constraints;
- partial indexes.

The selected approach must be documented in an RFC.

---

## 16. Cache Requirements

Availability reads may be cached.

Example cache key:

`parking:availability:{parkingAreaId}`

Recommended strategy:

- cache-aside;
- TTL 15–60 seconds;
- explicit invalidation after check-in, checkout, and capacity updates.

Requirements:

- cache miss falls back to PostgreSQL;
- Redis outage must not prevent core parking operations;
- stale cache must not be trusted for final admission decisions;
- admission must validate against PostgreSQL transaction state.

---

## 17. Flutter Requirements

### 17.1 Required Screens

- Splash / startup
- Login
- Register
- Home
- Parking area list
- Parking area detail
- Vehicle list
- Add vehicle
- Edit vehicle
- Active parking session
- Parking history
- Profile

Admin/staff:
- Dashboard
- Parking area management
- Session search
- Session detail
- Manual close

### 17.2 Screen States

Each network-driven screen should support:

- loading;
- success;
- empty;
- recoverable error;
- unauthorized/session expired.

### 17.3 State Management

The intern must choose and document one approach.

Candidates:

- Riverpod
- Bloc
- Provider

The decision should address:

- authentication state;
- server state;
- loading/error states;
- refreshing;
- navigation after logout.

### 17.4 Local Storage

Sensitive tokens should be stored using an appropriate secure storage mechanism.

---

## 18. Security Requirements

### Authentication
- password hashing with a modern KDF;
- short-lived access tokens;
- refresh strategy documented;
- logout behavior documented.

### Authorization
- server-side role checks;
- ownership checks;
- admin-only operations enforced server-side.

### Input Validation
- all requests validated;
- unexpected fields handled consistently;
- string lengths constrained;
- numeric bounds validated.

### Database Security
- parameterized queries or safe ORM usage;
- least-privilege database user where practical;
- migrations separated from runtime permissions if the project explores production deployment.

### Logging
Do not log:
- plaintext passwords;
- access tokens;
- refresh tokens;
- password hashes.

---

## 19. Observability Requirements

### Structured Logging

Every request log should include:

- timestamp;
- level;
- request_id;
- method;
- path;
- status_code;
- duration_ms.

Business-event logs may include:

- parking_session_id;
- parking_area_id;
- vehicle_id;
- user_id.

### Health Endpoints

Recommended:

- `/health/live`
- `/health/ready`

Readiness may check:
- PostgreSQL;
- optionally Redis, depending on whether Redis is treated as critical.

### Metrics

Stretch goal:

- request count;
- error count;
- request latency;
- active sessions;
- cache hit rate.

---

## 20. Testing Requirements

### Unit Tests

Minimum examples:

- availability calculation;
- permission checks;
- status transitions;
- normalization;
- error mapping.

### Integration Tests

Must use real PostgreSQL.

Required scenarios:

- register user;
- login;
- create vehicle;
- duplicate vehicle rejection;
- start parking;
- checkout;
- reject second active session;
- reject full parking;
- reject parking in closed area;
- capacity update validation;
- manual close;
- ownership enforcement.

### Concurrency Tests

Required:

- race for last parking slot;
- duplicate concurrent check-in for same vehicle;
- concurrent checkout.

### API Contract Tests

Validate:
- response shape;
- status code;
- error shape;
- authorization.

### Flutter Tests

At least:
- authentication screen;
- vehicle form;
- parking list;
- active session screen.

---

## 21. Deployment Requirements

### Local Environment

Docker Compose should start:

- API;
- PostgreSQL;
- Redis.

Flutter runs separately.

### Environments

Recommended:

- local;
- test;
- staging.

Production is optional.

### Configuration

Use environment variables for:

- database URL;
- Redis URL;
- JWT secrets;
- token TTL;
- logging level;
- server port.

Secrets must not be committed.

---

## 22. Software Development Process

Expected project flow:

1. PRD review
2. Domain modeling
3. RFC preparation
4. ERD review
5. API contract review
6. task breakdown
7. implementation
8. pull request
9. review
10. test
11. deploy
12. retrospective

The intern should not begin implementation until the mentor approves the core design.

---

## 23. Pull Request Requirements

Every PR should include:

- summary;
- motivation;
- implementation notes;
- API changes;
- database changes;
- migration notes;
- testing evidence;
- screenshots for UI changes;
- known limitations.

Small PRs are preferred.

---

## 24. Definition of Done

A feature is complete only when:

- acceptance criteria are met;
- tests are added;
- validation exists;
- error behavior is implemented;
- API documentation is updated;
- migrations are included;
- code has been reviewed;
- observability is considered;
- the intern can explain the implementation.

---

## 25. Milestones

### Milestone 1 — Discovery and Design

Deliverables:
- requirements questions;
- domain glossary;
- ERD;
- API draft;
- architecture diagram;
- RFCs.

### Milestone 2 — Backend Foundation

Deliverables:
- Node.js service;
- TypeScript setup;
- migrations;
- health endpoints;
- logging;
- auth;
- users;
- vehicles.

### Milestone 3 — Parking Core

Deliverables:
- parking areas;
- parking sessions;
- transaction-safe admission;
- checkout;
- history.

### Milestone 4 — Flutter MVP

Deliverables:
- auth flow;
- vehicle flow;
- parking list;
- parking detail;
- check-in;
- checkout;
- history.

### Milestone 5 — Redis

Deliverables:
- availability cache;
- invalidation;
- outage fallback;
- measurements.

### Milestone 6 — Administration

Deliverables:
- dashboard;
- parking-area management;
- session search;
- manual close;
- audit log.

### Milestone 7 — Quality and Delivery

Deliverables:
- CI;
- integration tests;
- concurrency tests;
- deployment;
- final documentation;
- retrospective.

---

## 26. Example Acceptance Scenarios

### Scenario A — Successful Parking

Given:
- parking area capacity = 10;
- occupancy = 9;
- vehicle has no active session.

When:
- user starts parking.

Then:
- session becomes ACTIVE;
- occupancy becomes 10;
- available becomes 0;
- API returns success.

### Scenario B — Parking Full

Given:
- capacity = 10;
- active sessions = 10.

When:
- user attempts to park.

Then:
- no session is created;
- API returns 409;
- error code = PARKING_FULL.

### Scenario C — Concurrent Last Slot

Given:
- capacity = 1;
- no active sessions.

When:
- two check-in requests arrive concurrently.

Then:
- one succeeds;
- one fails;
- active sessions = 1.

### Scenario D — Duplicate Active Session

Given:
- vehicle already has an ACTIVE session.

When:
- user attempts another check-in.

Then:
- request is rejected with 409;
- no second session is created.

### Scenario E — Redis Outage

Given:
- Redis unavailable;
- PostgreSQL healthy.

When:
- user lists parking areas and starts parking.

Then:
- parking list may be slower;
- core operation still succeeds if PostgreSQL permits it;
- no inconsistent state is created.

---

## 27. Open Questions for Intern Discussion

1. Should duplicate license plates be unique globally or per owner?
2. What plate normalization is required?
3. Should one user be allowed multiple simultaneously parked vehicles?
4. Should checkout be idempotent?
5. Should active-session uniqueness be enforced using a partial unique index?
6. Should occupancy be computed or stored?
7. Should capacity cache store `available`, `occupied`, or both?
8. What happens when Redis is unavailable?
9. How should refresh tokens be stored and revoked?
10. What metrics should the dashboard calculate from live vs historical data?
11. How should a stale active session be handled?
12. Should staff be allowed to transfer a session to another parking area?
13. Should parking area deletion be supported or replaced with status changes?
14. Should a disabled user still be able to checkout?
15. What should happen if the parking-area capacity is changed during active usage?

---

## 28. Final Project Evaluation

The final review should evaluate:

### Product Understanding
Can the intern explain why the feature exists?

### Domain Modeling
Can the intern explain relationships and invariants?

### Backend Quality
Is business logic separated from transport and persistence?

### Database Quality
Are constraints and transactions used correctly?

### Redis Understanding
Does the intern understand cache correctness and failure modes?

### Flutter Quality
Does the app clearly handle loading, empty, error, and success states?

### Testing
Are important edge cases automated?

### Delivery
Are pull requests understandable and reviewable?

### Communication
Can the intern defend technical choices and identify tradeoffs?

The goal is not merely that the application runs. The goal is that the intern can reason about how and why it works.

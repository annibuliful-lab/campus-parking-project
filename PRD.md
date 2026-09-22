# Campus Parking Management System

## Product Requirements Document

**Document owner:** Product Owner  
**Audience:** Intern / Junior Software Engineer  
**Version:** 1.0  
**Status:** Ready for discovery and technical design

---

# 1. Product Summary

The Campus Parking Management System helps campus users understand parking availability and manage their parking activity.

Users should be able to:

- register and manage their vehicles;
- see parking areas;
- understand whether parking is available;
- start a parking session;
- end a parking session;
- see their parking history.

Parking staff should be able to:

- inspect current parking activity;
- search for vehicles and parking sessions;
- correct specific operational issues when necessary.

Administrators should be able to:

- configure parking areas;
- configure capacity and operational status;
- view current operational information;
- review privileged staff/admin actions.

The project is also intended to train an intern/junior engineer in the software development lifecycle, including requirement clarification, technical design, tradeoff analysis, implementation, testing, review, deployment, and retrospective.

---

# 2. Problem Statement

Campus users may not know:

- which parking areas exist;
- whether a parking area is currently usable;
- whether space is still available;
- whether their vehicle is already considered parked;
- how long they have been parked;
- their previous parking activity.

Parking staff may not have a single operational view of:

- active parking sessions;
- vehicles currently parked;
- capacity by parking area;
- operational corrections;
- configuration changes.

The product should provide a consistent workflow for these problems without attempting to model every real-world parking feature.

---

# 3. Product Goals

The product should:

1. Give campus users a simple way to know where they can park.
2. Keep an accurate record of active parking sessions.
3. Prevent invalid parking states from being created.
4. Give staff enough visibility to investigate parking issues.
5. Give administrators control over parking-area configuration.
6. Preserve useful historical information.
7. Make privileged operational changes traceable.
8. Remain understandable and maintainable as a learning project.

---

# 4. Product Success Criteria

For the first release, the product is successful when:

- a user can create an account and sign in;
- a user can register a vehicle;
- a user can see parking areas and useful availability information;
- a user can successfully start and end a parking session;
- the system prevents parking beyond configured capacity;
- the system prevents the same vehicle from being actively parked more than once;
- a user can review past parking sessions;
- staff can search parking activity;
- authorized staff can resolve an invalid/stale active parking session;
- administrators can manage parking areas;
- important staff/admin actions can be traced to the actor who performed them.

Technical performance targets, architecture targets, and infrastructure targets are intentionally not defined here. The intern must propose them during technical design and justify them.

---

# 5. Target Users

## 5.1 Campus User

Typical users:

- student;
- lecturer;
- campus employee.

Primary needs:

- know parking availability;
- register a vehicle;
- start parking;
- end parking;
- review parking history.

## 5.2 Parking Staff

Primary needs:

- understand current parking activity;
- look up a vehicle or parking session;
- inspect active parking sessions;
- correct a session when a user cannot do so normally.

## 5.3 Administrator

Primary needs:

- manage parking areas;
- manage capacity;
- open/close parking areas;
- view current campus parking usage;
- review important administrative actions.

---

# 6. MVP Scope

## 6.1 In Scope

- account registration;
- login;
- basic roles;
- vehicle registration;
- vehicle list;
- vehicle update;
- vehicle removal/deactivation;
- parking-area listing;
- parking-area details;
- parking availability;
- parking check-in;
- parking checkout;
- active parking session;
- parking history;
- staff parking-session search;
- staff manual session correction;
- administrator parking-area management;
- administrative audit trail.

## 6.2 Out of Scope

The following are not required for MVP:

- payment;
- parking fees;
- automatic license plate recognition;
- physical gate/barrier integration;
- real IoT sensors;
- navigation to a specific parking space;
- machine-learning parking forecasts;
- dynamic pricing;
- multi-campus federation;
- external vehicle registries;
- permit enforcement;
- automatic fine calculation.

These may become future product requirements.

---

# 7. Product Assumptions

For MVP:

- parking capacity is defined at parking-area level;
- users are not assigned an exact physical parking space;
- users manually indicate when they enter and leave parking;
- staff may correct a parking session when operationally necessary;
- historical parking records should remain available;
- exact UI design is not specified by the Product Owner;
- exact technical architecture is not specified by the Product Owner.

If an assumption creates a product, security, usability, or technical problem, the intern should raise it during discovery rather than silently designing around it.

---

# 8. Core User Journeys

## 8.1 New User Journey

1. User creates an account.
2. User signs in.
3. User registers a vehicle.
4. User sees parking areas.
5. User inspects one parking area.
6. User sees whether parking is available.
7. User starts parking.
8. User sees an active parking session.
9. User ends the parking session.
10. User sees the completed session in history.

## 8.2 Returning User Journey

1. User signs in.
2. User sees whether an active parking session exists.
3. User may end that session or start another session when business rules allow.
4. User can review parking history.

## 8.3 Parking Staff Journey

1. Staff signs in.
2. Staff searches parking activity using supported criteria.
3. Staff opens a parking session.
4. Staff reviews current state.
5. Staff may manually close an invalid/stale active session.
6. The correction can later be traced.

## 8.4 Administrator Journey

1. Administrator signs in.
2. Administrator creates or updates parking-area information.
3. Administrator changes capacity or operational status.
4. New parking behavior follows the new configuration.
5. The configuration change can later be traced.

---

# 9. User Stories

## Epic A — Identity

### US-A01 Register

As a campus user, I want to create an account so that I can use campus parking services.

### US-A02 Login

As a registered user, I want to sign in so that I can access my vehicles and parking activity.

### US-A03 Role Protection

As the business owner, I want restricted operations to be available only to authorized roles so that normal users cannot perform staff or administrator actions.

## Epic B — Vehicle Management

### US-B01 Register Vehicle

As a user, I want to register a vehicle so that I can use it for parking.

### US-B02 View Vehicles

As a user, I want to see my registered vehicles so that I can choose one for parking.

### US-B03 Update Vehicle

As a user, I want to update vehicle details so that my information remains accurate.

### US-B04 Remove Vehicle

As a user, I want to stop using a vehicle in the system so that old vehicles do not remain active choices.

## Epic C — Parking Discovery

### US-C01 View Parking Areas

As a user, I want to see campus parking areas so that I know my parking options.

### US-C02 View Availability

As a user, I want to see whether parking is available so that I can decide where to park.

### US-C03 View Area Status

As a user, I want to know when a parking area is closed or unavailable so that I do not attempt to park there.

## Epic D — Parking Session

### US-D01 Start Parking

As a user, I want to start a parking session for one of my vehicles so that the system records that I am parked.

### US-D02 Prevent Invalid Parking

As the Product Owner, I want invalid parking attempts rejected so that occupancy and user records remain correct.

### US-D03 View Active Session

As a user, I want to see my current parking session so that I know where and when I parked.

### US-D04 End Parking

As a user, I want to end my parking session so that the parking area becomes available to other users.

### US-D05 View History

As a user, I want to see my parking history so that I can review previous parking activity.

## Epic E — Staff Operations

### US-E01 Search Parking Activity

As parking staff, I want to search parking sessions so that I can investigate parking issues.

### US-E02 Manual Close

As parking staff, I want to manually close an active session with a reason so that incorrect operational state can be fixed.

## Epic F — Parking Administration

### US-F01 Create Parking Area

As an administrator, I want to add a parking area so that it can be used by campus users.

### US-F02 Update Parking Area

As an administrator, I want to update parking information so that operational configuration remains accurate.

### US-F03 Change Capacity

As an administrator, I want to change parking capacity so that the system reflects operational reality.

### US-F04 Change Parking Status

As an administrator, I want to open, close, or place a parking area into maintenance so that users cannot start new parking sessions when the area should not accept vehicles.

## Epic G — Traceability

### US-G01 Audit Privileged Changes

As the Product Owner, I want important staff/admin actions to be traceable so that operational changes can be investigated.

---

# 10. Product-Level Data Expectations

The product must represent enough information to support:

- users;
- roles;
- vehicles;
- parking areas;
- capacity;
- parking-area operational status;
- active parking sessions;
- completed parking sessions;
- staff corrections;
- audit history.

The Product Owner is intentionally **not defining the database schema**.

The intern must decide and justify:

- entities/tables;
- relationships;
- keys;
- constraints;
- indexes;
- identifiers;
- deletion strategy;
- history strategy.

---

# 11. Product-Level Integration Expectations

The Flutter application must communicate with the backend through a documented contract that supports all accepted user journeys.

The Product Owner is intentionally **not defining**:

- REST endpoint paths;
- resource shape;
- request/response DTOs;
- pagination strategy;
- error schema;
- API versioning;
- retry behavior;
- idempotency strategy.

The intern must propose these during design review.

---

# 12. Product-Level Availability Expectations

The user should see useful parking availability information.

The Product Owner expects:

- availability not to knowingly exceed configured capacity;
- invalid parking attempts to be rejected;
- availability to reflect successful parking changes;
- temporary infrastructure problems not to permanently corrupt parking state.

The Product Owner is intentionally **not prescribing**:

- whether occupancy is stored or computed;
- how Redis is used;
- how freshness is maintained;
- locking strategy;
- database isolation strategy;
- cache invalidation strategy.

---

# 13. Product-Level Security Expectations

The product must:

- protect user accounts;
- restrict privileged features;
- prevent users from modifying another user's resources;
- avoid exposing credentials;
- avoid obvious injection-style vulnerabilities;
- preserve accountability for privileged actions.

The implementation approach is the intern's responsibility.

---

# 14. Product-Level UX Expectations

The mobile app should clearly communicate:

- loading;
- success;
- empty state;
- recoverable errors;
- permission failures;
- expired/invalid authentication state when applicable.

The Product Owner is intentionally not specifying:

- widget structure;
- navigation library;
- state-management package;
- clean architecture vs another app architecture.

---

# 15. Product-Level Quality Expectations

Before the Product Owner accepts a feature:

- the happy path must work;
- acceptance criteria must pass;
- invalid actions must be handled;
- business rules must remain true;
- important failure scenarios must be tested;
- implementation must be reviewable;
- the engineer must be able to explain the design.

Exact testing architecture is intentionally not prescribed.

---

# 16. Delivery Expectations

The intern/junior should work in small increments.

For each major feature:

1. clarify requirements;
2. propose technical design;
3. identify edge cases;
4. implement;
5. test;
6. raise a pull request;
7. respond to review;
8. demonstrate acceptance criteria.

---

# 17. Technical Deliverables Expected from the Intern

Before full implementation, the intern should produce:

- system context / high-level architecture;
- component architecture;
- database design / ERD;
- API design;
- authentication and authorization design;
- parking consistency/concurrency design;
- Redis purpose and failure behavior;
- Flutter application architecture;
- testing strategy;
- local-development/deployment plan;
- observability plan;
- identified risks and tradeoffs.

These are engineering outputs, not Product Owner requirements.

---

# 18. Milestones

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

# 19. Open Product Questions

The intern should raise questions when product behavior is ambiguous.

Examples:

- Can one user park two different vehicles at the same time?
- Is a license plate globally unique?
- What should happen when a user forgets to checkout?
- Can staff correct a completed session?
- What should users see when displayed availability may be stale?
- Can an administrator reduce capacity below current occupancy?
- Should disabled accounts still be allowed to checkout?
- Should parking areas ever be deleted?
- What exact information is required when staff manually close a session?

The engineer should not silently invent product behavior when the decision materially changes user-facing behavior.

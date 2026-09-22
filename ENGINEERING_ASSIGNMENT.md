# Engineering Assignment for Intern / Junior

## 1. Context

You are the engineer responsible for designing and implementing the Campus Parking Management System.

The Product Owner has defined:
- business goals;
- scope;
- user stories;
- business rules;
- acceptance criteria.

The Product Owner has intentionally **not defined the technical solution**.

Your job is to transform the business requirements into a technical design, defend your decisions, and implement the approved design.

---

# 2. Technology Constraints

You must use:
- PostgreSQL;
- Redis;
- Node.js;
- Flutter.

You may choose and justify:
- Node.js framework;
- TypeScript usage and project layout;
- PostgreSQL access library / ORM / query builder / raw SQL;
- Redis client and Redis role;
- authentication implementation;
- API shape;
- Flutter packages;
- state-management approach;
- local development tooling;
- testing libraries;
- deployment approach.

Every major choice should be explainable in terms of requirements and tradeoffs.

---

# 3. Required Design Deliverables

Before full implementation, prepare the following.

## 3.1 System Architecture

Describe:
- system boundaries;
- major components;
- communication paths;
- external dependencies;
- data ownership;
- failure boundaries.

Include at least one architecture diagram.

## 3.2 Backend Architecture

Explain:
- code/module organization;
- request flow;
- business/application logic placement;
- persistence boundaries;
- dependency direction;
- validation;
- error handling;
- configuration.

Do not choose an architecture pattern only because it is popular.

## 3.3 Database Design

Provide:
- ERD;
- proposed tables/entities;
- relationships;
- keys;
- constraints;
- indexes;
- deletion/history strategy;
- migration approach.

Explain how the design protects business rules rather than relying only on application convention.

## 3.4 Parking Consistency / Concurrency Design

Explain how the system guarantees:
- capacity is not exceeded;
- one vehicle cannot have multiple active sessions;
- concurrent requests remain correct;
- repeated requests do not corrupt state.

Include:
- race-condition examples;
- chosen mechanism;
- alternatives considered;
- failure behavior;
- testing approach.

## 3.5 Redis Design

Explain:
- why Redis exists in your architecture;
- what information is stored there;
- what is deliberately not stored there;
- whether you use caching, messaging, rate limiting, coordination, or another capability;
- TTL where relevant;
- invalidation where relevant;
- failure behavior;
- recovery behavior;
- authoritative data ownership.

A proposal that only says “use Redis for cache” is insufficient.

## 3.6 API Design

Provide:
- resource/action model;
- endpoints or equivalent contract;
- authentication contract;
- authorization behavior;
- request/response examples;
- error model;
- pagination where applicable;
- versioning approach;
- retry/idempotency behavior.

Prefer an OpenAPI document if choosing REST.

## 3.7 Authentication and Authorization Design

Explain:
- registration;
- login;
- credential protection;
- authentication state/token/session model;
- logout;
- role checks;
- ownership checks;
- disabled-user behavior if supported;
- sensitive-data handling.

## 3.8 Flutter Architecture

Explain:
- feature organization;
- navigation;
- state management;
- API client;
- authentication lifecycle;
- loading/error/empty state;
- secure local storage where applicable;
- testing approach.

## 3.9 Testing Strategy

Define:
- unit tests;
- integration tests;
- database tests;
- concurrency tests;
- API tests;
- Flutter tests;
- end-to-end tests if used.

Map important acceptance criteria to automated or repeatable tests.

## 3.10 Observability

Propose:
- logs;
- request correlation;
- health checks;
- metrics;
- debugging strategy;
- what information must not be logged.

## 3.11 Local Development

Provide a reproducible developer setup.

Explain:
- required services;
- startup procedure;
- migrations;
- seed data;
- test execution;
- environment variables;
- secrets handling.

## 3.12 Deployment Proposal

Provide a reasonable deployment plan.

It may be intentionally simple, but explain:
- API runtime;
- PostgreSQL;
- Redis;
- configuration/secrets;
- migrations;
- Flutter distribution;
- rollback considerations.

---

# 4. Required RFC Topics

Write RFCs for at least:

1. System Architecture
2. Database and Domain Model
3. Parking Session Concurrency / Consistency
4. Redis Usage
5. Authentication and Authorization
6. API Contract
7. Flutter Architecture
8. Testing and CI

You may combine or add RFCs when justified.

---

# 5. RFC Template

Each RFC should include:

## Context
What problem are we solving?

## Product Requirements
Which business rules and acceptance criteria constrain this decision?

## Options Considered
Describe credible alternatives. For important decisions, include more than one option.

## Proposed Decision
What are you proposing?

## Rationale
Why does it fit this project?

## Tradeoffs
What improves? What becomes harder?

## Failure Modes
What can fail and what happens?

## Security Considerations
What risks exist and how are they handled?

## Testing
How will correctness be demonstrated?

## Rollout / Migration
How will the design be introduced or changed safely?

## Open Questions
What remains unresolved?

---

# 6. Product Questions You Are Expected to Raise

You are expected to identify ambiguity rather than silently invent behavior.

Examples:
- Can one user park multiple different vehicles simultaneously?
- Is a license plate globally unique or only unique inside one campus/account?
- What should repeated checkout return?
- Can a disabled user still checkout?
- What happens if a user forgets to checkout for several days?
- May staff edit historical times or only close an active session?
- What does “delete vehicle” mean to the user?
- How fresh must displayed availability be?
- What behavior is acceptable during Redis failure?
- Can capacity be reduced below occupancy with an override?
- Which actions require audit history?
- How much user information should staff be allowed to see?

---

# 7. Technical Decisions You Are Expected to Make

You should make and defend decisions such as:
- monolith vs another architecture;
- framework selection;
- ORM/query builder/raw SQL;
- identifier strategy;
- database constraints;
- indexes;
- transaction boundaries;
- concurrency mechanism;
- Redis role;
- API conventions;
- authentication/session strategy;
- Flutter state management;
- test infrastructure;
- CI pipeline;
- observability;
- deployment topology.

The mentor may challenge the choice. The goal is not to guess the mentor's preferred answer; it is to reason from requirements and tradeoffs.

---

# 8. Recommended Engineering Sequence

1. Understand requirements.
2. Identify product ambiguities.
3. Draft architecture and RFCs.
4. Review with Product Owner / mentor.
5. Resolve product decisions.
6. Build core persistent behavior.
7. Demonstrate business correctness.
8. Build Flutter flows.
9. Introduce Redis for a justified purpose.
10. Test failure and concurrency scenarios.
11. Improve observability and CI.
12. Deploy.
13. Retrospective.

---

# 9. Required Demonstrations

At project review, demonstrate:
- successful check-in;
- successful checkout;
- duplicate active-parking prevention;
- full-parking rejection;
- concurrent last-slot behavior;
- closed-area rejection;
- unauthorized privileged-action rejection;
- staff manual correction;
- audit visibility;
- defined behavior when Redis is unavailable.

---

# 10. Final Technical Presentation

Prepare a short presentation containing:
- product problem;
- architecture;
- domain/data model;
- important technical decisions;
- concurrency design;
- Redis decision;
- security model;
- testing strategy;
- failure handling;
- tradeoffs;
- what you would change for production or higher scale.

You should be able to answer “why?” for every important technical choice.

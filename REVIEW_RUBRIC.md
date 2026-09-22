# Intern / Junior Review Rubric

This rubric is for mentor and Product Owner review. The goal is to evaluate engineering reasoning, not only feature completion.

## 1. Requirement Understanding

Strong indicators:
- identifies ambiguity;
- asks product questions;
- distinguishes business decisions from technical decisions;
- maps design back to business rules and acceptance criteria;
- does not start implementation before understanding critical behavior.

Warning signs:
- silently invents requirements;
- copies architecture without explaining it;
- treats acceptance criteria as optional.

## 2. System Architecture

Review questions:
- Are boundaries clear?
- Is the architecture proportionate to the problem?
- Are responsibilities understandable?
- Are failure boundaries considered?
- Can the engineer explain alternatives?
- Is unnecessary complexity avoided?

Do not grade based on whether the engineer chose the mentor's favorite architecture pattern.

## 3. Database Design

Review:
- entities and relationships;
- business invariants;
- constraints;
- indexes;
- history preservation;
- migration strategy;
- concurrency implications.

Key question:

> Which business rules remain true even if two requests happen concurrently?

## 4. Backend Design

Review:
- separation of concerns;
- business logic location;
- persistence boundaries;
- predictable errors;
- transaction boundaries;
- authorization enforcement;
- testability.

## 5. Redis Decision

Review:
- Does Redis solve a real problem?
- Is its role clear?
- Which data is authoritative?
- What happens during Redis failure?
- Can Redis data be rebuilt?
- Is invalidation/freshness addressed where relevant?

Do not reward Redis usage simply for using Redis.

## 6. API Design

Review:
- resource/action clarity;
- consistency;
- error semantics;
- retries/idempotency;
- pagination;
- authorization behavior;
- documentation quality.

## 7. Flutter Design

Review:
- explicit UI states;
- networking separation;
- authentication lifecycle;
- safe local state;
- accurate feedback after server operations;
- testability;
- state-management rationale.

## 8. Concurrency and Correctness

This is a major learning objective.

The engineer should explain:
- the race condition;
- why a naive check-then-write can fail;
- how the chosen mechanism prevents invalid state;
- how retry affects behavior;
- how the solution is tested.

## 9. Testing

Review:
- important business rules are automated;
- integration tests use realistic dependencies where valuable;
- concurrency is tested;
- failure scenarios are tested;
- the engineer understands what should and should not be mocked.

## 10. Security

Review:
- credential storage;
- token/session model;
- authorization;
- ownership checks;
- input validation;
- injection prevention;
- secrets management;
- log redaction.

## 11. Observability

Review:
- can requests be traced?
- can important failures be debugged?
- are logs useful rather than noisy?
- are health checks meaningful?
- are operational signals appropriate to the project?

## 12. Pull Request Quality

Review:
- PR size;
- explanation quality;
- testing evidence;
- migrations;
- documentation;
- response to review;
- commit clarity.

## 13. Communication

Engineer should be able to explain:
- what they decided;
- why;
- alternatives;
- risks;
- known limitations;
- what would change at higher scale or in production.

## 14. Final Review Questions

1. What is the source of truth and why?
2. What happens if Redis disappears?
3. How do you prevent two vehicles from taking the final slot?
4. What prevents one vehicle from having two active sessions?
5. Which business rules are enforced by durable constraints versus application code?
6. What happens if the client retries a request?
7. What happens if the durable change succeeds but a secondary operation fails?
8. Why did you choose this API model?
9. Why did you choose this Flutter state-management approach?
10. What bottleneck or correctness risk would appear first at 100x traffic?
11. What would you redesign for a real production campus?
12. Which product requirement was most ambiguous and how was it resolved?

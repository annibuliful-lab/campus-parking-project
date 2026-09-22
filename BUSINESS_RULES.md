# Campus Parking — Business Rules

This document defines business behavior only. It intentionally does not define database constraints, transaction strategy, API design, Redis design, locking, or framework choices.

## BR-001 Vehicle Ownership
A vehicle belongs to one user account for MVP. A normal user may view, update, and park only their own vehicles.

## BR-002 Active Parking Session
A parking session is active from successful check-in until successful checkout or authorized staff correction. A vehicle may not have more than one active parking session at the same time.

## BR-003 Parking Capacity
Every parking area has a configured capacity greater than zero. Active parking sessions for an area must not exceed that capacity. If no capacity remains, new parking attempts must be rejected.

## BR-004 Parking Area Operational Status
A parking area must support at least open, closed, and maintenance/unavailable states. Only an open area accepts new parking sessions. Existing vehicles must still be able to leave or otherwise resolve their sessions.

## BR-005 Check-In Validity
A parking attempt may succeed only when the user is authenticated, the vehicle belongs to the user, the vehicle is active/usable, the vehicle has no active parking session, the parking area exists, the parking area is accepting new parking, and capacity remains. A failed attempt must not leave a new active session behind.

## BR-006 Checkout Validity
A normal user may checkout only a currently active parking session associated with their account. Successful checkout makes the session historical and makes that capacity available again.

## BR-007 Duplicate or Concurrent Requests
Repeated or concurrent actions must not create impossible business state. One vehicle must not become active in two sessions, two requests must not both consume the same final slot, and repeated checkout must not release capacity twice or create duplicate history. Engineering must propose retry semantics.

## BR-008 Vehicle Removal
A vehicle with an active parking session cannot be removed from active use until that parking session is resolved. Historical sessions must remain understandable after a vehicle is no longer active.

## BR-009 Parking History
Completed parking sessions are historical business records. They must remain queryable by the relevant user and authorized staff and preserve enough information to identify the vehicle, parking area, start time, end time, and how the session ended when corrected manually.

## BR-010 Staff Manual Close
Authorized staff may close an active session when operational correction is necessary. A reason is required. The system must preserve who performed the correction, which session changed, when it happened, and the reason.

## BR-011 Capacity Changes
An administrator may change parking capacity. The system must not silently create an invalid current state. If current occupancy is greater than a requested new capacity, the exact product behavior must be agreed before implementation.

## BR-012 Parking Area Removal
Historical records must remain explainable even if a parking area is no longer used. The engineer must propose archive, deactivate, soft-delete, or another suitable approach and explain tradeoffs.

## BR-013 Privileged Actions
Creating or changing parking areas, changing capacity or operational status, and correcting another user's session require privileged authorization. Engineering must define and document the authorization model.

## BR-014 Auditability
Important staff/admin actions must be traceable to an authenticated actor. Audit information must answer who did it, what happened, what resource was affected, when it happened, and why when a reason is required.

## BR-015 Failure Safety
A failed operation must not leave partially valid business state. Failed check-in must not consume capacity without an active session. Failed checkout must not release capacity while leaving the session active. Failed configuration changes must not partially apply.

## BR-016 Redis Requirement
Redis is part of the required technology stack, but the Product Owner does not define its technical role. Engineering must explain why Redis is needed, what data goes there, what happens if Redis is unavailable, what data is authoritative, and what can be reconstructed. Redis should not be used merely because it appears in the stack.

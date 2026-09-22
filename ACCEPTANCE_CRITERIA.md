# Campus Parking — Acceptance Criteria

These scenarios define observable product behavior and intentionally avoid implementation details.

## Account Registration

### AC-A01 Successful Registration
Given a new valid email and valid required input, when the user registers, then an account is created, the user can later authenticate, and sensitive credential data is not returned.

### AC-A02 Duplicate Email
Given an existing account for the submitted email, when another registration is attempted, then registration is rejected and no duplicate active account is created.

### AC-A03 Invalid Registration
Given invalid or incomplete required information, when registration is submitted, then it is rejected, useful validation feedback is returned, and no partial account is created.

## Login

### AC-B01 Valid Login
Given valid credentials for an allowed account, when login is submitted, then authentication succeeds and the client can access protected features.

### AC-B02 Invalid Login
Given invalid credentials, when login is submitted, then login fails and no authenticated state is created.

## Vehicle Management

### AC-C01 Register Vehicle
Given an authenticated user and valid vehicle information, when the user registers a vehicle, then it appears in that user's vehicle list and another user cannot modify it.

### AC-C02 Invalid Vehicle
Given invalid required vehicle information, when registration is submitted, then registration fails and no partial vehicle is created.

### AC-C03 Remove Unused Vehicle
Given a vehicle owned by the user with no active parking session, when it is removed/deactivated, then it is no longer available for new parking and historical sessions remain understandable.

### AC-C04 Remove Vehicle with Active Parking
Given a vehicle with an active parking session, when the user attempts removal, then removal is rejected and active parking remains unchanged.

## Parking Area Discovery

### AC-D01 View Areas
Given a user can access the application, when parking areas are loaded, then the user sees available parking options and each area communicates at least its identity, operational status, and useful capacity/availability information.

### AC-D02 Closed Area
Given a parking area is closed/unavailable, when the user views it, then that state is visible and new parking cannot successfully start there.

## Successful Parking

### AC-E01 Start Parking
Given an authenticated user, a vehicle they own, no active session for that vehicle, an open parking area, and remaining capacity, when the user starts parking, then exactly one active parking session is created, current parking shows it, and availability reflects the new active vehicle.

## Parking Full

### AC-F01 Reject Full Area
Given a parking area has no remaining capacity, when the user attempts to start parking, then the request is rejected, no active parking session is created, and existing valid state remains unchanged.

## Duplicate Active Parking

### AC-G01 Prevent Second Session
Given a vehicle already has an active parking session, when another check-in is attempted for that vehicle, then the attempt is rejected and exactly one active session remains.

## Concurrent Last Slot

### AC-H01 Only One Request Wins
Given a parking area has exactly one remaining slot, when two valid users attempt to consume that final slot at effectively the same time, then only one new parking session succeeds, the other is rejected, and active parking does not exceed capacity.

The engineer must demonstrate this behavior using an automated or repeatable test.

## Active Parking View

### AC-I01 See Current Session
Given a user has an active parking session, when current parking is viewed, then the app clearly identifies the active session, vehicle, parking area, and start time.

### AC-I02 No Active Session
Given a user has no active parking session, when current parking is viewed, then the application clearly communicates that no active parking exists.

## Checkout

### AC-J01 Successful Checkout
Given a user owns an active parking session, when they checkout, then the session is no longer active, an end time is recorded, the session appears in history, and availability reflects one fewer active vehicle.

### AC-J02 Checkout Another User's Session
Given a normal user attempts to checkout a session they do not own, when checkout is submitted, then the operation is rejected and the target session remains unchanged.

### AC-J03 Repeated Checkout
Given a session has already ended, when checkout is repeated, then the result is deterministic, no duplicate history is created, and capacity is not released twice. The engineer must propose the exact client-facing behavior for Product Owner approval.

## Parking History

### AC-K01 View History
Given a user has completed parking sessions, when history is opened, then the user can identify parking area, vehicle, start time, and end time for those sessions.

## Staff Search

### AC-L01 Find Parking Activity
Given authorized staff, when staff searches using supported criteria, then matching parking activity is returned and active/completed sessions can be distinguished. Engineering should propose practical MVP search criteria.

## Staff Manual Close

### AC-M01 Correct Active Session
Given an active session requiring operational correction and authorized staff, when staff manually closes it with a reason, then the session is no longer active, the correction is identifiable as a staff action, actor/reason/time can be reviewed, and availability reflects the correction.

### AC-M02 Missing Reason
Given staff attempts manual close without a required reason, when the action is submitted, then the correction is rejected.

## Parking Administration

### AC-N01 Create Parking Area
Given an authorized administrator and valid configuration, when an area is created, then it becomes discoverable according to its configured operational state.

### AC-N02 Valid Capacity Increase
Given an authorized administrator, when capacity is increased, then future parking decisions use the new capacity and availability reflects it.

### AC-N03 Capacity Reduction Below Occupancy
Given current active parking exceeds the requested new capacity, when an administrator attempts the reduction, then the system does not silently create an invalid state. The exact outcome must be agreed during design review.

### AC-N04 Close Area
Given an authorized administrator, when an area is changed to closed/unavailable, then new parking attempts are rejected while existing active sessions remain visible and resolvable.

## Authorization

### AC-O01 Normal User Attempts Admin Action
Given an authenticated normal user, when they attempt an admin-only action, then the action is rejected and no privileged change is applied.

## Auditability

### AC-P01 Audit Privileged Action
Given staff/admin performs an auditable operation, when it succeeds, then an authorized reviewer can determine who performed it, what changed, when it happened, the affected resource, and the reason when applicable.

## Infrastructure Failure

### AC-Q01 Redis Unavailable
Given Redis is temporarily unavailable, when a core parking operation occurs, then the system must not create corrupt parking state. Engineering must propose whether the operation remains available or intentionally fails and explain why.

### AC-Q02 Durable Storage Unavailable
Given durable parking data cannot be safely accessed, when a state-changing parking operation is attempted, then the system must not report a successful durable change unless that change can actually be guaranteed.

## Mobile Error Handling

### AC-R01 Recoverable Network Error
Given a mobile request fails due to a recoverable connectivity issue, when the app handles the result, then the user receives a clear error state, the app does not falsely show an unconfirmed parking action as successful, and retry is possible where appropriate.

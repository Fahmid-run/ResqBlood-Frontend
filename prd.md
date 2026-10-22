# Blood Donation & Emergency Assistance Platform — Full PRD

Product Requirements Document · PERN Stack · Version 1.0 · October 2026

The biggest scope risk is trying to build donor matching, hospital management, emergency transport, real-time messaging, analytics, and fraud detection all at once. This PRD defines the complete product vision while separating the MVP, advanced features, and future enhancements so you can build from scratch without drowning in complexity.

## Product overview

Working name: BloodBridge

BloodBridge is a blood donation coordination platform connecting patients, verified hospitals, eligible donors, volunteers, and administrators to help coordinate urgent blood requests and donation logistics.

Architecture

PERN + TypeScript

Primary database

PostgreSQL

Primary users

Donors, patients, hospitals

Core challenge

Reliable matching and coordination

## 1. Product objectives

### Primary objectives

1. Help patients and hospitals publish legitimate blood requests.
2. Help identify potentially suitable donors by blood group, location, availability, and eligibility rules.
3. Allow donors to respond and coordinate donations without assigning the same remaining slot multiple times.
4. Track a request from submission through verification and fulfillment.
5. Reduce fake requests, abuse, privacy violations, and unreliable donor responses.
6. Give administrators visibility into unmet demand, response times, and platform activity.

### Non-goals for the initial release

- Diagnosing patients or making clinical decisions.
- Determining final medical eligibility for donation.
- Replacing a blood bank, hospital information system, or blood-testing laboratory.
- Guaranteeing that a matched donor's blood is safe or compatible for transfusion.
- Operating emergency medical dispatch or guaranteeing blood availability.

BloodBridge coordinates people and workflows. Qualified medical professionals and authorized blood banks remain responsible for clinical screening, compatibility confirmation, collection, and transfusion.

## 2. User roles and permissions

| Role                   | Main responsibilities                                                                                |
| ---------------------- | ---------------------------------------------------------------------------------------------------- |
| Guest                  | Browse public information and verified active requests with limited detail                           |
| Donor                  | Maintain donor profile, manage availability, receive invitations, respond, and view donation history |
| Patient / requester    | Create requests, provide required evidence, track progress, and coordinate with authorized parties   |
| Hospital administrator | Register a hospital, manage verification, and oversee hospital operations                            |
| Hospital staff         | Create and manage requests for their assigned hospital, subject to permissions                       |
| Volunteer              | Accept coordination or transport tasks and update progress                                           |
| Platform admin         | Verify, moderate, suspend, resolve disputes, and view platform analytics                             |
| Super admin            | Manage platform-wide configuration, privileged access, and administrative roles                      |

Authorization principle: every protected action must verify both the user's role and their permission to access the specific resource. Being hospital staff must not grant access to every hospital's records.

## 3. Full feature list

The following is the product backlog. Each feature includes its expected behavior and its priority.

Priority definitions:

- P0 — MVP: required for a usable first release.
- P1 — Advanced: important for a production-style portfolio project.
- P2 — Future: valuable enhancement after the core system is stable.

### Module A — Authentication and account management

A1. Registration and login

P0

- Register with name, email or phone, and password.
- Log in and log out securely.
- Hash passwords using Argon2id or bcrypt.
- Validate inputs on both frontend and backend.
- Prevent duplicate accounts for unique identifiers.
- Provide password reset and email verification workflows.
- Use secure cookie-based sessions or access/refresh token authentication.
- Revoke sessions on logout or account compromise.

Acceptance criteria: invalid credentials are rejected, passwords are never stored in plaintext, and protected APIs cannot be accessed without valid authentication.

A2. Role-based access control

P0

- Support donor, patient/requester, hospital staff, volunteer, and administrator roles.
- Restrict routes and API operations by permission.
- Enforce ownership of requests, profiles, and assignments.
- Support account suspension and reactivation.
- Record privileged administrative actions.
- Require controlled promotion to privileged roles.

Acceptance criteria: changing a frontend role or request payload never grants additional permissions.

A3. Profile and account settings

P0

- Edit profile information.
- Manage phone and email verification.
- Configure notification preferences.
- Change passwords and review active sessions.
- Request account deactivation.
- Capture consent for location use and data processing where required.

Acceptance criteria: users can edit only their own ordinary profile data; sensitive changes require reauthentication or appropriate verification.

### Module B — Donor management

B1. Donor registration and profile

P0

- Record donor blood group and Rh type.
- Capture date of birth or an age-eligibility indicator as appropriate.
- Store a general location and optional preferred donation radius.
- Track profile completion and verification status.
- Let donors voluntarily provide additional relevant information through a privacy-conscious process.
- Prevent public exposure of private contact and medical information.

B2. Availability management

P0

- Set available, unavailable, or temporarily unavailable.
- Set a temporary unavailability period.
- Configure normal and emergency availability preferences.
- Define a preferred travel radius.
- Automatically exclude donors who have opted out or are not currently eligible for consideration.
- Restore availability when a temporary restriction expires, provided no other restriction remains.

B3. Donation history and eligibility

P0

- View verified past donation records.
- Track the last confirmed donation date.
- Calculate a provisional eligibility date using configurable rules.
- Distinguish reported donation history from hospital-verified history.
- Prevent donors from manually marking an unverified donation as confirmed.
- Display a clear message when eligibility requires professional assessment.

Important: cooldown intervals and age criteria must be configurable and reviewed against applicable medical guidance. Application rules are preliminary screening aids, not medical clearance.

### Module C — Patient and requester workflows

C1. Request creation

P0

- Specify required blood group, quantity, urgency, and required-by time.
- Specify the hospital or collection location.
- Provide a description of the need and a safe contact method.
- Identify the requester's relationship to the patient when appropriate.
- Attach supporting documentation through a controlled, private upload process.
- Save a draft before submitting.
- Display validation errors and missing information.

C2. Request tracking

P0

- View pending verification, verified, matching, partially fulfilled, fulfilled, rejected, cancelled, and expired requests.
- View the number of donor responses and confirmed assignments.
- See the next action required.
- Edit permitted fields before verification or under controlled amendment rules.
- Cancel a request and provide a reason.
- Receive updates when its status changes.
- View a timeline of important events.

C3. Request privacy and safety

P0

- Limit public visibility of patient details.
- Reveal donor contact information only when an authorized workflow requires it.
- Warn users against payments, scams, and sharing unnecessary medical information.
- Provide reporting and abuse-escalation mechanisms.
- Let authorized requesters correct inaccurate information through an auditable workflow.

### Module D — Hospital management

D1. Hospital registration and verification

P0

- Register hospital identity, address, location, and contact details.
- Submit supporting evidence for verification.
- Support pending, approved, rejected, and suspended verification states.
- Record who reviewed a hospital and when.
- Restrict trusted hospital privileges until verification is approved.

D2. Hospital staff and permissions

P1

- Invite staff members to a hospital.
- Assign roles such as manager, request creator, and request verifier.
- Revoke staff access.
- Scope all operations to the staff member's assigned hospital.
- Track staff actions in audit history.

D3. Hospital dashboard

P0

- View active and critical requests.
- View matching progress and donor responses.
- Manage request updates and cancellations.
- Confirm donor arrival and record authorized donation outcomes.
- Review historical requests and operational metrics.

### Module E — Request verification and moderation

E1. Verification workflow

P0

- Queue submitted requests for review.
- Assign requests to authorized reviewers.
- Approve, reject, or request additional information.
- Record verification decisions, reasons, reviewer identity, and timestamps.
- Support resubmission after correction.
- Automatically trust neither uploaded documents nor user-supplied claims without review.

Acceptance criteria: unverified requests cannot trigger donor notifications; only authorized reviewers can approve requests.

E2. Admin moderation

P0

- Review reported requests, users, hospitals, and volunteers.
- Suspend or reinstate accounts with a recorded reason.
- Flag repeated or suspicious request patterns.
- Manage duplicate requests.
- Review abuse reports and disputed donation outcomes.
- Maintain a moderation decision history.

### Module F — Matching engine

This is the core technical module and should be built as a dedicated backend service, not as a collection of controller conditionals.

F1. Blood compatibility rules

P0

- Model blood group and Rh compatibility rules centrally.
- Evaluate candidate donor types against the requested type.
- Keep donor eligibility separate from blood-type compatibility.
- Version rules and test edge cases.
- Treat compatibility as preliminary candidate filtering; final compatibility is determined by qualified clinical staff.

F2. Eligibility filtering

P0

Exclude or flag donors based on:

- Unavailable status.
- Active restrictions or account suspension.
- Configured provisional donation cooldown.
- Missing required profile information.
- Incompatible blood group under the configured rule set.
- Opted-out notification preferences.
- Existing active assignment conflicts, where applicable.

F3. Nearby donor discovery

P0

- Search within a configurable radius.
- Calculate distance between the request location and donor's chosen location.
- Expand the search radius when the initial candidate pool is insufficient.
- Rank candidates without exposing exact home addresses.
- Support geospatial indexing and pagination as the donor base grows.

Implementation path: begin with coordinate-based distance calculation; move to PostgreSQL PostGIS for indexed spatial searches.

F4. Candidate ranking

P0

- Rank by suitability, distance, availability, and operational constraints.
- Consider response history only if the data is reliable and the scoring is fair.
- Explain why a candidate appears in results.
- Avoid unfairly penalizing donors for declining previous requests.
- Keep ranking weights configurable and testable.

F5. Matching diagnostics

P1

- Show why candidates were excluded.
- Record matching runs and rule versions.
- Track the number of eligible candidates.
- Detect insufficient donor availability.
- Allow authorized admins to inspect a failed matching run.

### Module G — Donor invitations and assignments

G1. Donor invitations

P0

- Notify selected candidates in controlled batches.
- Include urgency, general location, requested blood group, and a secure response link.
- Expire invitations after a configured period.
- Prevent repeat invitations for the same request and matching cycle.
- Stop sending new invitations once sufficient commitments are secured.

G2. Accept, decline, or no response

P0

- Allow a donor to accept or decline.
- Track pending responses and expiration.
- Prevent unauthorized responses to another donor's invitation.
- Allow withdrawal before the relevant cutoff under defined rules.
- Notify the requester of meaningful progress without exposing unnecessary donor details.

G3. Assignment and concurrency safety

P0

- Create a distinct assignment when a donor is selected.
- Support the required number of donor commitments.
- Handle simultaneous acceptances safely using PostgreSQL transactions and appropriate locking or atomic conditional updates.
- Prevent duplicate active assignments for the same donor/request combination.
- Release capacity if an assignment is cancelled.
- Make retrying an acceptance request safe through idempotency.

Acceptance criteria: concurrent acceptance requests cannot exceed the configured number of active commitments, even when multiple API instances are running.

### Module H — Donation lifecycle

H1. Donation coordination

P0

- Record planned appointment or collection details.
- Track donor confirmation, arrival, and outcome.
- Allow authorized hospital staff to record whether a donation was completed, cancelled, or unsuccessful.
- Keep an assignment distinct from a completed donation.
- Notify relevant parties about status changes.

H2. Request fulfillment

P0

- Track required, committed, and confirmed fulfilled quantities separately.
- Support partial fulfillment.
- Mark a request fulfilled only after the configured fulfillment criteria are met.
- Cancel or release unnecessary outstanding invitations when the need is met.
- Keep historical records after cancellation or completion.

A donor accepting an invitation must not automatically count as a completed donation.

### Module I — Notifications and real-time updates

I1. In-app notifications

P0

- Create notifications for invitations, acceptance, rejection, verification, cancellation, and fulfillment.
- Maintain read/unread state.
- Support pagination and notification history.
- Avoid duplicate notifications on retries.

I2. Email notifications

P1

- Send verification and account emails.
- Send request status updates and donor invitations.
- Track delivery attempts and failures.
- Retry temporary delivery failures safely.
- Respect user preferences for non-essential messages.

I3. Real-time updates

P1

- Update dashboards using WebSockets or Server-Sent Events.
- Push meaningful request and assignment status changes.
- Authenticate connections and authorize subscriptions.
- Reconcile missed events when users reconnect.

I4. Notification fan-out and queues

P1

- Queue matching and notification work outside HTTP requests.
- Process candidates in batches.
- Track retries, failures, and dead-letter jobs.
- Use Redis with BullMQ or a comparable job queue.
- Ensure retried jobs don't create duplicate assignments or duplicate logical notifications.

### Module J — Emergency escalation

J1. Escalation engine

P1

- Detect critical requests with insufficient donor responses.
- Expand search radius according to configurable rules.
- Notify additional eligible donor batches.
- Alert authorized hospital staff or moderators.
- Track each escalation step and its outcome.
- Stop escalation when the request is cancelled, expired, or sufficiently fulfilled.

### Module K — Volunteers and transport

K1. Volunteer management

P1

- Volunteer registration and verification.
- Skills, service area, availability, and preferred task types.
- Volunteer task discovery and acceptance.
- Task assignment, cancellation, and completion.
- Reports, reliability indicators, and moderation.

K2. Transport coordination

P1

- Request assistance travelling to an authorized collection site.
- Match suitable volunteers based on location and availability.
- Track task acceptance and progress.
- Share only the location details necessary for the task.
- Provide safe cancellation and escalation workflows.

Transport coordination must not be presented as a replacement for professional medical transport or emergency services.

### Module L — Admin dashboard and analytics

L1. Operational dashboard

P0

- Total active, verified, fulfilled, cancelled, and expired requests.
- Requests awaiting verification.
- Verified hospitals and active donors.
- Requests with insufficient matching candidates.
- Recent administrative and moderation events.

L2. Performance analytics

P1

- Time from submission to verification.
- Time from verification to first donor response.
- Request fulfillment rate.
- Invitation response and acceptance rates.
- Unfulfilled requests by blood group and geographic area.
- Volunteer task completion.
- Filters by date range, location, and request category.

L3. Demand forecasting

&#x20;P2&#x20;

- Analyze historical demand by blood group and region.
- Compare requests against verified donation outcomes and donor availability.
- Detect potential shortages.
- Produce trend reports with limitations and data-quality warnings.
- Consider statistical forecasts before introducing machine learning.

### Module M — Trust, safety, and accountability

- M1. Audit trail — P0: record critical state transitions and privileged actions.
- M2. Abuse reporting — P0: allow users to report suspicious requests, harassment, or misuse.
- M3. Rate limiting — P0: limit repeated login attempts, request creation, and invitation responses.
- M4. Idempotency — P0: prevent duplicate operations after network retries.
- M5. Fraud indicators — P1: detect repeated requests, suspicious account patterns, and repeated no-shows.
- M6. Dispute management — P1: review disputed assignment or donation outcomes.
- M7. Retention and deletion policy — P1: control document retention, account deactivation, and lawful deletion requests.
- M8. Administrative access review — P1: audit privileged roles and sensitive-data access.

Do not automatically ban people based on a risk score alone. Flag suspicious activity for human review.

### Module N — Frontend experience

- N1. Landing page — P0: explain the platform, show verified public requests, and provide clear registration actions.
- N2. Search and discovery — P0: filter requests by permitted location, urgency, and status.
- N3. Role-specific dashboards — P0: separate donor, requester, hospital, volunteer, and admin experiences.
- N4. Responsive layouts — P0: usable on mobile, tablet, and desktop.
- N5. Accessible forms — P0: labels, keyboard navigation, readable contrast, and understandable validation.
- N6. Loading and error states — P0: loading indicators, empty states, retry actions, and API error messages.
- N7. Maps — P1: show approximate distance and authorized facility locations.
- N8. Internationalization — P1: support English and Bangla.
- N9. Installable PWA — P2: provide a mobile-like experience without requiring a native app.
- N10. Dark mode — P2: optional theme preference.

## 4. Database design: entities and relationships

Use PostgreSQL as the source of truth. Keep the schema normalized around business concepts, and introduce extra tables only when a real workflow requires them.

The following is a conceptual schema, not a list of exact columns or a finished Prisma schema.

### Domain 1 — Identity and access

User

Central identity and authentication

Role / Permission

Authorization model

Session / Token

Authentication lifecycle

Donor Profile

1:1 with user

Patient Profile

1:1 with user

Volunteer Profile

1:1 with user

Hospital Staff Membership

User ↔ Hospital

A hospital should be a separate organization entity. Staff membership is the link between a user and a hospital, allowing multiple staff members per hospital.

### Domain 2 — Hospitals and requests

Hospital

Organization and verification state

Blood Request

Required blood, quantity, urgency, deadline, location, current state

Request Verification

One request → many review events

Request Status History

One request → many transitions

Requests created by patients can reference a requester and an authorized collection facility. Requests created by hospital staff should also reference the responsible hospital and creating staff member.

### Domain 3 — Matching and donation

Blood Request

Donor Match

Candidate suitability, score, match run, invitation state

Assignment

Actual donor commitment or reservation

Donation Record

Appointment, arrival, verified outcome, historical record

A request has many candidates; a candidate may be considered for multiple requests over time. Assignments represent actual commitments, while donation records represent outcomes. Keep these entities distinct.

### Domain 4 — Notifications and operations

Notification

User → many notifications

Delivery Attempt

Notification → delivery attempts

Audit Log

Actor → many actions

Abuse Report

Reporter → report; moderator → decision

Volunteer Task

Task → assignment and progress

Matching Run

Request → multiple matching attempts

### Complete conceptual entity checklist

Use this as your database planning checklist.

| Domain                  | Entities to consider                                         |
| ----------------------- | ------------------------------------------------------------ |
| Identity                | User, Role, Permission, UserRole, Session or RefreshToken    |
| Profiles                | DonorProfile, PatientProfile, VolunteerProfile               |
| Hospital                | Hospital, HospitalStaffMembership, HospitalVerification      |
| Location                | Location or spatial attributes attached to relevant entities |
| Donor availability      | AvailabilityPeriod, DonorPreference, EligibilityAssessment   |
| Blood requests          | BloodRequest, RequestVerification, RequestStatusHistory      |
| Matching                | MatchingRun, DonorMatch, Assignment                          |
| Donation                | DonationRecord, Appointment or DonationEvent                 |
| Notifications           | Notification, NotificationDelivery, NotificationPreference   |
| Volunteer operations    | VolunteerTask, TaskAssignment, TaskStatusHistory             |
| Safety                  | AbuseReport, ModerationAction, AuditLog                      |
| Advanced infrastructure | OutboxEvent, IdempotencyRecord, configurable rules           |

Do not create all these tables immediately. The first implementation can combine a few concepts where their lifecycles are simple. Separate them when you need independent history, retries, permissions, or multiple records per parent.

### Relationship rules to establish before coding

1. A user has at most one donor profile in the initial design.
2. A hospital can have many staff members and many requests.
3. A request can have many verification events and status transitions.
4. A request can have many donor matches.
5. A donor can be matched to many requests, but overlapping assignments must follow your reservation rules.
6. An assignment can have zero or one resulting donation record in the initial design.
7. A user can receive many notifications and generate many audit events.
8. A volunteer can perform many tasks over time.
9. A matching run belongs to a request and records which rules or version produced its results.
10. Deleting a user must not silently destroy records needed for audit, fulfillment, or dispute resolution.

## 5. Core business workflows

These are the workflows the database, APIs, and UI must support consistently.

### Workflow 1 — Patient creates an emergency request

1\. Submit request

Validate required information and save the request.

2\. Verify

An authorized reviewer or verified workflow assesses the request.

3\. Find candidates

Filter by compatibility rules, eligibility, availability, and distance.

4\. Invite donors

Create invitation records and schedule notifications.

5\. Coordinate

Handle responses, reserve assignments, and update the requester.

6\. Confirm outcome

Authorized staff record donation outcomes and fulfillment.

### Workflow 2 — Hospital creates a request

1. Hospital staff signs in.
2. Backend verifies hospital membership and permission.
3. Staff creates a request associated with their hospital.
4. Backend validates the required details.
5. Request follows the applicable verification workflow.
6. Once approved, the matching engine starts.
7. Hospital staff tracks responses and records authorized outcomes.

### Workflow 3 — Donor accepts an invitation

1. Donor receives an invitation.
2. Backend validates invitation ownership, expiry, and current request status.
3. Backend checks whether the donor can still be considered.
4. A transaction attempts to reserve the required commitment.
5. The database prevents excess or duplicate active assignments.
6. The system records the response and creates appropriate notifications.
7. A later workflow records the actual donation outcome.

### Workflow 4 — Request cannot find donors

1. The matching run returns too few eligible candidates.
2. The system records the failed or insufficient result.
3. The request remains open if it is still valid.
4. The escalation engine may expand the search radius.
5. Additional invitations are created in batches.
6. Hospital staff or authorized moderators receive an alert if the shortage continues.
7. The system stops escalation when the request is fulfilled, cancelled, or expired.

## 6. Request and assignment state machines

State machines prevent invalid transitions and inconsistent data.

### Blood request lifecycle

```
DRAFT
  ↓ submit
PENDING_VERIFICATION
  ├── request changes → NEEDS_INFORMATION
  ├── reject          → REJECTED
  └── approve         → VERIFIED
                          ↓
                       MATCHING
                          ↓
                       ACTIVE
                     ↙       ↘
          PARTIALLY_FULFILLED  FULFILLED
                     ↘       ↙
                    CANCELLED / EXPIRED
```

The arrows are illustrative, not permission to transition arbitrarily. For example, a fulfilled request must not return to active merely because a delayed job executes.

Define the allowed transitions centrally and validate them in the backend. Keep a separate verification state if it avoids mixing moderation decisions with operational request status.

### Assignment lifecycle

```
PENDING_RESPONSE
    ├── donor declines → DECLINED
    ├── invitation expires → EXPIRED
    └── donor accepts → ACCEPTED
                           ↓
                        CONFIRMED
                           ↓
                        ARRIVED
                           ↓
                    Donation outcome
                     ↙           ↘
                COMPLETED      UNSUCCESSFUL

Other permitted outcomes: CANCELLED, WITHDRAWN, NO_SHOW
```

The exact states should reflect actual business requirements. Avoid adding states that do not have a clear meaning or transition rule.

## 7. API requirements

Use a versioned REST API, for example `/api/v1`. These are proposed endpoints, not a final route specification.

| Domain            | Example endpoints                                                           | Purpose                      |
| ----------------- | --------------------------------------------------------------------------- | ---------------------------- |
| Authentication    | `POST /auth/register`, `POST /auth/login`, `POST /auth/logout`              | Account access               |
| Profiles          | `GET /me`, `PATCH /me`, `GET /donors/me`                                    | User and donor settings      |
| Donor discovery   | `GET /donors/nearby`                                                        | Authorized nearby search     |
| Availability      | `PATCH /donors/me/availability`                                             | Update availability          |
| Requests          | `POST /blood-requests`, `GET /blood-requests`, `GET /blood-requests/:id`    | Create and discover requests |
| Request lifecycle | `PATCH /blood-requests/:id`, `POST /blood-requests/:id/cancel`              | Controlled changes           |
| Verification      | `POST /blood-requests/:id/verification-decisions`                           | Review requests              |
| Matching          | `POST /blood-requests/:id/matching-runs`, `GET /blood-requests/:id/matches` | Trigger and inspect matching |
| Invitations       | `GET /donors/me/invitations`, `POST /invitations/:id/respond`               | Donor responses              |
| Assignments       | `GET /assignments/:id`, `POST /assignments/:id/confirm`                     | Coordinate commitments       |
| Donations         | `POST /assignments/:id/donation-record`, `PATCH /donations/:id/outcome`     | Record donation outcomes     |
| Hospitals         | `POST /hospitals`, `GET /hospitals/:id`                                     | Organization management      |
| Notifications     | `GET /notifications`, `PATCH /notifications/:id/read`                       | Notification management      |
| Volunteers        | `GET /volunteer-tasks`, `POST /volunteer-tasks/:id/accept`                  | Task coordination            |
| Administration    | `/admin/reports`, `/admin/users`, `/admin/moderation`                       | Platform management          |

### API engineering rules

- Use Zod to validate request parameters, body, and query strings.
- Use a consistent error response format.
- Return correct HTTP status codes.
- Paginate list endpoints.
- Apply authorization at the service/resource level, not just the route level.
- Add rate limits to sensitive endpoints.
- Use idempotency for operations that may be retried.
- Never trust client-supplied distance, match score, role, verification state, or fulfillment status.
- Keep external notification sending outside the main database transaction; use durable job processing or an outbox pattern where needed.

## 8. Non-functional requirements

These requirements define the quality of the product, not just its features.

Security and privacy

Secure authentication, resource-level authorization, least-privilege database access, encrypted transport, safe document storage, audit logs, and minimal collection of personal and medical data.

Data integrity

Foreign keys, unique constraints, valid status transitions, transactions for assignments, and reliable fulfillment accounting.

Performance

Indexed queries, bounded pagination, spatial indexes where appropriate, asynchronous notification delivery, and no unbounded donor scans in ordinary request handlers.

Reliability

Retry-safe jobs, recoverable failures, request expiry processing, monitoring, backups, and protection against duplicate operations.

Accessibility and usability

Mobile-first layouts, keyboard accessibility, readable contrast, clear language, and understandable emergency status messages.

### Suggested initial engineering targets

These are proposed targets to test against—not guarantees of production performance.

| Metric                   | Initial target                                                               |
| ------------------------ | ---------------------------------------------------------------------------- |
| Ordinary API latency     | p95 below 500 ms under a defined test load                                   |
| Matching response        | Return a job/run identifier promptly; process larger searches asynchronously |
| Assignment correctness   | Zero over-capacity assignments in concurrency tests                          |
| Notification retries     | Recover from temporary delivery errors without duplicate logical actions     |
| Request status integrity | Reject invalid and stale transitions                                         |
| Backup recovery          | Document and test a recovery procedure                                       |
| Accessibility            | Keyboard-operable core flows and automated accessibility checks              |

For a real emergency service, performance targets alone are insufficient. Availability, incident response, operational staffing, and escalation procedures must also be defined.

## 9. Business rules and edge cases

Implement and test these explicitly.

| Scenario                                                | Expected behavior                                                                  |
| ------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| Request has no compatible donors                        | Report zero candidates and trigger permitted escalation                            |
| Donor becomes unavailable after matching                | Revalidate eligibility before accepting or confirming an assignment                |
| Two donors accept the last slot simultaneously          | Allow only the permitted number of active commitments                              |
| Same donor responds twice                               | Return a consistent result without creating duplicate assignments                  |
| Request is cancelled while notifications are queued     | Suppress unsent invitations and reject stale acceptances                           |
| Request deadline passes during matching                 | Prevent new assignments and mark expiry through a safe transition                  |
| Hospital staff tries to edit another hospital's request | Reject access                                                                      |
| Patient tries to approve their own request              | Reject the verification action                                                     |
| Notification provider is unavailable                    | Retry delivery independently of request state changes                              |
| Donor withdraws after accepting                         | Apply withdrawal rules, release capacity when appropriate, and resume matching     |
| A request is partially fulfilled                        | Continue matching only for the remaining legitimate need                           |
| Donation fails to occur                                 | Do not count it as a successful donation                                           |
| Duplicate requests describe the same need               | Flag for review; don't automatically merge without authorization                   |
| Donor deletes their account                             | Apply retention and anonymization policies without breaking required audit history |
| Matching rules change                                   | Record the rule version used by each matching run                                  |
| A delayed job runs twice                                | Make the job idempotent and validate the current request state                     |

## 10. Technical architecture

Use a modular monolith first. You do not need microservices for this project.

React + TypeScript

Dashboards · Forms · Maps · Notifications

Express + TypeScript

Routes · Validation · Authentication · Authorization

Application services

Matching · Verification · Assignments · Fulfillment

PostgreSQL

Prisma · Transactions

Redis + workers

BullMQ · Background jobs

Redis and workers are an advanced addition. The core application must remain correct even when a notification provider or worker is unavailable.

### Recommended stack

| Layer             | Technology                                                             |
| ----------------- | ---------------------------------------------------------------------- |
| Frontend          | React, TypeScript, Vite                                                |
| Routing           | React Router                                                           |
| Styling           | Tailwind CSS, shadcn/ui                                                |
| Server state      | TanStack Query                                                         |
| Forms             | React Hook Form, Zod                                                   |
| Backend           | Node.js, Express, TypeScript                                           |
| ORM               | Prisma                                                                 |
| Database          | PostgreSQL                                                             |
| Geospatial        | PostGIS, introduced after basic matching works                         |
| Authentication    | Secure cookie-based session or access/refresh token strategy           |
| Background jobs   | Redis, BullMQ                                                          |
| Real-time         | Socket.IO or Server-Sent Events                                        |
| Testing           | Vitest, Supertest, Playwright                                          |
| API documentation | OpenAPI / Swagger                                                      |
| Deployment        | Static frontend host, Node-compatible backend host, managed PostgreSQL |

Use raw SQL selectively when necessary for advanced PostgreSQL features such as PostGIS queries or specialized locking patterns that are awkward to express through your ORM.

## 11. Testing requirements

The matching engine and assignment lifecycle deserve more than ordinary CRUD tests.

### Unit tests

- Blood compatibility rule cases.
- Eligibility and cooldown calculations.
- Distance calculations and radius expansion.
- Candidate ranking.
- Priority ordering.
- Request and assignment transition rules.
- Fulfillment calculations.

### Integration tests

- User and hospital authorization.
- Request verification.
- PostgreSQL transaction behavior.
- Donor invitation response.
- Assignment uniqueness.
- Cancellation and expiration.
- Notification persistence and retries.
- Audit event creation.

### End-to-end tests

1. A patient registers and creates a request.
2. An authorized reviewer verifies it.
3. An eligible donor receives an invitation.
4. The donor accepts.
5. Authorized hospital staff records the outcome.
6. The requester sees the final request status.

Also test the failure path: no eligible donors, donor withdrawal, expired requests, and simultaneous acceptances.

## 12. Implementation roadmap

Build the project in this order. Each phase should be demonstrably working before you move on.

### Build checklist

0 of 34 tasks

Phase 1 — Foundation

Week 1

0/5

Initialize React and Express TypeScript projects

Connect PostgreSQL and Prisma

Set up migrations, validation, logging, and error handling

Implement registration, login, and authorization

Create a minimal responsive application shell

Phase 2 — Profiles and organizations

Week 2

0/4

Implement donor and patient profiles

Implement hospital registration and verification

Build donor availability management

Add hospital membership and ownership checks

Phase 3 — Requests and moderation

Week 3

0/4

Create and update blood requests

Implement request state transitions

Build verification queue and review history

Add request cancellation and expiration rules

Phase 4 — Matching engine

Weeks 4–5

0/5

Implement compatibility and eligibility services

Build coordinate-based nearby search

Add candidate filtering and ranking

Write unit tests for edge cases

Add matching-run history and diagnostics

Phase 5 — Invitations and assignments

Week 6

0/4

Create invitations and donor responses

Implement assignment capacity protection

Handle concurrent acceptance and idempotency

Track partial fulfillment and donor withdrawal

Phase 6 — Donation and notifications

Week 7

0/4

Implement donation outcomes and history

Add in-app notifications

Build notification preferences and delivery tracking

Implement request and assignment timelines

Phase 7 — Reliability and advanced workflows

Weeks 8–9

0/4

Add background jobs and retry handling

Implement escalation and request expiry workers

Add audit logs, rate limiting, and abuse reports

Test failure recovery and concurrent operations

Phase 8 — Portfolio polish

Week 10

0/4

Build admin analytics and complete dashboards

Add automated end-to-end tests

Document architecture, API, and database relationships

Deploy, monitor, and prepare a technical demonstration

Copy roadmap checklist

The 10-week schedule is an indicative plan, not a deadline guarantee. It assumes regular development time, and the advanced phases may take longer as you encounter real implementation problems.

## 13. MVP acceptance criteria

Do not call the MVP complete until these outcomes work:

- Users can register, authenticate, and access only permitted resources.
- Donors can create profiles and control their availability.
- Patients and authorized hospital staff can create blood requests.
- Unverified requests cannot initiate donor outreach.
- Matching returns candidates based on the configured rules.
- Donors can accept or decline invitations.
- Concurrent acceptance cannot exceed the available assignment capacity.
- Assignment and donation outcomes remain distinct.
- Partial fulfillment, cancellation, and expiration work correctly.
- Notifications and audit history are persisted.
- Critical workflows have automated tests.
- Sensitive data is protected by authorization and privacy controls.
- The application is deployed with documented setup instructions.

## 14. Success metrics

Measure the platform using operational outcomes rather than page views alone.

| Metric                           | What it tells you                                                  |
| -------------------------------- | ------------------------------------------------------------------ |
| Request verification time        | How quickly requests are reviewed                                  |
| Time to first eligible candidate | How efficiently matching works                                     |
| Time to first donor acceptance   | How quickly donors respond                                         |
| Fulfillment rate                 | How many verified requests reach their defined completion criteria |
| No-candidate rate                | Where blood-group or geographic shortages occur                    |
| Donor response rate              | Whether invitations reach responsive donors                        |
| Assignment cancellation rate     | How often commitments fail                                         |
| Notification delivery rate       | Whether communication infrastructure is working                    |
| Duplicate assignment incidents   | Whether concurrency protection is correct                          |
| Unauthorized access incidents    | Whether access control is functioning                              |

Define the denominator and measurement window for every metric. For example, exclude cancelled requests from certain fulfillment calculations only if that exclusion is explicitly documented.

## 15. Final scope recommendation

For your first complete portfolio release, prioritize these five technical challenges:

1\. Request state machine

Design valid transitions, verification history, expiration, cancellation, and partial fulfillment.

2\. Matching engine

Separate compatibility, eligibility, geographic filtering, and ranking into independently testable services.

3\. Concurrency-safe assignments

Use database constraints, transactions, and idempotency to prevent over-allocation under simultaneous requests.

4\. Reliable notification pipeline

Separate business state changes from asynchronous delivery, retries, and duplicate job handling.

5\. Security and auditability

Enforce resource-level permissions, protect personal information, and preserve a reliable record of privileged actions.

My recommendation: start by writing the requirements, drawing the entity relationship diagram, defining the request and assignment state machines, and identifying the database constraints. Then implement one complete vertical slice—from creating a request to recording its outcome—before expanding into volunteers, transport, advanced analytics, and forecasting.

That approach gives you a finished, testable product rather than a large collection of partially implemented features.

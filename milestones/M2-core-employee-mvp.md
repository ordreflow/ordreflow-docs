# M2: Core Employee MVP

## Goal

Deliver a small-team pilot for registering, reviewing, approving, and exporting time
on assigned work.

## Scope

- Simple pilot authentication
- Employee and leader/manager roles
- Assigned orders or tasks
- Mobile manual-duration time registration
- Backend validation and authorization
- Personal weekly overview
- Weekly totals
- Editing pending or denied time entries
- Leader/manager review, approval, and denial of permitted time entries
- Locking approved time entries
- Basic export of approved time entries
- Clear loading, error, and empty states
- Domain/API contract for this milestone

Full administration, user and role management, advanced reporting, advanced export
formats or integrations, clock-in/out time registration, offline synchronization,
and multi-level approval can be added in later milestones.

## Acceptance Criteria

- A pilot employee can authenticate through the agreed simple pilot login.
- Employees can only access their permitted data and assigned orders or tasks.
- A leader/manager can only access time entries within the leader/manager's permitted scope.
- An employee can register a manual duration on assigned work from a phone.
- Employees can review their weekly registrations and total hours.
- Employees can edit pending or denied entries, while approved entries cannot be edited.
- The API enforces validation and authorization independently of the frontend.
- A leader/manager can review and approve or deny permitted time entries.
- Approved entries can be exported in the agreed pilot format for a selected period.
- Loading, error, and empty states are clear on the relevant mobile screens.
- The complete pilot flow works end-to-end: employee login, assigned work, time
  registration, review or correction, leader/manager approval or denial, and export
  of approved entries.

## Open Decisions

- The exact mechanism for the simple pilot login.
- The export format, exported fields, and export permissions.
- How leaders/managers are associated with the employees they can review.
- Whether denying an entry requires a reason and how an employee resubmits it.
- The exact role names used by the application.

## Result

To be completed after the milestone.

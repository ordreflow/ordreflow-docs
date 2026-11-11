# M3 Definition of Done for Requirements and Use Cases

Status: **Draft for review**

This document defines when an M3 requirement or use case (user story) is
**done**. It applies to the functional requirements `FR-01`–`FR-40` and the
non-functional requirements in the
[M3 requirements](../Requirements/M3-ordreflow-requirements.md), and to the
acceptance criteria in the [M3 milestone](../milestones/M3-PWA.md).

The issue-level Definition of Done in
[task planning and backlog](task-planning-and-backlog.md) still applies to every
issue. This document adds the criteria for when a **requirement** is complete.
A requirement is often delivered through several frontend, backend, and
documentation issues. All of those issues being closed does not by itself
make the requirement done.

## Levels of done

| Level | Done when |
| --- | --- |
| Issue | The [issue-level Definition of Done](task-planning-and-backlog.md#definition-of-done) is met. |
| Requirement / use case | The general requirement DoD below **and** the requirement-specific criteria are met. |
| Milestone M3 | Every Must requirement is done and the M3 acceptance scenarios are demonstrated (see [Milestone DoD](#milestone-m3-definition-of-done)). |

## General Definition of Done for a requirement

A functional requirement (`FR-xx`) is done when all applicable points are met.

### Scope and traceability

- [ ] Every issue that implements the requirement references its ID (for
  example `FR-23`) in the title or description, and is linked to the parent
  issue when there is one.
- [ ] The user story's goal is achievable end to end by the role named in the
  story, in the deployed test environment, without manual database changes.
- [ ] Anything deliberately left out is written down as a follow-up issue or
  as an open decision, not silently dropped.
- [ ] Decisions the requirement depends on (see **Open decisions** in the
  requirements) are agreed and recorded, or the chosen assumption is
  documented in the issue.

### Implementation

- [ ] The backend and frontend parts are merged into `main` through reviewed
  pull requests, following [branching and PRs](branching-and-prs.md).
- [ ] Authorization, status rules, and date-policy rules are enforced in the
  **API**, not only hidden in the UI (`SEC-01`, `SEC-03`).
- [ ] Lists, direct reads, and direct URLs return only data in the user's
  permitted scope (`SEC-02`).
- [ ] Expected failures return consistent structured errors that the frontend
  shows in an understandable way (`INT-01`).
- [ ] Database changes are delivered as migrations and work on existing data
  (`SUP-03`).

### Quality and verification

- [ ] Automated tests cover the main success path, the most important
  failure/permission paths, and the business rules in the requirement
  (`SUP-02`).
- [ ] The relevant screens are tested on a supported phone viewport with touch,
  and have loading, error, and empty states (`USE-01`, `USE-02`).
- [ ] The UI never shows an operation as saved before the backend has
  confirmed it (`USE-03`).
- [ ] Dates, time zones, and durations follow `OTH-01`.
- [ ] Logs contain no passwords, tokens, entry comments, or file contents
  (`SEC-04`, `SUP-01`).
- [ ] No known critical or high-severity defect remains open for the
  requirement.

### Documentation and demonstration

- [ ] [API contract](api-contract.md) and other affected docs are updated.
- [ ] The requirement has been demonstrated to the team, and to the product
  owner when relevant, and the issue status has moved to `Done`.

## Requirement-specific criteria

The criteria below come in addition to the general DoD. They describe the
minimum observable behavior that must be demonstrated for each use case.

### Time registration and weekly overview

| ID | Done when |
| --- | --- |
| `FR-01` | An employee can register a time entry on a work item they can access. The category and cost-bearing department are derived from the work item. Registering on an inaccessible work item is rejected by the API. |
| `FR-04` | An employee can select a week and see only their own entries for that week, with work item, category, duration, and status. |
| `FR-10` | An employee can edit or delete an entry only while it is Draft or Rejected and inside an open period (or covered by a valid exception). Approved and Closed entries cannot be changed through the UI or the API. |
| `FR-12` | A rejected entry shows the reviewer's reason to the employee, who can correct and resubmit it while the period is open. |
| `FR-15` | The weekly total equals the sum of the employee's entries for the selected week, uses the agreed duration unit, and updates after changes. |
| `FR-23` | An employee can save a Draft and submit it as Ready for approval. Only Ready entries appear for review. Allowed state transitions are enforced by the API. |

### Offline and PWA

| ID | Done when |
| --- | --- |
| `FR-16` | The app can be installed as a PWA on the supported phones and browsers and opens the supported mobile workflow from the home screen. |
| `FR-17` | An employee can save a new entry with no connection. It is shown as **queued**, not as saved, within the proposed time target (`PERF-02`). |
| `FR-18` | Queued entries synchronize automatically when the connection returns. Each entry shows pending, saved, or needs attention. A retry never creates a duplicate (`INT-02`). Revoked access or a passed cutoff gives a clear failure with a next action instead of a false success. |
| `FR-27` | On the last day of a weekly, calendar-monthly, or custom period, each active employee gets exactly one reminder. Daily mode sends none (`INT-04`). |

Offline requirements are also done only when local data is separated per user,
sign-out warns about unsynchronized entries, and confirmed sign-out clears
local data (`SEC-07`).

### Review and approval

| ID | Done when |
| --- | --- |
| `FR-02` | A category reviewer sees Ready entries only in the categories they review, including entries from employees in other departments. Managing an employee does not give access to review their entries. |
| `FR-03` | A category reviewer can approve or reject (with a required reason) only permitted entries. Approved entries are locked. A user without reviewer rights is rejected by the API. |
| `FR-20` | An administrator can assign one or more reviewers to a category, and new Ready entries in that category reach those reviewers. |
| `FR-24` | An authorized viewer can see a timeline of edits, submissions, reviews, and status changes with actor and UTC time shown in local time. Only necessary data is stored, and ordinary users cannot edit it (`SEC-06`). |

### Work structure, assignment, and planning

| ID | Done when |
| --- | --- |
| `FR-08` | A department manager can create projects, tickets, or tasks under categories available to their departments, and nowhere else. |
| `FR-09` | An employee sees general selectable work and work assigned to them, and nothing outside their permitted scope. |
| `FR-19` | An authorized category editor can create and edit categories and subcategories with one cost-bearing department and visibility for everyone or selected departments. The charged department cannot change once time is registered (`SEC-08`). |
| `FR-21` | Assignment-only work is visible only to assignees and people with authority over it. Other users cannot find it in lists, search, or by direct URL. |
| `FR-22` | A manager can assign one work item to several eligible employees with separate expected hours and target completion date **per employee**. |
| `FR-31` | A manager sees allocated versus registered hours and upcoming or overdue assignments for employees in their departments, without opening private entry details in other departments' categories. |
| `FR-32` | **Assigned to me** filters the already permitted list. Turning it off shows general work again, and showing a parent never exposes private siblings (`USE-04`). |
| `FR-33` | An employee can mark assigned work as finished for manager review. This does not approve or change the status of the related time entries. |

### Users, access, and periods

| ID | Done when |
| --- | --- |
| `FR-05` | An administrator can create and deactivate local users. Deactivated users cannot sign in, and their historical entries are kept. |
| `FR-06` | A user can sign in and out with local login. Passwords are hashed, sign-in is rate-limited, and the API is only reachable over HTTPS in test and production (`SEC-04`). |
| `FR-13` | An administrator can grant system access independently of the displayed job title. Changing the job title does not change access. |
| `FR-14` | An administrator can assign users and managers to one or more departments, and managers can then plan work for exactly those employees. |
| `FR-25` | An administrator can choose daily, weekly (Monday–Sunday), calendar-monthly, or custom monthly (cutoff day 1–28). Periods lock right after their last day in the company's time zone, with tests for month changes and boundary days. |
| `FR-26` | An administrator can give a named employee a time-limited, audited exception for specific work dates and actions. It never unlocks Approved or Closed entries (`SEC-09`). |

### Reporting and export

| ID | Done when |
| --- | --- |
| `FR-07` | An authorized user can create an export of Approved entries for a period. The selection is stored as a fixed batch, and creating it does not close any entries (`INT-03`). |
| `FR-11` | A manager can see a D3.js chart of the ten categories or work items (as agreed) with the most hours in a selected date range, based only on data in their scope. |
| `FR-28` | An authorized user can see previous exports with their entries, creator, dates, and completion status. Others cannot see export history. |
| `FR-29` | Downloading a previous export again returns the same original selection, even if newer Approved entries exist. |
| `FR-30` | Marking an export completed closes all its entries together in one operation, records who and when, and is safe to retry. The UI asks for confirmation first and separates downloaded from completed batches (`USE-03`). |

### Should and Could requirements

Should (`FR-34`–`FR-38`) and Could (`FR-39`, `FR-40`) requirements use the
same general DoD when they are delivered. In addition:

- `FR-34`, `FR-35`, `FR-38`: filters and navigation are keyboard accessible,
  keep context from related pages, and still work on mobile (`USE-05`). The
  overview meets the agreed response target (`PERF-03`).
- `FR-36`, `FR-37`: notifications use the existing reminder mechanism, match
  real events, and do not create misleading duplicates (`INT-05`, `SUP-04`).
- `FR-39`, `FR-40`: delivered or deferred **together**. Done only when the
  same person gets the same user and access through both methods, and
  `SEC-10`, `INT-06`, `USE-06`, and `SUP-05` are met.

If a Should or Could requirement is not delivered, it is moved to a later
milestone or to [future-work ideas](future-work-ideas.md); it is not marked as
done.

## Non-functional requirements

Non-functional requirements are not finished as separate stories. They are done
when they have been checked as part of the functional requirements they affect,
and the result is documented.

| Category | Done when |
| --- | --- |
| Performance (`PERF-01`–`PERF-03`) | The test profile (devices, dataset, concurrency) is agreed, the measurement has been run on a representative test deployment, and the results are recorded. Until then the targets are **proposed** and cannot be marked as met. |
| Security and privacy (`SEC-01`–`SEC-09`) | Automated tests prove that the API refuses actions outside the user's scope, including direct URLs and offline sync. The GDPR retention and deletion policy (`SEC-05`) is agreed before rollout. |
| Integration (`INT-01`–`INT-04`) | Error formats, idempotent offline operations, export batch operations, and reminders are described in the API contract and covered by tests, including retries. |
| Usability (`USE-01`–`USE-04`) | Key flows are checked on a supported phone against the agreed accessibility target (proposed WCAG 2.2 AA). |
| Supportability (`SUP-01`–`SUP-03`) | Structured logs with correlation IDs exist. The test areas listed in `SUP-02` are automated. Migrations, backup/restore, and failed-sync support steps are documented and have been tried in the test environment. |
| Other (`OTH-01`) | Date-only values, company time zone cutoffs, UTC audit timestamps, and a consistent duration unit are verified by tests. |

## Milestone M3 Definition of Done

M3 is done when:

- [ ] All Must requirements (`FR-01`–`FR-33` and the Must non-functional
  requirements) meet this Definition of Done.
- [ ] The seven Must acceptance scenarios in the
  [M3 milestone](../milestones/M3-PWA.md#acceptance-criteria-for-must) have
  been demonstrated end to end in the test environment, including on a real
  phone.
- [ ] Open decisions that affect the delivered behavior are agreed and
  recorded in the requirements or milestone document.
- [ ] Should and Could requirements are marked as delivered, or moved to a
  later milestone or future-work ideas.
- [ ] The **Result** section of the M3 milestone document is filled in.

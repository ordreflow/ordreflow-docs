# M3 OrdreFlow Requirements

Status: **Draft for review**

This M3 version builds on the
[M2 requirements](M2-core-employee-mvp-requirements.md). Functional
requirements `FR-01`–`FR-15` keep their previous IDs and priority order, with
wording updated for the category, department, and review model. New functional
requirements are ordered by business priority from `FR-16` onward. The M2
document remains unchanged. The [M3 milestone](../milestones/M3-PWA.md)
summarizes this scope.

## Priority and scope

- **Must:** Required to complete M3.
- **Should:** Intended if capacity allows, without holding up completion of the
  Must requirements.
- **Could:** Stretch requirements that can move to a later milestone.

Requirements are grouped by priority. The first fifteen functional stories
retain their inherited order; the newer stories are ranked within their Must,
Should, and Could groups. The order describes business importance, not
implementation dependency. Non-functional requirements are grouped by priority
and category. Security and privacy rules apply whenever a feature is delivered.

Each company has a separate OrdreFlow environment. Local sign-in is the
baseline; Entra ID and the Owner's choice of sign-in methods are a paired Could.
Large optional features requiring new infrastructure are documented in
[future-work ideas](../docs/future-work-ideas.md) instead of being M3 Should or
Could requirements.

## Domain and access assumptions

- A user's displayed role or job title does not grant access. System
  administration, department management, category review, and work-item
  assignment are separate grants. Owner-configured roles and permissions are
  future-work ideas, not part of this milestone.
- Users can belong to multiple departments and manage more than one department.
  Department managers plan employees' work in the departments they manage;
  category reviewers approve time in the categories assigned to them. Managing
  an employee does not automatically make someone the reviewer of that
  employee's time entries.
- Categories may contain subcategories and work items such as projects, tickets,
  and tasks. A category is visible to all active users in the environment or
  only to members of selected departments. Visibility permits browsing and
  selecting the category, not viewing other employees' entries. This draft
  assumes one cost-bearing department per category, even if it is visible to
  multiple departments or everyone.
- Work items may be generally selectable or assignment-only. Assignment-only
  items are visible to assignees and people with explicit authority over that
  category or work. Department managers may see aggregate hours for employees
  they manage without automatically seeing private entry details in another
  department's category.
- Every time entry references a work item. General work such as HR meetings is
  represented by a reusable work item under a category. The category and
  cost-bearing department are derived from that work item rather than stored
  redundantly on the entry. Work items do not move between categories, and a
  category's charged department cannot change after time is registered under it.
- The proposed time-entry states are **Draft**, **Ready for approval**,
  **Approved**, **Rejected**, and **Closed**. Approved entries remain locked;
  Closed entries cannot be changed even with a temporary editing exception.
  An approved entry becomes Closed only when its persisted export batch is
  explicitly completed. Completing a task and approving a time entry are
  separate processes.
- A company-wide daily, weekly, calendar-monthly, or custom monthly date policy
  controls an employee's ability to create, edit, or delete entries by work
  date. A selected custom cutoff day is 1–28; the period ends on that day and
  the next begins the day after it. Periods lock immediately after their last
  day in the company's time zone, without a grace period. Only weekly,
  calendar-monthly, and custom periods send a last-day reminder. A rejected
  entry in a locked period requires an administrator-issued exception to be
  corrected and resubmitted.
- Each export keeps a fixed set of approved entries, a creator, and its
  creation/completion history. Downloading it again does not change the
  selection or close entries. Explicit completion closes all of the export's
  entries together.

## Functional requirements

Functional requirements are user stories.

### Must

| ID | Requirement |
| --- | --- |
| `FR-01` | As an employee, I want to register a time entry against an accessible work item, so that my worked hours are recorded against the correct category. |
| `FR-02` | As a category reviewer, I want to view time entries in the categories I review, so that I can inspect them before they are finalized. |
| `FR-03` | As a category reviewer, I want to approve or reject permitted time entries, so that the responsible team can review them for invoicing or payroll. |
| `FR-04` | As an employee, I want to view my time entries for a selected week, so that I can review what I registered. |
| `FR-05` | As an administrator, I want to create and deactivate local user accounts, so that employees and managers can access the system while historical work is retained as required. |
| `FR-06` | As a user, I want to authenticate with local login, so that I can securely access the features and data available to me. |
| `FR-07` | As an authorized administrator or payroll user, I want to create an export containing a fixed set of approved entries for a selected period, so that I can use the records for invoicing or payroll without closing them prematurely. |
| `FR-08` | As a department manager, I want to create permitted projects, tickets, or tasks under categories available to my departments, so that work can be assigned to employees. |
| `FR-09` | As an employee, I want to view general work and work assigned to me, so that I know what I can register time against. |
| `FR-10` | As an employee, I want to correct a time entry that is still open for changes, so that I can fix mistakes before it is finalized. |
| `FR-11` | As a manager, I want to view a D3.js visualization of the ten categories or work items with the most registered hours for a selected date range, so that I can understand how working time is distributed. |
| `FR-12` | As an employee, I want to know why a time entry was rejected, so that I can correct and resubmit it. |
| `FR-13` | As an administrator, I want to assign fixed system access independently of a user's displayed job title, so that users receive the access needed for their work. |
| `FR-14` | As an administrator, I want to assign users and department managers to one or more departments, so that managers can plan work for the correct employees. |
| `FR-15` | As an employee, I want to view the total time for a selected week, so that I can check my total hours. |
| `FR-16` | As a user, I want to install OrdreFlow as a PWA, so that I can access the supported mobile workflow from my device. |
| `FR-17` | As an employee, I want to save a time entry while offline, so that I can record work without a connection. |
| `FR-18` | As an employee, I want queued entries to synchronize when connectivity returns and show whether each is pending, saved, or needs attention, so that I know what the server accepted. |
| `FR-19` | As an authorized category editor, I want to create and edit categories and subcategories with a cost-bearing department and visibility for everyone or selected departments, so that work is organized and charged appropriately. |
| `FR-20` | As an administrator, I want to assign one or more reviewers to categories, so that time entries reach the responsible reviewers regardless of an employee's home department. |
| `FR-21` | As an employee, I want assignment-only projects, tickets, and tasks to be visible only to me when assigned and to people with authority over them, so that private work is protected. |
| `FR-22` | As a department manager, I want to assign work items to one or more eligible employees with expected hours and a target completion date **for each employee**, so that I can delegate and plan their capacity. |
| `FR-23` | As an employee, I want to save a draft and submit it for approval, so that reviewers can distinguish work in progress from work ready for review. |
| `FR-24` | As an authorized viewer, I want a time-entry timeline of relevant edits, reviews, and status changes with the actor and time, so that I can understand what happened. |
| `FR-25` | As an administrator, I want to configure a daily, Monday–Sunday weekly, calendar-monthly, or custom monthly time-entry period with a cutoff day from 1 to 28, so that employees can create, edit, and delete eligible entries for permitted work dates until the period closes. |
| `FR-26` | As an administrator, I want to grant a named employee a time-limited exception for specific work dates and create/edit/delete actions after cutoff, so that late corrections, including rejected entries, are possible without unlocking Approved or Closed entries. |
| `FR-27` | As an employee, I want a reminder on the last day of a weekly, calendar-monthly, or custom monthly registration period, so that I can submit entries and check that offline work has synchronized before cutoff; daily mode needs no reminder. |
| `FR-28` | As an authorized administrator or payroll user, I want to view previous exports with their original entries, creators, dates, and completion status, so that I can trace what was sent for payroll or invoicing. |
| `FR-29` | As an authorized administrator or payroll user, I want to download the original selection from a previous export again, so that I can recover the same export without accidentally including newer entries. |
| `FR-30` | As an authorized administrator or payroll user, I want to mark an export completed after processing it, so that all its included entries become Closed together and their completion is recorded. |
| `FR-31` | As a department manager, I want a basic view of employees' allocated versus registered hours and upcoming or overdue assignments within my scope, so that I can plan work without opening unrelated private entries. |
| `FR-32` | As an employee, I want an **Assigned to me** filter, so that I can focus on delegated work while still finding general work when the filter is off. |
| `FR-33` | As an employee, I want to mark assigned work as finished for manager review, so that task completion can be handled separately from approval of time entries. |

### Should

| ID | Requirement |
| --- | --- |
| `FR-34` | As an authorized manager or administrator, I want to filter a time-entry overview by category, work item, employee, date, and status and reach it from related pages, so that I can inspect permitted entries efficiently. |
| `FR-35` | As a department manager, I want a more detailed overview of expected versus actual hours across my assignments, so that I can plan capacity beyond the basic employee view. |
| `FR-36` | As an employee, I want to be notified of new assignments or rejected entries through the existing notification mechanism, so that I know when to act. |
| `FR-37` | As an employee, I want to be notified when an offline registration has synchronized or needs correction through the existing notification mechanism, so that I do not mistake a queued entry for a saved one. |
| `FR-38` | As a desktop user, I want clearer navigation for management pages, so that I can move between related work while mobile navigation remains usable. |

### Could

| ID | Requirement |
| --- | --- |
| `FR-39` | As a user, I want to sign in with my company's Microsoft Entra ID when enabled, so that I can use my company account. |
| `FR-40` | As an Owner, I want to choose local login, Entra ID, or both for this company's environment, so that the company can select its supported sign-in methods. |

`FR-39` and `FR-40` are delivered or deferred together. When both methods are
enabled for one person, they must resolve to the same OrdreFlow user and access.

## Non-functional requirements

The six requirements in the earlier document are retained here under their
respective category identifiers. Performance values below are **proposed
targets for review**, pending agreement on supported devices, representative
data, and the test environment.

### Must

#### Performance

| ID | Requirement |
| --- | --- |
| `PERF-01` | On a representative test deployment with 25 concurrent users and 50,000 time entries, at least 95% of weekly-entry, accessible-work, and reviewer-list API requests shall complete within 2 seconds at the API boundary. **Proposed target.** |
| `PERF-02` | On supported phones, at least 95% of offline saves shall show a locally queued confirmation within 1 second; for up to ten queued entries, a healthy connection shall produce a server-confirmed outcome within 60 seconds after reconnection. **Proposed targets.** Failed or conflicted entries must not be shown as saved. |

#### Security

| ID | Requirement |
| --- | --- |
| `SEC-01` | All protected operations shall be authenticated and authorized by the backend; frontend visibility shall not be treated as a security boundary. A displayed job title shall not grant system, department, or category access. |
| `SEC-02` | API responses, lists, direct reads, export history, and re-downloads shall contain only data within the user's permitted scope. Category visibility to **Everyone** means active users of this environment can select it, not that they can view other people's entries or restricted child work. |
| `SEC-03` | The backend shall enforce category-reviewer responsibility, department-management and work-item assignments, the company-local work-date period, explicit completion, and status rules on every write, including offline synchronization. A manager does not acquire approval rights merely by managing an employee. Approved and Closed entries remain locked despite temporary exceptions. |
| `SEC-04` | Test and production traffic shall use HTTPS. Local passwords shall be stored as non-reversible hashes; authentication shall be rate-limited and account recovery shall not disclose existing credentials. Secrets, passwords, tokens, entry comments, and file contents shall not be copied into diagnostic logs. |
| `SEC-05` | Before rollout, the company shall agree on purpose, minimum fields, authorized access, retention, and deletion or pseudonymization for accounts, entries, export batches and any retained files, review reasons, audit events, logs, backups, and offline data. Requests for access or erasure shall follow that policy and applicable obligations; audit data shall not be retained indefinitely by default. |
| `SEC-06` | Time-entry and export audit events shall record only successful relevant changes, actor, and UTC time, without unnecessary full snapshots or copies of free text. Ordinary users shall not edit retained history; deleting an editable entry shall not silently erase history still required by the retention policy. |
| `SEC-07` | Offline storage shall contain only necessary data and remain separated by signed-in user. Sign-out shall warn of unsynchronized entries before explicit discard and clear local data on confirmed sign-out. The API shall recheck access at sync time after assignment or access changes. |
| `SEC-08` | Changes to category charging or work-item parentage shall not retroactively reclassify registered time. Work items shall not move between categories, and a category's charged department shall remain fixed once time is recorded under it. |
| `SEC-09` | The date policy and administrator-issued exceptions shall be enforced by the backend. Exceptions shall identify the employee, work-date range, allowed actions, and expiry, be audited, and never permit changes to Approved or Closed entries. Rejection after cutoff alone shall not unlock an entry. |

#### Integration

| ID | Requirement |
| --- | --- |
| `INT-01` | Validation, authentication, authorization, and other expected API failures shall use consistent structured responses that the frontend can process. User IDs supplied by the frontend shall not override the authenticated actor. |
| `INT-02` | Offline create requests shall carry stable operation identifiers so retries cannot create duplicate entries. The API shall return a saved, rejected, or conflict outcome for each operation; failures remain available for correction or retry. |
| `INT-03` | Creating an export shall persist its fixed set of approved entries. Re-downloading shall use that same selection and shall not close entries. Explicit completion shall close all of its entries atomically, with who/when recorded; retries shall not create duplicate batches or close entries twice. The API contract shall define creation, download, history, and completion. |
| `INT-04` | The backend shall schedule at most one cutoff reminder per active employee per weekly, monthly, or custom period on the final local day; daily mode shall not schedule one. A retry shall not generate duplicate reminders. |

#### Usability

| ID | Requirement |
| --- | --- |
| `USE-01` | Employee-facing screens shall be responsive and usable with touch input on supported phone viewports, with readable text and keyboard-accessible controls. The proposed accessibility target is WCAG 2.2 AA. |
| `USE-02` | Every in-scope screen shall provide clear loading, error, and empty states with an explanation or next action where appropriate. |
| `USE-03` | The client shall not show a create, edit, or finalization operation as server-saved until the backend confirms it. Offline queued, synchronizing, saved, rejected, and conflicted states shall be distinguishable with retry guidance. Export screens shall distinguish downloaded from completed batches and ask for explicit confirmation before completion. |
| `USE-04` | **Assigned to me** shall filter an already-authorized list without hiding general selectable work when turned off or exposing private siblings when showing a parent for context. |

#### Supportability

| ID | Requirement |
| --- | --- |
| `SUP-01` | The API and sync flow shall emit structured, privacy-limited operational logs with correlation IDs and health/error signals sufficient to diagnose failures without using the business audit timeline as a diagnostic log. |
| `SUP-02` | Automated tests shall cover department, category, assignment, and direct-URL access; review routing; period cutoffs including month changes, non-daily reminders, exceptions, and offline retry/conflicts; export history/re-download/atomic completion; audit visibility; migration of existing data; and key employee/manager browser flows. |
| `SUP-03` | Database migrations, backup/restore, and failed-sync support steps shall be documented and exercised in a representative test environment before deployment. |

#### Other

| ID | Requirement |
| --- | --- |
| `OTH-01` | Work dates and per-employee target completion dates shall be date-only values; period cutoffs shall use the configured company time zone. Audit timestamps shall be in UTC and display correctly in local time. Expected and actual durations shall use a consistent non-floating-point unit in API contracts and reports. |

### Should

#### Performance

| ID | Requirement |
| --- | --- |
| `PERF-03` | Filtered management overviews should meet an approved 95th-percentile response target under representative data and concurrency. A 3-second target is proposed for review. |

#### Integration

| ID | Requirement |
| --- | --- |
| `INT-05` | If additional notifications are delivered, assignment, rejection, and sync messages shall use the existing reminder delivery mechanism, correspond to actual events, and avoid misleading duplicates. A new notification channel is outside this scope. |

#### Usability

| ID | Requirement |
| --- | --- |
| `USE-05` | Desktop management views should support keyboard-accessible filters, preserve context when arriving from a related page, and retain usable mobile navigation. |

#### Supportability

| ID | Requirement |
| --- | --- |
| `SUP-04` | If additional notifications are delivered, tests and support documentation shall cover failed or duplicate notifications without copying personal data into logs. |

### Could

#### Security

| ID | Requirement |
| --- | --- |
| `SEC-10` | If Entra ID is delivered, the API shall validate token issuer, audience, and lifetime and link the Entra tenant and object IDs to the correct user. Disabling a sign-in method shall block that method and its continuing sessions without removing the last working administrator sign-in. |

#### Integration

| ID | Requirement |
| --- | --- |
| `INT-06` | If Entra ID is delivered, its configuration shall be optional per company environment, verified before enabling it, and linked to the same user, permissions, and audit identity as local sign-in. |

#### Usability

| ID | Requirement |
| --- | --- |
| `USE-06` | If multiple sign-in methods are delivered, the sign-in page shall offer only the methods enabled in that environment and explain unavailable options. |

#### Supportability

| ID | Requirement |
| --- | --- |
| `SUP-05` | If Entra ID is delivered, integration tests shall cover enabled sign-in modes, the same user's effective access, user deactivation, and removal of a method. |

## Open decisions

- Approve the performance profile, supported phones and browsers, accessibility
  standard, representative dataset, and concurrency.
- Confirm the one charged department per category assumption. Decide whether
  category visibility and reviewer assignments extend to subcategories, and
  whether assignments outside an employee's visible departments are allowed.
- Define supported category/work-item nesting, any required reviewer fallback,
  whether reviewers may approve their own entries, and which work-item types
  require assignment rather than general selection. Choose whether the top-ten
  report groups by categories or work items.
- Confirm time-entry transitions for edited Draft entries and whether task
  completion submits its entries. Define local account recovery and whether
  offline entry is create-only, including behavior after a session expires or
  access is revoked.
- Approve GDPR retention and deletion policies for accounts, audit data, review
  reasons, offline copies, logs, backups, and retained export files. Decide
  which changed values are necessary in a user-visible timeline.
- Agree on the time and channel for the required non-daily cutoff reminder, and
  how to preserve and retain an original export for re-download. Decide which
  columns and format the export contains.
- Additional Should notifications use the existing reminder channel; agree on
  its delivery method before implementation. The cost and constraints of Entra
  ID, the paired Could integration, are evaluated separately.

Materials, file attachments, configurable roles, external integrations, and
other larger ideas are tracked in the [future-work ideas](../docs/future-work-ideas.md).

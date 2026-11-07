# M3: PWA with Offline Time Registration and Work Management

Status: **Draft for review**

## Goal

Deliver an installable mobile-first PWA that can queue time registrations offline,
organize and delegate work by department and category, route approvals to the
right category reviewers, and maintain a traceable path from registration to
completed payroll or invoice exports. Local sign-in remains available.

The [M3 requirements](../Requirements/M3-ordreflow-requirements.md) contain the
numbered functional and non-functional requirements. **Must** defines milestone
completion; **Should** is planned when capacity permits; **Could** is stretch
work that may move to a later milestone. Requirements `FR-01`–`FR-15` retain
their earlier priority order; the new stories from `FR-16` are ordered by
business priority within their groups.

The [Danish M3 proposal (PDF)](M3-proposal.pdf) presents the scope and grouped
requirements for review, marking delivered M2 behavior separately from M3 work.

## Target dates

- Approximate start: 4 November 2026.
- Approximate completion: 2–4 December 2026. Documentation, diagrams, and
  reports follow the implementation period.

## Already available from the earlier implementation

Local sign-in, administrator user creation/deactivation, personal weekly time
entries and totals, correction of open or returned entries, and display of
review reasons already have application screens and API support. The app also
has manager-based review, order/task management, a one-off CSV download, and a
top-ten **orders** report. These are existing starting points, not proof that
the rewritten M3 category access, work assignment, lifecycle, or export batch
requirements are complete.

## Must: core scope

### Work structure and access

- Support local sign-in and explicit system administration separately from a
  user's displayed job title. Each company runs in its own environment.
- Users may belong to several departments; managers may manage several
  departments. Managers plan their employees' work but do not automatically
  review those employees' time registrations.
- Categories and subcategories are visible either to everyone signed in to the
  environment or to members of selected departments. Each category has one
  proposed cost-bearing department, regardless of who may select it. General
  work, such as **HR / Meetings**, is available as a reusable work item.
- Projects, tickets, and tasks sit under categories. Generally selectable work
  is accessible to its audience; assignment-only work is visible to assignees
  and explicitly authorized managers, category reviewers, or administrators.
  Employees can filter the permitted work list using **Assigned to me**.
- Department managers delegate work to eligible employees with **expected
  hours and a target completion date per employee**. A basic workload view
  compares allocated and registered hours. Task completion is reviewed
  separately from approval of time entries.
- Each time entry references a work item, through which its category and
  cost-bearing department are determined. Projects and other work items do not
  move between categories; a category's charging department cannot change once
  time is recorded under it. A manager of Plumbing can see workload totals for
  a plumber who attends an HR meeting, while an HR category reviewer handles
  the HR entry and its details.

### Registration, review, and audit

- Retain personal weekly entries and totals, online registration and correction,
  the existing top-ten hours report adapted to categories or work items, and
  export of approved entries.
- Install the PWA and allow employees to queue new time entries offline. Show
  queued, synchronizing, saved, and rejected/conflicted states; retry without
  creating duplicates. The server checks current work access and the entry
  period again at synchronization time.
- An employee saves a **Draft** and submits it as **Ready for approval**. A
  reviewer assigned to the entry's category can approve it or reject it with a
  reason. A rejected entry may be corrected and resubmitted while its period
  is open; after cutoff, correction requires an administrator-issued exception.
- **Approved** entries cannot be edited. **Closed** entries cannot be changed
  even with a temporary exception. An authorized viewer can see a time-entry
  timeline of relevant edits, review decisions, and state changes, including
  who acted and when; audit events are retained only under the agreed policy.

| Time-entry state | Meaning |
| --- | --- |
| Draft (`kladde`) | Saved, not yet submitted. |
| Ready for approval (`klar til godkendelse`) | Submitted for category review. |
| Approved (`godkendt`) | Approved by a category reviewer; locked. |
| Rejected (`afvist`) | Returned with a reason for correction and resubmission. |
| Closed (`afsluttet`) | An export containing the approved entry was explicitly completed; permanently locked. |

### Date windows and reminders

- An administrator chooses one company-wide **daily, weekly (Monday–Sunday),
  calendar-monthly, or custom monthly** period for employees to create, edit,
  or delete eligible entries by work date. For a custom monthly cutoff, select
  day **1–28**: a period starts the day after the previous cutoff and ends on
  the next cutoff, inclusive (for example, 29 October–28 November for the 28th).
- A period locks immediately after its last day in the company's time zone;
  there is **no grace period**. Weekly, monthly, and custom periods send a
  reminder to active employees on their last day to submit entries and check
  that offline registrations have synced. Daily mode sends no reminder.
- Administrators can grant time-limited, auditable exceptions for a named
  employee, permitted actions, and work dates after cutoff. An exception can
  allow creation or correction/deletion of unapproved entries; it never unlocks
  Approved or Closed entries. A queued offline entry arriving after cutoff is
  rejected with a clear reason unless a valid exception applies.

### Export batches and explicit completion

- An authorized administrator or payroll user creates an export batch of
  approved entries for a selected period. The batch records its original entry
  selection, creator, creation time, and status so it appears in export history.
- The user can inspect an export and download the **same original export** again.
  Re-downloading does not rerun the date filter or create a second batch.
- Creating or downloading an export does **not** close entries. After payroll or
  invoicing is finished, an authorized user explicitly completes the batch.
  Completion closes **all entries in that batch together**, records who and
  when, and is safe to retry without closing entries twice. An entry already
  included in an unfinished batch cannot be silently added to another.

### Quality baseline

- Enforce authorization, category visibility, time-entry locks, export-history
  access, and company-specific date policy in the API; frontend controls alone
  do not grant access. Keep operational logs separate from the business audit
  timeline and never log credentials, tokens, or full time-entry comments.
- Before rollout, agree on GDPR data minimization, authorized viewers,
  retention, and deletion/pseudonymization for entries, export history, audit
  events, offline copies, backups, and logs. Provide accessible mobile states,
  consistent structured errors, tests for key workflows, and documented
  backup/restore and deployment procedures. Performance thresholds in the
  requirements remain proposed until the test profile is agreed.

## Should: planned if capacity permits

- Configure richer management overviews with filters and related-page links;
  expand workload planning beyond the basic allocation view.
- Notify employees about assignments, rejections, and synchronization outcomes
  using the same delivery mechanism as the required cutoff reminder; improve
  desktop management navigation. A new notification channel is separate work.

## Could: stretch scope

- Add Microsoft Entra ID sign-in and an Owner setting to permit local login,
  Entra ID, or both in this company's environment. These two features move
  together if deferred. Neither is required for M3 completion; Entra is a
  deliberate large, optional integration outside the normal small-addition
  rule for Should and Could.

## Acceptance criteria for Must

1. A user signs in locally, sees only permitted general and assigned work, and
   can register time on an accessible work item. A manager can delegate the same
   work item to two employees with different planned hours and target dates.
2. An employee can queue time offline, see that it is not yet server-saved, and
   synchronize it once without duplicate entries. Revoked access or a cutoff
   produces an actionable failure instead of a false success.
3. A plumber may select HR / Meetings and register time charged to HR. An HR
   category reviewer can see, approve, or reject that entry; the Plumbing
   manager sees only permitted workload totals, not private HR details.
4. Draft, Ready, Approved, Rejected, and Closed rules are enforced by the API.
   Rejection after a period locks requires an administrator's valid exception
   for correction. Approved and Closed entries remain locked.
5. All four date-policy modes work at their configured boundaries, including
   month changes. Non-daily periods send one last-day reminder; daily periods
   do not. An expired period grants no employee edit/create/delete access.
6. A payroll or administrative user creates an export batch, views its history
   and entries, re-downloads the same selection, and completes the entire
   batch explicitly. Entries remain Approved until completion, then become
   Closed together; the operation is idempotent and audited.
7. The timeline and export history expose only authorized, necessary data,
   while company-approved retention, offline data handling, error states, and
   repeatable core-flow tests are in place.

## Decisions still to agree

- Confirm category reviewer and visibility inheritance into subcategories,
  allowed work-item nesting, reviewer self-approval, and fallback reviewers.
  The draft assumes one cost-bearing department per category.
- Define the notification channel and delivery time for the required cutoff
  reminder. Offline phones may not receive a remote reminder; the PWA must
  continue to show the state of any locally queued entries.
- Agree how the original export is preserved for re-download (a retained file
  or a minimal immutable snapshot), how long it is kept, and the supported
  formats and columns. Set device/browser and performance acceptance targets.
- Confirm the detailed transition rules for editing Draft entries and whether
  finishing a task submits its registrations; decide how offline registration
  behaves after a local session expires.

Configurable roles, materials, file attachments, new notification channels,
and other structural additions are tracked in
[future-work ideas](../docs/future-work-ideas.md) rather than as M3 Should or
Could requirements.

## Result

To be completed after the milestone.

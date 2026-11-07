# OrdreFlow Future-Work Ideas

Status: **Ideas for later planning**

These ideas are deliberately outside the [M3 milestone](../milestones/M3-PWA.md).
They need separate prioritization, requirements, and acceptance criteria before
they are scheduled. M3 Should/Could work is limited to additions built on the
Must foundation, except for the deliberately optional Entra ID integration.

| Idea | Why it needs separate planning |
| --- | --- |
| Owner-configurable roles and permissions | Requires persistent application roles, permission assignments, a management UI, and replacement of fixed authorization rules throughout the API and domain. Department management and category review remain explicitly scoped in M3. |
| Structured items and materials on time entries | Requires an item model, quantities, validation, access rules, and decisions about the company item catalogue and any external API integration. |
| Photo and document attachments | Requires file storage, upload/download authorization, limits, backups, GDPR data minimization, retention, and removal procedures. |
| Commonly used item packages and discounts | Depends on a defined item and pricing model before packages and discounts can be specified. |
| New notification channels | Push or external delivery would introduce additional integration, consent or device considerations, and delivery support. Small additional messages using M3's existing cutoff-reminder channel may still be Should work. |
| Multi-level approval | Requires a separate approval sequence and rules for who may act at each step. |
| Nextsign or other payroll signature integration | Requires an external contract, identity mapping, security review, and an agreed monthly approval process. |
| Employee scheduling and customer notifications | Requires scheduling and customer-contact models beyond per-assignee expected hours and target dates. |
| Clock-in/clock-out registration | Requires start/stop timing, timezone and offline conflict rules beyond the M3 manual-duration flow. |
| Transferring work between categories or cost-bearing departments | Requires explicit decisions about historic charging, review authority, assignments, and audit history. M3 keeps these relationships stable. |

Microsoft Entra ID sign-in and the Owner-controlled choice of login methods are
the **paired Could requirements in M3**, not items in this list. If unfinished,
they can be evaluated for a later milestone without blocking M3 completion.

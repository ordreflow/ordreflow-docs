# Task Planning and Backlog

This document describes how the OrdreFlow team creates, organizes, and tracks work across the frontend, backend, and documentation repositories.

## Source of Truth

- The organization-level GitHub Project is the central planning board.
- GitHub Issues contain the actual work descriptions.
- The repository containing an issue identifies where the work belongs.
- The docs repository contains project-wide process documentation.
- Project status and task progress should not be duplicated in Markdown.

## Where to Create Issues

Create a normal issue in the repository responsible for the work:

- Frontend work: `ordreflow-frontend`
- Backend work: `ordreflow-backend`
- Documentation work: `ordreflow-docs`

The preferred workflow is:

1. Open the relevant repository.
2. Create the issue from the repository's Issues tab.
3. Add the issue to the organization-level `OrdreFlow` Project.
4. Set the relevant Project fields.
5. Add the issue to a parent issue when it belongs to a larger outcome.

Draft issues in the Project can be used for ideas that do not yet have a clear repository or owner. They should be converted into repository issues before development starts.

## Minimum Issue Requirements

Every issue must contain or have:

- A clear goal
- A clear scope
- The relevant repository
- An assignee
- A milestone
- Relevant labels or an issue type

The goal and scope belong in the issue description. The repository, assignee, milestone, labels, and type should be set using GitHub's issue and Project fields where available.

## Recommended Issue Information

Add the following when they improve understanding of the work:

- Acceptance criteria
- Out-of-scope behavior
- Dependencies
- Related issues or pull requests
- Testing notes
- Design or implementation notes

These are recommendations, not mandatory fields for every small task.

`Priority` and `Estimate` are intentionally not used as issue requirements or Project fields. The current backlog order, milestone, and sprint planning determine what the team works on next.

## Issue Types and Labels

Use labels or issue types to describe the kind of work. Suggested values include:

- `frontend`
- `backend`
- `docs`
- `integration`
- `feature`
- `task`
- `bug`
- `spike`
- `documentation`

Use labels for meaningful categorization. Do not create a label for every implementation detail.

## Parent and Child Issues

Use a parent issue when one larger, coherent outcome requires multiple independently deliverable issues.

The parent issue should describe:

- The overall goal
- The overall scope
- What completion means
- The relevant milestone

Each child issue should describe one concrete piece of work and have its own assignee, repository, labels, and acceptance criteria where useful.

Example:

```text
Parent: M1: Implement frontend proof of concept
  Child: Scaffold Blazor WebAssembly POC frontend
  Child: Create frontend API client
  Child: Build mobile order selection
  Child: Build mobile time-registration form
  Child: Display saved entries and weekly total
  Child: Integrate and test frontend POC on a real phone
```

Do not create duplicate independent issues for the same parent outcome. Use a child issue instead.

Use dependencies when one issue must happen before another but the issues are not parent and child. For example, a frontend API client may be blocked by the backend API contract.

For work spanning multiple repositories, the parent can be placed in the repository that owns the overall outcome. Children can belong to the repositories where the implementation takes place. Link the related issues in both directions.

## Milestones

Milestones group work by a project outcome:

- `M0`: Inception and Foundation
- `M1`: Proof of Concept
- `M2`: Core Employee MVP

A milestone is not the same as a parent issue. A milestone may contain several unrelated issues, while a parent issue groups issues that together deliver one specific outcome.

## GitHub Project Fields

Use the built-in `Status` field to track workflow progress.

The initial Project fields should be limited to:

- `Status`
- `Milestone`

The Project may later use a custom `Iteration` field named `Sprint` for the weekly sprints. This field is optional until sprint planning is configured.

Do not add `Priority` or `Estimate` fields.

## Board Statuses

The Project can use these statuses:

- `Backlog`
- `Ready`
- `In Progress`
- `Code Review`
- `Testing`
- `Ready for Demo`
- `Done`
- `Blocked`

An issue should move to `Ready` only when the team understands the goal, scope, and expected result well enough to begin work.

## Development Workflow

1. Capture the work as an issue in the responsible repository.
2. Write the minimum required goal and scope.
3. Set the assignee, milestone, labels, and issue type.
4. Add the issue to the organization-level Project.
5. Set its status and sprint if a sprint field is configured.
6. Add parent, child, and dependency relationships when relevant.
7. Move the issue to `Ready` when it is understood.
8. Create a short-lived branch for the issue.
9. Link the pull request to the issue.
10. Review, test, and demonstrate the work when relevant.
11. Move the issue to `Done` and close it when the work is complete.

## Definition of Ready

An issue is ready for development when:

- The goal is understandable.
- The scope is clear.
- The correct repository is selected.
- An assignee is known.
- A milestone is selected.
- Relevant labels or an issue type are set.
- Important dependencies are visible.

## Definition of Done

An issue is done when:

- The described work is complete.
- Relevant tests or verification have been performed.
- The work has been reviewed where appropriate.
- Documentation has been updated where necessary.
- The result can be demonstrated when relevant.
- No critical defect remains.

## Backlog Maintenance

- Keep one shared backlog in the organization Project.
- Do not create separate personal backlogs.
- Keep vague ideas in the backlog until they are clarified.
- Break large outcomes into parent and child issues.
- Use checklists inside an issue for minor steps that do not need separate tracking.
- Keep the current backlog order meaningful because no priority field is used.
- Update issue descriptions when the agreed scope changes.

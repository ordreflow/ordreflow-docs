# Git Branching and Pull Requests

This document describes the branching and pull-request strategy for the OrdreFlow repositories.

The same strategy applies to:

- `ordreflow-frontend`
- `ordreflow-backend`
- `ordreflow-docs`

## Main Branch

`main` is the stable branch. It should contain work that is integrated, reviewed, and usable.

Rules for `main`:

- Do not push directly to `main`.
- Changes must be introduced through a pull request.
- Pull requests to `main` require at least one approval from another team member.
- Successful build and test checks should be required when CI is available.
- Force pushes to `main` are not allowed.
- Merged branches should be deleted.

There is no need for a separate `develop` branch for this project.

## Branch Names

Use short-lived branches with a type and a clear description:

```text
feature/<issue-number>-<short-description>
fix/<issue-number>-<short-description>
docs/<issue-number>-<short-description>
chore/<issue-number>-<short-description>
```

Examples:

```text
feature/14-time-registration-form
fix/19-invalid-duration-validation
docs/8-branching-strategy
chore/2-flox-environment
```

## Independent Issues

Use the normal workflow when an issue can be integrated without depending on incomplete work from another issue.

```text
main
  -> feature/<issue-number>-<description>
  -> pull request to main
```

The branch should start from the latest `main` and contain only the work for its issue.

## Parent and Child Issues

Use an integration branch when a parent issue contains several child issues that together form one incomplete feature or milestone.

The parent branch starts from `main`:

```text
main
  -> feature/m1-frontend-poc
```

Child branches start from the parent branch:

```text
feature/m1-frontend-poc
  -> feature/12-scaffold-frontend
  -> feature/13-api-client
  -> feature/14-time-registration-form
```

Workflow:

1. Create the parent branch from the latest `main`.
2. Create a child branch from the parent branch.
3. Implement the child issue.
4. Open a pull request from the child branch into the parent branch.
5. Review and merge the child pull request into the parent branch.
6. Integrate and test the complete parent feature.
7. Open a final pull request from the parent branch into `main`.
8. Get one approval and merge the parent pull request into `main`.

The parent branch may be incomplete while the child issues are being implemented. `main` remains stable until the complete parent feature is ready.

The parent branch is an integration branch, not a replacement for child branches. Direct commits to it should be limited to small integration fixes.

## When to Use Each Workflow

Use an independent branch when:

- The issue can be understood and tested on its own.
- It does not depend on incomplete sibling work.
- Integrating it into `main` will not make `main` confusing or unusable.

Use a parent integration branch when:

- Several child issues together form one incomplete feature.
- The individual child results are not useful until the complete flow exists.
- The team wants to test the complete feature before changing `main`.

For the M1 frontend POC, the parent integration branch is appropriate because the scaffold, API client, screens, and integration testing together form one usable flow.

## Pull Requests

Each pull request should:

- Link to the relevant issue.
- Explain what was changed.
- Include relevant testing information.
- Be reviewed by another team member.
- Stay focused on one issue or child issue.
- Be updated when the implementation or scope changes.

Child pull requests should target the parent integration branch. Independent pull requests should target `main`.

The final parent pull request should summarize the complete feature and verify the parent issue's acceptance criteria.

## Merge Strategy

The current recommended merge strategy is:

- Regular merge for child pull requests into the parent branch.
- Squash merge for the final parent pull request into `main`.

A squash merge combines the commits from a pull request into one commit on the target branch. The pull request still keeps its review and commit history on GitHub, while `main` receives one clear commit for the completed feature.

If a team member chooses a different merge method for a specific case, the reason should be recorded in the pull request.

## Cross-Repository Work

When a feature requires changes in more than one repository:

- Create an issue in each repository where implementation is required.
- Link the related issues.
- Use separate branches and pull requests in each repository.
- Use parent and child issues when the work forms one larger outcome.
- Merge changes in dependency order when necessary.

For example, a backend API issue and a frontend API-client issue should remain in their respective repositories while being connected through the same parent outcome or related issue links.

## Issue and Pull Request Lifecycle

1. Create the issue in the relevant repository.
2. Add it to the organization-level GitHub Project.
3. Set its milestone, assignee, labels, and issue type.
4. Decide whether it is independent or belongs to a parent integration branch.
5. Create the branch from the correct base branch.
6. Open a draft pull request early when feedback is useful.
7. Implement and test the issue.
8. Request review.
9. Merge into the parent branch or `main`, depending on the workflow.
10. Update the issue and Project status.
11. Delete the merged branch.

## Current M1 Example

```text
Parent issue: M1: Implement frontend proof of concept
Parent branch: feature/m1-frontend-poc

Child issue: Scaffold frontend
Child branch: feature/<issue-number>-scaffold-frontend
Pull request target: feature/m1-frontend-poc

Child issue: API client
Child branch: feature/<issue-number>-api-client
Pull request target: feature/m1-frontend-poc

Final pull request:
feature/m1-frontend-poc -> main
```

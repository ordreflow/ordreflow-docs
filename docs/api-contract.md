# OrdreFlow API Contract

Status: **Draft**

This document proposes the HTTP and JSON contract for the M1 proof of concept. It is the shared reference for the Blazor frontend and ASP.NET Core backend. It should be updated when the project agrees on a different behavior.

## Scope

The M1 contract supports the first end-to-end employee flow:

- List seeded orders or cases available to the test employee.
- Create a time registration.
- Retrieve saved registrations for a date range.
- Return the total registered duration for that date range.
- Return validation failures in a stable format.

The following are outside this contract:

- Full Microsoft Entra ID authentication.
- Administration and order management.
- Editing or deleting time entries.
- Reports and exports.
- Offline synchronization.
- Start/stop time registration.
- Pagination and advanced filtering.

## M1 Request Flow

The POC flow connects the phone-first frontend to the API and PostgreSQL database:

```mermaid
sequenceDiagram
    actor Employee
    participant Frontend as Blazor WebAssembly
    participant API as ASP.NET Core API
    participant Database as PostgreSQL

    Employee->>Frontend: Open time registration
    Frontend->>API: GET /api/orders
    API->>Database: Load available orders
    Database-->>API: Seeded order summaries
    API-->>Frontend: 200 OrderCollection

    Employee->>Frontend: Enter date, order, duration, and note
    Frontend->>API: POST /api/timeEntries
    API->>API: Validate request

    alt Invalid request
        API-->>Frontend: 400 ErrorCollection
    else Valid request
        API->>Database: Save time entry
        Database-->>API: Generated time-entry ID
        API-->>Frontend: 201 TimeEntry
    end

    Frontend->>API: GET /api/timeEntries?fromDate=...&toDate=...
    API->>Database: Load entries and total duration
    Database-->>API: Entries and weekly total
    API-->>Frontend: 200 TimeEntryCollection
    Frontend-->>Employee: Show saved entries and weekly total
```

## Conventions

| Convention | Draft decision |
| --- | --- |
| Base path | `/api` |
| HTTP format | JSON over HTTP(S) |
| JSON property names | camelCase |
| Multi-word URL segments | camelCase |
| Query parameter names | camelCase |
| Date values | `YYYY-MM-DD` date-only strings |
| Duration values | Integer minutes |
| Identifiers | Positive integers for the POC |
| Employee identity | Determined by the authenticated or development user context |

The frontend must not send an `employeeId` when creating or retrieving time entries. The backend owns the employee context and must apply authorization independently of the frontend.

## Resources

### Order summary

An order summary is the minimum information needed to select an order on a phone:

```json
{
  "id": 1,
  "number": "ORD-1001",
  "title": "Example order"
}
```

### Time entry

```json
{
  "id": 42,
  "orderId": 1,
  "orderNumber": "ORD-1001",
  "date": "2026-09-03",
  "durationMinutes": 150,
  "note": "Development work"
}
```

`orderNumber` is included so a saved entry can be displayed without requiring the frontend to resolve the order separately.

## Endpoints

### Sign in

```http
POST /api/auth/login
Content-Type: application/json
```

Local sign-in for `UC-06` (`FR-06`, `SEC-01`, `SEC-04`). This is the only endpoint, besides `GET /api/health`, that does not require a token.

Request body:

```json
{
  "email": "employee@example.com",
  "password": "The user's password"
}
```

Successful response:

```http
200 OK
Content-Type: application/json
```

```json
{
  "accessToken": "eyJhbGciOi...",
  "tokenType": "Bearer",
  "expiresAt": "2026-11-17T13:00:00Z"
}
```

The frontend sends the token as `Authorization: Bearer <accessToken>` on every other request. Signing out is done by the client discarding the token; the token stops working at `expiresAt`.

Failures:

| Status | Code | When |
| --- | --- | --- |
| `401` | `InvalidCredentials` | The email or password is wrong, the account doesn't exist, or the account is locked. The same answer is used for all three, so it does not reveal whether an account exists or is locked. |
| `403` | `UserInactive` | The email and password are correct, but the user has been deactivated. It is only returned after a correct password. |
| `429` | `TooManyLoginAttempts` | Too many sign-in attempts from the same client IP. The `Retry-After` header gives the number of seconds to wait. |

Security rules:

- Passwords are stored only as salted, non-reversible hashes (ASP.NET Core Identity).
- An account is locked for 15 minutes after 5 wrong passwords in a row. While locked, even the correct password returns `401 InvalidCredentials`.
- Sign-in is rate-limited per client IP: 20 attempts per minute by default, counting successful and failed attempts. It can be changed with the `LoginRateLimit__PermitLimit` and `LoginRateLimit__Window` environment variables. Colleagues behind one public IP share the limit, so it is set to slow down attacks rather than individual users.
- The client IP and scheme are read from `X-Forwarded-For` and `X-Forwarded-Proto`, because the API runs behind the frontend's nginx and, in test, Caddy.
- Test and production are only reachable over HTTPS (Caddy). Outside development the API sends `Strict-Transport-Security` on HTTPS responses.
- Passwords and tokens are never written to logs.
- There is no self-service password recovery yet, so no endpoint can reveal credentials or whether an account exists.

### List available orders

```http
GET /api/orders
```

The endpoint returns the seeded orders available to the current POC user. The response is an envelope so pagination or additional collection metadata can be added later without changing the top-level response type.

Response:

```http
200 OK
```

```json
{
  "items": [
    {
      "id": 1,
      "number": "ORD-1001",
      "title": "Example order"
    }
  ]
}
```

### Create a time entry

```http
POST /api/timeEntries
Content-Type: application/json
```

Request body:

```json
{
  "orderId": 1,
  "date": "2026-09-03",
  "durationMinutes": 150,
  "note": "Development work"
}
```

The request body is intentionally flat. The `requestBody` wrapper in the current backend prototype is an implementation detail and is not part of this contract.

Successful response:

```http
201 Created
Content-Type: application/json
```

```json
{
  "id": 42,
  "orderId": 1,
  "orderNumber": "ORD-1001",
  "date": "2026-09-03",
  "durationMinutes": 150,
  "note": "Development work"
}
```

The response must contain the database-generated `id` of the new time entry.

### Retrieve time entries

```http
GET /api/timeEntries?fromDate=2026-09-01&toDate=2026-09-07
```

`fromDate` and `toDate` are inclusive. The frontend should send both values for the selected week. The endpoint returns entries belonging to the current POC user and the total duration for the requested period.

Response:

```http
200 OK
```

```json
{
  "fromDate": "2026-09-01",
  "toDate": "2026-09-07",
  "items": [
    {
      "id": 42,
      "orderId": 1,
      "orderNumber": "ORD-1001",
      "date": "2026-09-03",
      "durationMinutes": 150,
      "note": "Development work"
    }
  ],
  "totalDurationMinutes": 150
}
```

The backend should return entries ordered by `date` descending, then `id` descending, unless the frontend has a specific display requirement.

## Validation

The backend is the source of truth for validation. The frontend may provide early feedback, but invalid requests must still be rejected by the API.

| Field | Draft rule |
| --- | --- |
| `orderId` | Required and greater than zero. The order must exist and be available to the current user. |
| `date` | Required and must be a valid date-only value. |
| `durationMinutes` | Required integer from `1` through `1440`. |
| `note` | Optional; maximum length is 1000 characters. |
| `fromDate` | Required for the retrieval endpoint and must be a valid date-only value. |
| `toDate` | Required, must be a valid date-only value, and must not be before `fromDate`. |

The POC does not define whether future dates are allowed. That business rule should be decided before the employee MVP is finalized.

## Errors

Validation and other expected operation failures use HTTP status codes and an array of error objects:

```http
400 Bad Request
Content-Type: application/json
```

```json
[
  {
    "code": "INVALID_DURATION",
    "message": "Duration must be between 1 and 1440 minutes.",
    "type": "Validation"
  }
]
```

Expected status codes for the POC are:

| Status | Meaning |
| --- | --- |
| `200` | Successful retrieval |
| `201` | Time entry created |
| `400` | Invalid request or validation failure |
| `401` | Missing, invalid or expired token, or failed sign-in (see [Sign in](#sign-in)) |
| `403` | Signed in, but not allowed to do this, or the user is deactivated |
| `404` | Requested order or resource does not exist |
| `429` | Too many sign-in attempts; wait for the `Retry-After` seconds |
| `500` | Unexpected server failure; implementation details must not be exposed |

## M2 Evolution

M2 is expected to add authenticated employee context, assigned orders or tasks, and editing of entries when allowed. Those changes should extend this contract rather than make the frontend provide employee identity or bypass backend authorization.

Possible future operations include:

```http
PUT /api/timeEntries/{id}
```

The exact update rules, locking behavior, and delete policy are not part of this draft.

## Accounts, System Access and Departments (UC-05)

These endpoints implement `UC-05` (`FR-05`, `FR-13`, `FR-14`, `SEC-01`) on the M3 `Employee` and `Department` model. All of them require a signed-in employee, and the backend checks every rule itself; hiding a button in the frontend is not a security boundary.

### Access model

| Concept | Meaning |
| --- | --- |
| System access | Grants on the employee: `Employee`, `DepartmentManager`, `CatagoryReviewer`, `Payroll`, `Administrator`. They are the only thing that grants access. Every account has `Employee`; without it the account cannot sign in. Grants are read on every request, so a change also applies to tokens that were already issued. |
| Title | Optional displayed text such as `Electrician`, at most 100 characters. It never grants system, department, or category access. |
| Department | A named group of employees. An employee can be a member of several departments. |
| Department manager | An employee linked as manager of a department. The link requires the `DepartmentManager` grant. If the grant is removed or the employee is deactivated, the department scope ends at once; the link is kept. |
| Managed employees | The active members of the departments the caller manages, excluding the caller. These are exactly the employees the manager may plan work for. |

Deactivating an employee keeps the account, department links and time entries. A deactivated employee cannot sign in (`403 UserInactive`) and is no longer part of any manager's managed employees. An administrator cannot deactivate themselves or remove their own `Administrator` or `Employee` access.

### Endpoints

| Method and path | Who | Body | Success |
| --- | --- | --- | --- |
| `GET /api/employees/me` | Employee | – | `200` own profile |
| `GET /api/employees` | Administrator | – | `200` `{ "items": [...] }` |
| `GET /api/employees/{id}` | Administrator | – | `200` |
| `POST /api/employees` | Administrator | `name`, `email`, `password`, optional `title`, optional `permissions` (for example `["DepartmentManager"]`) | `201` `{ "id": "..." }` |
| `PUT /api/employees/{id}` | Administrator | `name`, `title` (empty clears it) | `204` |
| `PUT /api/employees/{id}/permissions` | Administrator | `{ "permissions": ["Employee", "Payroll"] }`; replaces the grants | `204` |
| `POST /api/employees/{id}/deactivate` | Administrator | – | `204` |
| `POST /api/employees/{id}/activate` | Administrator | – | `204` |
| `GET /api/departments` | Employee | – | `200` `{ "items": [...] }` |
| `POST /api/departments` | Administrator | `{ "name": "Service" }` | `201` `{ "id": "..." }` |
| `PUT /api/departments/{id}/employees/{employeeId}` | Administrator | – | `204`; adding an existing member is harmless |
| `DELETE /api/departments/{id}/employees/{employeeId}` | Administrator | – | `204` |
| `PUT /api/departments/{id}/managers/{employeeId}` | Administrator | – | `204` |
| `DELETE /api/departments/{id}/managers/{employeeId}` | Administrator | – | `204` |
| `GET /api/departments/managed-employees` | DepartmentManager | – | `200` `{ "items": [...] }` |

An employee:

```json
{
  "id": "a41e…",
  "name": "Morgan Manager",
  "email": "morgan.manager@example.com",
  "title": "Project lead",
  "status": "Active",
  "createdAt": "2026-11-17T13:25:14Z",
  "permissions": ["DepartmentManager", "Employee"]
}
```

`GET /api/departments` returns every department to an administrator. Other employees receive only the departments they belong to or manage. Members are listed for administrators and for the department's managers; other members only see themselves:

```json
{
  "items": [
    {
      "id": "6c1f…",
      "name": "Service",
      "managedByMe": true,
      "managers": [
        { "employeeId": "a41e…", "name": "Morgan Manager", "email": "morgan.manager@example.com", "title": "Project lead", "status": "Active" }
      ],
      "members": [
        { "employeeId": "5e53…", "name": "Casey Employee", "email": "casey.employee@example.com", "title": "Electrician", "status": "Active" }
      ]
    }
  ]
}
```

`GET /api/departments/managed-employees` returns the same person objects as `members`.

### Errors

| Status | Code | When |
| --- | --- | --- |
| `400` | `EmailInvalid`, `EmailAlreadyInUse`, `PasswordRequired`, Identity password codes | The account details are invalid or the email is taken. |
| `400` | `InvalidSystemPermission` | A permission name is unknown. |
| `400` | `TitleTooLong` | The title is longer than 100 characters. |
| `400` | `DepartmentNameRequired`, `DepartmentNameTooLong`, `DepartmentNameTaken` | The department name is blank, longer than 200 characters, or in use. |
| `400` | `DepartmentManagerAccessRequired` | The employee to be made manager lacks the `DepartmentManager` grant. |
| `400` | `EmployeeInactive` | The employee to be placed in a department is deactivated. |
| `400` | `EmployeeAlreadyDeactivated`, `EmployeeAlreadyActive` | The status already has the requested value. |
| `403` | `SelfDeactivationForbidden`, `SelfAdministrationRemovalForbidden` | An administrator tries to lock themselves out. |
| `403` | – | The caller lacks the required grant. |
| `404` | `EmployeeNotFound`, `DepartmentNotFound`, `DepartmentMembershipNotFound`, `DepartmentManagementNotFound` | The employee or department does not exist, or the link does not exist. |

## Current Implementation Gaps

The current backend prototype differs from this draft in several areas:

- It exposes `POST /api/time_entries` instead of `/api/timeEntries`.
- Its request uses a `requestBody` wrapper.
- It uses `workItemId`, `hours`, `startTime`, `endTime`, and `comment`.
- It stores a `DateTime` and decimal hours rather than a date-only value and integer minutes.
- It does not yet expose the orders or time-entry retrieval endpoints.
- It currently returns `200 OK` after creation rather than `201 Created`.
- It does not copy the database-generated time-entry ID into the response command.

These differences are implementation work, not changes to the intended frontend contract.

## Open Decisions

- Whether the domain term should be `order`, `case`, or `workItem`.
- Whether the POC uses a development-only identity or a local authentication setup.
- Whether start/stop timing will be supported in addition to manual duration.
- Whether a common `ProblemDetails` error format should replace the POC error array.
- Whether order selection will later include tasks beneath an order.

# M1: Proof of Concept

## Goal

Prove that the Blazor WebAssembly frontend can communicate with the ASP.NET Core API, persist data through EF Core in PostgreSQL, and display the result on a phone.

## Scope

- Seeded test employee
- Seeded orders or cases
- Mobile time-registration form
- Date, order, duration, and optional note
- Basic validation
- Create time-entry API operation
- Retrieve time-entry API operation
- Display saved registrations
- Display a weekly total
- Real PostgreSQL persistence
- Basic Swagger/API testing
- Testing from a real phone

Out of scope:

- Full Microsoft Entra ID integration
- Offline synchronization
- Full administration
- Reports and export
- Configurable checklists
- Final hosting setup
- A decision between manual duration and start/stop timing beyond what is needed for the POC

## Done When

- The frontend calls the real backend API.
- A test user can create a time registration.
- The API validates the request.
- EF Core saves the registration in PostgreSQL.
- The registration remains after restarting the backend.
- The frontend can retrieve and display the registration.
- The weekly total is correct for the POC data.
- Invalid input is rejected.
- The flow works on a real phone.

## Result

To be completed after the milestone.

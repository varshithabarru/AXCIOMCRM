# AcxiomCRM

AcxiomCRM is an ASP.NET Core MVC CRM targeting .NET 10. It uses EF Core with SQLite and ASP.NET Core Identity for authentication and role-based access.

## Run locally

Install the .NET 10 SDK, then configure the initial administrator password through user secrets or an environment variable. No default password is shipped.

```powershell
cd AcxiomCRM
dotnet user-secrets init
dotnet user-secrets set "Identity:InitialAdmin:Password" "<strong unique password>"
dotnet run --urls http://127.0.0.1:5058
```

The initial administrator email defaults to `admin@acxiomcrm.local`; override it with `Identity:InitialAdmin:Email` if needed. Existing admin passwords are not reset at application startup. SQLite data is created locally and ignored by Git.

## Features

- Customer, Lead, Follow-Up, and Opportunity management with server-side validation and role-aware record visibility.
- ASP.NET Core Identity roles: Admin, Manager, and Sales Executive.
- Audit logging for authentication and successful CRM mutations.
- Dashboard KPIs for customers, leads, opportunities, and weighted pipeline value, plus lead-status, opportunity-pipeline, and monthly-sales charts.
- Authenticated JSON API with DTOs and validation:
  - `GET/POST /api/customers`, `GET/PUT/DELETE /api/customers/{id}`
  - `GET/POST /api/leads`, `GET/PUT/DELETE /api/leads/{id}`
  - `GET/POST /api/opportunities`, `GET/PUT/DELETE /api/opportunities/{id}`
  - `GET/POST /api/follow-ups`, `GET/PUT/DELETE /api/follow-ups/{id}`

API reads require authentication and are scoped for Sales Executives. API create, update, and delete operations are restricted to Admin and Manager roles. Unauthenticated API requests return HTTP 401; authenticated users lacking the required role receive HTTP 403.

## Build and test

```powershell
dotnet restore AcxiomCRM.slnx
dotnet build AcxiomCRM.slnx --no-restore --nologo
```

## Git hygiene

The root `.gitignore` excludes build output, local configuration secrets, SQLite databases and sidecar files, IDE state, and test output. Do not commit local database files or credentials.

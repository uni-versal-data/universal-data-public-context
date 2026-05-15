# Public Project Context

## Project name

Import Translation Manager

## One-line summary

A local-first data operations suite for reviewing incoming data files, maintaining reusable import mappings, and producing governed transformation and compliance outputs.

## Product purpose

The project helps operations teams turn changing source files into consistent destination import files. It provides workflows for defining sources and destinations, reviewing field metadata, building reusable mapping profiles, running transformations, managing picklists, and keeping an audit trail of configuration and run activity.

The codebase has grown from an Import Translation Manager module into a modular suite foundation with additional data preparation modules and shared platform administration.

## Target users

- Operational users who prepare and review import files.
- Data administrators who define source schemas, destination schemas, picklists, and mapping profiles.
- Tenant or system administrators who manage users, module access, field help, retention settings, and platform configuration.
- Developers extending the backend, frontend, transformation engine, or module workflows.

## Current modules

- Import Translation Manager: source and destination definitions, mapping profiles, transformation runs, run history, picklist handling, unmapped-value review, and lifecycle controls.
- CPC Import Builder: reusable compliance profiles, standalone CPC generation, integrated CPC generation during transformations, saved run history, and downloadable artifacts.
- Phone Number Cleaning: standalone phone-cleaning runs, country-aware normalization, alias handling, source defaults, saved outputs, and integration points with the import and CPC workflows.
- Shared platform administration: tenant, user, module subscription, entitlement, field-help, audit, job, retention, and integration foundations.
- Dynamics automation area: currently platform scaffolding and integration foundations rather than a complete automation product.

## Current technical stack

- Backend: Python, FastAPI, SQLAlchemy, Alembic, Pydantic settings, PyJWT, file parsing helpers, and a CLI worker process.
- Database: PostgreSQL-oriented schema with Alembic migrations; SQLite is used for lightweight local and test scenarios where appropriate.
- Frontend: React, TypeScript, Vite, React Router, MSAL browser libraries, and Testing Library/Vitest tests.
- Packaging and runtime: Dockerfiles for backend and frontend, Docker Compose for a demo-style stack, nginx for serving the built frontend and proxying API calls.
- Testing: pytest for backend tests, Vitest for frontend tests, and CI jobs for migrations, builds, frontend checks, backend tests, and Docker configuration.

## Repository structure summary

- `backend/`: FastAPI application, API routes, service layer, database models, Alembic migrations, transformation engine, CLI entry points, and backend tests.
- `frontend/`: React/Vite application, routed pages, shared components, auth helpers, styles, static callback files, and frontend tests.
- `docs/`: architecture notes, API notes, beta work-item notes, and this public context process.
- `samples/`: demo sample files used by local seeded scenarios.
- `.github/workflows/`: GitHub Actions workflows for CI, public-context validation, and safe public-context publishing.
- `docker-compose.yml`: local/demo orchestration for database, backend, worker, and frontend services.

## Local development summary

Backend development generally uses a Python virtual environment, editable installation with development dependencies, migrations, optional seed data, and a FastAPI development server. The backend exposes a CLI for migration, seeding, health checks, retention cleanup, and worker execution.

Frontend development generally uses npm dependencies and the Vite development server. The frontend development server proxies API calls to the backend service.

Docker Compose can run a full local/demo stack with database, backend, worker, and frontend services. Public context should describe this at a high level only; private paths, hostnames, tenant IDs, secrets, and deployment-specific values are deliberately excluded.

## Testing summary

- Backend tests live under `backend/tests/` and are run with `python -m pytest` from the backend project.
- Frontend tests live alongside the React source and are run with `npm test` from the frontend project.
- Frontend production build is run with `npm run build`.
- Migrations are validated through Alembic in CI.
- Docker Compose configuration and backend/frontend Docker image builds are checked in CI.
- Browser-level end-to-end coverage is limited and should be expanded for hosted or beta workflows.

## Deployment summary

The repository includes Docker-based packaging for a demo-style deployment with four main services: database, backend API, background worker, and static frontend. Startup runs database migrations and optional demo seeding before serving traffic. The backend and worker share file storage for uploads, outputs, and reports.

The current deployment posture is suitable for local evaluation and a controlled demo-style environment. Broader hosted deployment, horizontal scaling, production object storage, richer telemetry, and customer-managed identity onboarding remain areas to confirm or harden.

## Public AI context publishing

The private repository contains an automated GitHub Actions workflow that publishes only this public context file to a separate public context repository or branch. The publisher uses an explicit allowlist, copies only `docs/public-context.md`, generates a small public README, generates a static JSON endpoint and OpenAPI document from the copied context file, validates the exact generated file list, and fails before publishing if the output includes any unapproved files.

The intended public output contains only `README.md`, `public-context.md`, `api/public-context.json`, `openapi.json`, and optionally `.nojekyll`. The JSON endpoint exists so a custom GPT action can retrieve the reviewed Markdown context. Private source folders, workflow files, infrastructure files, compose files, samples, data, internal docs, migrations, auth configuration, secrets, and implementation details are deliberately excluded.

## Key architectural principles

- Destination-first modeling: destination import definitions are treated as the stable contract; source definitions and mapping profiles connect incoming files to that contract.
- Reusable configuration: sources, destinations, picklists, compliance profiles, and mappings are managed as reusable tenant-scoped records.
- Lifecycle governance: sources and mapping profiles use draft, testing, approved, and archived states, with backend enforcement rather than UI-only checks.
- Deterministic transformation: the transformation engine executes stored configuration and validation rules rather than page-specific logic.
- Traceability: run records, artifacts, issues, snapshots, audit logs, and platform jobs preserve operational evidence.
- Tenant-aware access: module subscriptions, groups, entitlements, users, and external identity mappings are part of the shared platform model.
- Data minimisation: source samples and operational artifacts should be retained only as needed, with personal data redacted or removed where possible.
- Performance containment: large mapping, profile, picklist, and field-review pages should keep heavy controls mounted only when needed and avoid rendering large arrays inline.

## Current implementation status

Implemented:

- Local suite foundation with Import Translation Manager, CPC Import Builder, Phone Number Cleaning, and shared platform administration.
- FastAPI routes and service-layer support for module workflows, platform operations, authentication, retention, run history, and downloads.
- SQLAlchemy models and Alembic migrations for the current application schema.
- Durable background processing for transformation, CPC, and phone-cleaning runs.
- React module shell, dashboard, admin screens, source/destination/mapping workflows, run detail screens, picklists, settings, CPC pages, and phone-cleaning pages.
- CI coverage for backend tests, migrations, frontend tests/build, and Docker checks.
- Secure public-context publishing for the reviewed AI-safe context file only.

Still to confirm or improve:

- Full hosted production deployment model.
- Production-grade object storage and observability.
- Browser-level end-to-end regression coverage.
- Horizontal scaling and multi-worker production behavior.
- Complete Dynamics, Dataverse, SharePoint, or KingswaySoft automation handoff.
- Customer-admin identity onboarding and group sync.

## Known constraints

- The Docker Compose stack is best understood as a local or controlled demo deployment rather than a horizontally scalable production topology.
- The background worker depends on shared storage access to uploaded files and generated artifacts.
- Local seeded authentication is suitable for development and demos only; hosted access should use an approved external identity flow.
- Some downstream automation areas are intentionally placeholders or future integration boundaries.
- Browser-level regression tests and real hosted smoke tests are limited.
- File uploads, reports, and exports may contain personal data during active processing, so retention and logging behavior must stay conservative.

## Common Codex instructions

- Inspect existing backend, frontend, migration, and test patterns before changing code.
- Preserve existing behavior unless the requested change explicitly changes it.
- Include focused backend tests for backend behavior changes.
- Include frontend tests when changing user-visible workflows or component behavior.
- Include migration considerations for database model or schema changes.
- Keep transformation, CPC, and phone-cleaning behavior deterministic and explainable.
- Consider frontend performance on large mapping, profile, picklist, and definition-review pages.
- Avoid hard-coding tenant-specific values, customer-specific labels, deployment paths, hostnames, or identity-provider identifiers.
- Avoid exposing personal data in logs, reports, exports, screenshots, test fixtures, or AI-shared context.
- Keep public documentation high-level and sanitize examples before copying them outside the private repository.

## Information deliberately excluded

This public context intentionally excludes:

- Secrets, credentials, signing keys, access tokens, private keys, and database passwords.
- Private hostnames, private IP addresses, private filesystem paths, tenant IDs, application IDs, and internal-only URLs.
- Real customer names, real user accounts, personal data, production data, and raw uploaded files.
- Demo account credentials and environment-specific authentication values.
- Proprietary business logic that is not needed to understand the public shape of the project.
- Internal deployment runbooks, commercial terms, support processes, and operational incident details.
- Source code and detailed implementation internals beyond high-level architectural context.

## Last reviewed date

2026-05-14

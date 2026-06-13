# Issues to Create — Stateless AWS Sweepstake

This file lists ready-to-create issues for converting the Sweepstake repository into a stateless AWS-hosted web application. Each entry contains a suggested title, description, acceptance criteria, labels, priority, and suggested estimate/milestone. Copy these into GitHub Issues or use them as-is.

---

## Frontend

### 1) Set up Vue 3 + TypeScript + Vuetify project
- Description: Initialize a new Vue 3 project using TypeScript and Vuetify 3. Configure project structure, linting, formatting (ESLint + Prettier), and a basic component layout for the sweepstake UI (landing page, login, create sweepstake, view entries).
- Acceptance criteria:
  - Vite or Vue CLI project scaffolded with TypeScript and Vuetify.
  - ESLint and Prettier configured and added to CI checks.
  - App builds successfully with `npm run build` and runs locally with `npm run dev`.
  - Basic routes and placeholder pages for Home, Login, Create, and Dashboard exist.
- Labels: frontend, enhancement, starter
- Priority: High
- Estimate: 3-5 pts

### 2) Integrate Cognito authentication in frontend
- Description: Add authentication flow using AWS Cognito (hosted UI or Cognito SDK). Implement sign-in, sign-up, sign-out flows and store JWT tokens securely (e.g., in memory / secure cookie). Protect routes that require authentication.
- Acceptance criteria:
  - Users can sign up and sign in using Cognito.
  - Auth state is available to app and protected routes redirect to login when unauthenticated.
  - JWT token is accessible for API calls.
- Labels: frontend, auth
- Priority: High
- Estimate: 3 pts

### 3) Static hosting config: S3 + CloudFront build/deploy
- Description: Add build scripts and deployment configuration to publish the frontend to S3 and invalidate CloudFront. Include a minimal GitHub Actions workflow that builds the site and deploys on push to main (or on release).
- Acceptance criteria:
  - Build artifact produced by `npm run build` is deployable to S3.
  - GitHub Actions workflow present that builds and uploads to S3 and invalidates CloudFront.
  - Instructions documented in README.
- Labels: infra, frontend, ci/cd
- Priority: Medium
- Estimate: 2-3 pts

### 4) Implement main UI: Create/Enter sweepstake flows
- Description: Build the key pages and components: create sweepstake form, join entry form, and results display. Integrate with backend API (stubbed endpoints if backend not ready).
- Acceptance criteria:
  - Forms validate input and show success/failure flows.
  - UI calls API endpoints for creating sweepstakes and entries (use mock or real endpoints).
  - Responsive layout and basic accessibility checks.
- Labels: frontend, feature
- Priority: Medium
- Estimate: 5 pts

---

## Backend

### 5) Bootstrap Java Lambda project
- Description: Create a Java project (Maven or Gradle) with a package layout for Lambda functions. Include example handler, dependencies, build profile, and local run/test instructions (e.g., using AWS SAM local or localstack).
- Acceptance criteria:
  - Java project scaffolded and builds (`mvn package` or `./gradlew build`).
  - Example Lambda function that returns a JSON health response.
  - README contains instructions for building and deploying the function.
- Labels: backend, java
- Priority: High
- Estimate: 3 pts

### 6) Implement API Gateway + Lambda integration for REST endpoints
- Description: Define REST endpoints for sweepstake operations (create sweepstake, list sweepstakes, join sweepstake, get results). Implement Lambda handlers for each operation (initially minimal logic) and map via API Gateway (or HTTP API).
- Acceptance criteria:
  - Endpoints exist and invoke corresponding Lambda handlers.
  - Basic end-to-end test demonstrating request flows.
  - OpenAPI/Swagger or endpoint documentation included.
- Labels: backend, api
- Priority: High
- Estimate: 5-8 pts

### 7) Add Cognito authorizer to API/Lambdas
- Description: Configure API Gateway to use Cognito authorizer and validate JWTs in Lambda where needed. Ensure roles and permissions are minimal and least-privilege.
- Acceptance criteria:
  - API Gateway protected by Cognito authorizer for routes requiring auth.
  - Lambda validates token claims or relies on API Gateway for authorization.
  - Documentation of required client claims/roles.
- Labels: backend, auth, security
- Priority: High
- Estimate: 3 pts

---

## Infrastructure & DevOps

### 8) Choose and implement IaC for the project (SAM / CloudFormation / Terraform)
- Description: Provide Infrastructure-as-Code to define S3, CloudFront, Cognito, API Gateway, Lambda, roles, and (optional) database resources. Include separate stacks/environments for dev/staging/prod.
- Acceptance criteria:
  - IaC repository or directory with templates exists.
  - Deploy command documented and deploys core resources in dev environment.
  - Environments are parameterized (e.g., via variables or parameter store).
- Labels: infra, iac
- Priority: High
- Estimate: 8-13 pts

### 9) Setup GitHub Actions CI/CD for backend and IaC
- Description: Create GitHub Actions workflows to build/test Java Lambdas, run static checks, and deploy IaC and Lambdas to dev environment on push to main or via manual workflow_dispatch.
- Acceptance criteria:
  - Actions present for build/test for Java and frontend build.
  - Deploy workflow deploys IaC and uploads Lambda artifacts.
  - Secrets and environment variables documented for GitHub repository.
- Labels: ci/cd, infra
- Priority: Medium
- Estimate: 5 pts

### 10) Configure environments and secrets (dev/staging/prod)
- Description: Define environment configuration and store secrets in GitHub Actions or AWS Secrets Manager/SSM parameter store. Provide a process for promoting from dev to staging/prod.
- Acceptance criteria:
  - Documentation for required secrets and environment variables.
  - Example env files or parameter mappings for each environment.
- Labels: infra, ops
- Priority: Medium
- Estimate: 2-3 pts

---

## Data & Persistence

### 11) Design data model and choose storage
- Description: Decide on data storage (DynamoDB, RDS, etc.) and document schema for sweepstakes, entries, users (if needed). Provide migration plan and access patterns.
- Acceptance criteria:
  - Documented schema and chosen datastore with reasoning.
  - CRUD access patterns and required indexes described.
- Labels: design, database
- Priority: Medium
- Estimate: 3 pts

### 12) Implement persistence layer for sweepstakes and entries
- Description: Implement database access code in Java Lambdas for creating/listing sweepstakes and entries, including tests and error handling.
- Acceptance criteria:
  - Lambdas can persist and read data from chosen storage.
  - Unit tests for persistence logic.
- Labels: backend, database
- Priority: Medium
- Estimate: 5 pts

---

## Quality & Process

### 13) Add project documentation and setup guide
- Description: Improve README with development setup steps, architecture summary (refer to `.github/ISSUE_TEMPLATE/architecture.md`), and deployment steps.
- Acceptance criteria:
  - README includes setup, build, deploy, and testing instructions.
  - Links to architecture file and issue tracking are present.
- Labels: docs
- Priority: High
- Estimate: 1-2 pts

### 14) Testing strategy and initial tests (unit & integration)
- Description: Define a testing strategy and add initial unit and integration tests for frontend components and backend Lambda logic. Include guidance on local test runs and CI integration.
- Acceptance criteria:
  - Unit tests for core backend and frontend components.
  - CI runs tests and reports results.
- Labels: tests, ci/cd
- Priority: Medium
- Estimate: 5 pts

### 15) End-to-end test plan (optional)
- Description: Define e2e tests (Cypress or Playwright) to cover user flows: sign-up/login, create sweepstake, join, and view results. Add initial tests and CI job.
- Acceptance criteria:
  - Basic e2e tests added and run in CI for the dev environment.
- Labels: tests, e2e
- Priority: Low
- Estimate: 5-8 pts

---

## Suggested milestones and labels
- Milestones: v0.1-init, v0.2-core-api, v0.3-ci-deploy
- Suggested labels: frontend, backend, infra, auth, ci/cd, docs, tests, high-priority, medium-priority, low-priority

---

If you want I can:
- Break these into smaller issues (I already included estimates) and create separate files per area.
- Generate gh CLI or curl commands you can run to create issues automatically.
- Create GitHub issue templates for recurring issue types.

Tell me if you want any edits (rename, reorder, add more detail) or want the file placed in a different path.
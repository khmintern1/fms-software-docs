# System Requirements

## 1. Purpose

This document describes the functional scope and quality requirements for the
Fuel Management System (FMS). It is based on the backend API routes and the
application configuration currently present in this workspace. Feature
availability can differ between local-station and cloud deployments.

## 2. Users and Access

The system is intended for station staff and administrators. The exact role
names and permissions are configurable; the application provides user, role,
and permission management.

## 3. Functional Requirements

### 3.1 Authentication and access control

- **FR-01:** The system shall allow a user to sign in and sign out.
- **FR-02:** The system shall provide authenticated API access for local and
  cloud application functions.
- **FR-03:** Authorized administrators shall be able to create, view, update,
  and delete user accounts.
- **FR-04:** Authorized administrators shall be able to manage roles and
  permissions and assign or revoke them for users and roles.
- **FR-05:** The system shall support card-based authentication where the
  deployment enables it.

### 3.2 Station and fuel configuration

- **FR-06:** Users with suitable access shall be able to manage stations and
  shops.
- **FR-07:** Users with suitable access shall be able to manage tanks, fuel
  types, stock prices, dispensers, and nozzles.
- **FR-08:** The system shall record fuel delivery (fuel-in) information.
- **FR-09:** The system shall support ATG-related fuel-in data operations where
  an ATG integration is configured.
- **FR-10:** The system shall record fuel price changes.

### 3.3 Sales and daily operations

- **FR-11:** The system shall allow authorized users or connected workflows to
  create, view, update, and delete sales records according to the deployment's
  access rules.
- **FR-12:** The system shall support preset sales and cashier approval
  workflows in the local deployment.
- **FR-13:** The system shall record station sales and credit sales.
- **FR-14:** The system shall support customer credit updates for one or more
  credit-sale records.
- **FR-15:** The system shall allow users to start and end a daily operating
  session in the local deployment.
- **FR-16:** The system shall retain sales history and related history details.

### 3.4 Cloud customer and business management

- **FR-17:** The cloud deployment shall provide management for companies,
  customers, members, payments, discounts, vehicle types, and customer credits.
- **FR-18:** The cloud deployment shall support customer lookup by car number
  and retrieval of top customers by amount.
- **FR-19:** The cloud deployment shall support shop-level station sales
  summaries.

### 3.5 Dashboards and reports

- **FR-20:** The local deployment shall provide a dashboard for today's sales.
- **FR-21:** The cloud deployment shall provide an information dashboard.
- **FR-22:** The system shall provide operational reports for the data exposed
  by each deployment. These include sales, sale details, fuel deliveries,
  tanks, pumps, weekly summaries, and totalizer differences where available.
- **FR-23:** The local deployment shall provide daily sales, sales summaries,
  daily lists, and sale-ledger reports.

### 3.6 Device and tank integrations

- **FR-24:** When configured, the system shall exchange real-time messages with
  station devices through an MQTT broker.
- **FR-25:** When configured, the system shall obtain tank measurement data
  through an ATG service.
- **FR-26:** The system shall make integration settings configurable for the
  target deployment.

## 4. Non-functional Requirements

The following requirements describe expected system qualities. Values such as
response-time targets, retention periods, and availability objectives must be
set by the deploying organization; they are not defined in the current
repository.

- **NFR-01 Security:** Protected local and cloud API routes shall require
  authentication. Access to administrative actions shall follow configured
  roles and permissions.
- **NFR-02 Data integrity:** The system shall preserve consistent links among
  stations, tanks, dispensers, nozzles, sales, and related records when data is
  created or updated.
- **NFR-03 Deployment configuration:** Environment-specific secrets, URLs,
  database settings, cache settings, and integration endpoints shall be
  supplied through deployment configuration rather than hard-coded into the
  application.
- **NFR-04 Maintainability:** Backend and frontend components shall remain
  separately deployable and maintainable through their respective repositories.
- **NFR-05 Observability:** Deployments shall expose application and service
  logs sufficient for operators to diagnose API, queue, and integration issues.
- **NFR-06 Backup and recovery:** Production operators shall maintain and
  periodically verify backups for application data and required configuration.
- **NFR-07 Compatibility:** The backend requires PHP 8.2 or later. The frontend
  is built with Next.js 15 and React 19. Deployment environments shall provide
  compatible runtimes and services.
- **NFR-08 Performance and availability:** Deployment owners shall define and
  monitor service-level targets appropriate to the station and cloud use cases.

## 5. External Dependencies

Depending on deployment, FMS uses:

- MySQL for application data
- Redis and Laravel Horizon for cache and queue processing
- An MQTT broker and compatible station devices for messaging
- An ATG endpoint for tank measurements
- Docker Compose for the documented container deployments

Not every deployment necessarily enables every integration.

## 6. Out of Scope and Open Decisions

The repository does not establish universal numeric targets for throughput,
response time, uptime, data retention, or recovery time. These should be agreed
for each deployment. Hardware models, supported payment providers, and exact
business approval rules also depend on station configuration and should be
specified separately where needed.

## 7. Source of Requirements

This document summarizes the current implementation surface; it is not a
formal acceptance test plan. Refer to `fuel-management/routes/` for API route
definitions, `fuel-management/README.md` for deployment details, and
`fuel-management-frontend-nextjs/` for the web application implementation.

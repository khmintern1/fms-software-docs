# System Architecture

## 1. Architecture Overview

The Fuel Management System (FMS) is organized into three primary layers: a web frontend, a backend API, and supporting data and integration services.

The frontend and backend are maintained in separate repositories, allowing them to be developed and deployed independently. The backend exposes separate local and cloud API route groups, while deployment configurations determine which services and integrations are available.

The system supports two primary operating modes:

- **Local Mode:** Runs at a fuel station and supports station-level operations and communication with station equipment.
- **Cloud Mode:** Provides centralized management, business operations, and reporting across multiple stations.

## 2. Core Architecture and System Data Flow

The following diagram illustrates the local fuel station architecture.

![Local Fuel Station Architecture Diagram](<images/Local Fuel Station Architecture Diagram.png>)

<label style="text-align: center;"><span style="font-weight:bold;">**Figure 1.**</span> Fuel Station Architecture Diagram</label>                        

The system follows a client-server architecture. The frontend communicates with the Laravel backend through HTTP API requests. The backend processes business logic, interacts with the database and supporting services, and returns responses to the frontend.

MQTT and ATG integrations support device messaging and tank monitoring where configured.

## 3. System Components

### 3.1 Frontend

**Technology Stack:** Next.js 15, React 19, TypeScript, and Axios.

**Responsibilities:**

- Provide the user interface for local and cloud workflows.
- Handle user interactions and validate form inputs where implemented.
- Communicate with the backend API using the configured API URL.
- Connect to the configured MQTT WebSocket endpoint for real-time features where enabled.
- Display application data, transaction results, and operational reports.

### 3.2 Backend

**Technology Stack:** PHP 8.2+, Laravel 11, Laravel Sanctum, and Laravel Horizon.

**Responsibilities:**

- Process application requests and execute business logic.
- Validate incoming requests and return JSON responses.
- Authenticate API users and manage user accounts, roles, and permissions.
- Read and update application records in MySQL.
- Use Redis for configured caching and queue services.
- Integrate with MQTT and ATG services where enabled.
- Execute background jobs and scheduled processes according to the deployment configuration.

### 3.3 Database

**Technology:** MySQL in the documented Docker deployments.

**Responsibilities:**

- Store persistent application and operational data.
- Maintain relationships among stations, shops, tanks, dispensers, nozzles, sales, fuel deliveries, customers, and transaction histories.
- Enforce database constraints defined in Laravel migrations.
- Support data retrieval and updates performed by the backend application.

Laravel database connections are configured through environment settings, allowing deployments to use the appropriate database configuration.

### 3.4 Supporting Services

The system uses additional services to support communication, background processing, and deployment operations.

| Service | Responsibility |
|---|---|
| Redis | Provides configured cache and queue services. |
| MQTT / Mosquitto | Supports messaging between the application and connected devices. |
| ATG Service | Provides tank-related data through a configured endpoint. |
| Docker Compose | Defines and runs application services in local and production environments. |
| Supervisor | Manages configured Laravel application and background processes. |
| Caddy | Provides local networking and TLS functionality in the documented station setup. |

The availability of these services depends on the selected deployment mode and its configuration.

## 4. Module Architecture

The application is organized into functional modules that support station operations and centralized business management.

| Module | Responsibility |
|---|---|
| Authentication and Access Control | Login, card-authentication routes, Sanctum-protected API access, user accounts, roles, and permissions. |
| Station Setup | Manage stations, shops, and station-specific configuration. |
| Fuel and Equipment | Manage tanks, fuel types, stock prices, dispensers, and nozzles. |
| Sales and Daily Operations | Manage sales, credit sales, station sales, price changes, and daily sessions. |
| Fuel Delivery and History | Record fuel deliveries, ATG-related data, histories, and history details. |
| Customer and Business Records | Support cloud business features involving companies, customers, members, payments, discounts, vehicle types, and customer credits. |
| Dashboard and Reporting | Present sales summaries and local or cloud operational reports. |
| Device Integration | Exchange messages through MQTT and retrieve tank data through configured ATG services. |

The modules interact with the backend API and database according to their responsibilities and the features enabled for each deployment.

## 5. System Data Flow

A typical application request follows these steps:

1. **User Interaction:** A user performs an action in the Next.js frontend.
2. **API Request:** The frontend sends an HTTP request containing JSON data to the configured Laravel API URL.
3. **Authentication and Validation:** The backend authenticates requests to protected routes and validates incoming data.
4. **Business Logic Processing:** The backend executes the relevant application logic and reads or updates MySQL records.
5. **Supporting Service Integration:** The application uses Redis for configured cache or queue operations and communicates with MQTT or ATG services when required.
6. **API Response:** The backend returns a JSON response with an appropriate HTTP status code.
7. **User Interface Update:** The frontend processes the response and displays the result to the user.

The exact request flow depends on the endpoint, authentication requirements, and the services involved.

## 6. API Communication

### 6.1 API Design

The backend provides REST-like resource endpoints, including Laravel API resource routes and operation-specific endpoints.

| Attribute | Description |
|---|---|
| API Style | REST-like resource endpoints and operation-specific routes. |
| Data Format | JSON. |
| HTTP Methods | Determined by the individual API endpoint. |
| Authentication | Laravel Sanctum for protected API routes. |
| Frontend Session | NextAuth is also used for the frontend web session. |
| Error Handling | JSON responses and Laravel framework exception responses, depending on the endpoint and error type. |
| Transport Security | HTTP or HTTPS, depending on the deployment configuration. |

### 6.2 Authentication and Authorization

Laravel Sanctum protects the configured local and cloud API route groups. Login, card-authentication, and logout routes are defined in the backend authentication routes.

The frontend also uses NextAuth for its web session. The interaction between frontend session authentication and backend API authentication depends on the implemented configuration.

### 6.3 Request and Response Handling

The frontend communicates with the backend using the configured API URL. Requests and responses use JSON where supported by the endpoint.

Shared backend response helpers return JSON containing `message` and `data` fields, along with an HTTP status code. Laravel validation and authentication failures may use framework-generated exception responses, so the exact response structure can vary.

### 6.4 Transport Security

The application supports HTTP or HTTPS according to the deployment configuration.

The documented local station setup configures Caddy for TLS. Production HTTPS depends on the production host, reverse proxy, certificates, and related network configuration.

## 7. Database Architecture

The database schema is maintained through Laravel migrations. MySQL stores persistent application and operational records, while foreign keys and other constraints are defined in the relevant migration files.

### 7.1 Main Entity Groups

| Entity Group | Representative Entities |
|---|---|
| Station Configuration | Stations, shops, tanks, fuel types, stock prices, dispensers, and nozzles. |
| Operational Records | Sales, station sales, fuel deliveries, histories, history details, and daily session data. |
| Cloud Business Records | Companies, customers, members, payments, discounts, vehicle types, customer credits, and credit sales. |
| Access Control | Users, personal access tokens, roles, and permissions. |

### 7.2 Data Relationships

The database maintains relationships among station configuration, equipment, operational transactions, business records, and access-control entities.

The specific relationships, foreign keys, indexes, and constraints are defined in the individual Laravel migration files.

The detailed table structures and entity relationships should be documented in the Database Design section. This architecture overview does not replace the complete database reference.

## 8. Security Architecture

Security controls are implemented across authentication, authorization, request validation, configuration management, and network communication.

### 8.1 Authentication

- Login, card-authentication, and logout routes are implemented in the backend.
- Laravel Sanctum middleware protects the configured local and cloud API route groups.
- Personal access tokens are represented in the application's access-control data model.

### 8.2 Authorization

The backend includes role and permission management.

Seeded roles include `admin`, `installer`, `manager`, `cashier`, `pprd`, and `kyawsan`. Permissions can be managed through the API.

Actual access to individual operations depends on the implemented authorization rules and route configuration.

### 8.3 Input Validation

- Laravel request validation is used in authentication and applicable application request flows.
- Frontend forms provide client-side validation where implemented.
- Backend validation remains responsible for validating requests received by the API.

### 8.4 Configuration and Data Protection

Database, Redis, API, MQTT, and ATG configuration values are supplied through environment configuration where applicable.

Production deployments must protect credentials and other secrets, restrict access to internal services, and avoid exposing sensitive configuration values in source control or client-side code.

### 8.5 Secure Communication

HTTPS/TLS availability depends on deployment configuration.

The documented local station setup uses Caddy for internal TLS. Production deployments must configure TLS through the production host or reverse proxy and apply appropriate network-access restrictions.

## 9. Deployment Architecture

### 9.1 Local Station Deployment

The backend repository's `docker-compose-local.yml` defines the following services:

- Backend application
- MySQL database
- Redis
- Mosquitto MQTT broker

The frontend runs as a separate Next.js application and is configured to use the local API URL and, where required, the MQTT WebSocket endpoint.

The backend repository's installer and networking configuration can set up Caddy and station services.

This deployment supports station-level operations and integrations with locally configured equipment and services.

### 9.2 Cloud / Production Deployment

The backend repository's `docker-compose-prod.yml` defines the backend and Redis containers.

MySQL and MQTT are expected to be provided by separately configured services or infrastructure. The frontend is deployed separately and points to the configured cloud API URL.

The actual infrastructure may vary according to the production environment.

### 9.3 Runtime Operations

Docker Compose and Supervisor are used to manage the backend deployment and processes.

The provided Supervisor configurations start the Laravel application, Horizon, and scheduled or MQTT processes according to the local or production configuration.

The backend Makefile documents operational commands for health checks, logs, backups, and restoration.

Actual proxy configuration, database hosting, monitoring, backup schedules, and recovery procedures depend on the deployment environment.

### 9.4 Environment Configuration

Environment variables configure application URLs, database credentials, Redis, MQTT, and ATG endpoints.

Each deployment must supply appropriate values for its environment and protect credentials and other sensitive configuration data.

## 10. Architecture Decisions

The following decisions describe the documented architecture and their associated trade-offs.

| Decision | Reason and Trade-off |
|---|---|
| Separate frontend and backend repositories | Allows independent development and deployment of the Next.js interface and Laravel API, but requires coordinated API URL and version configuration. |
| Local and cloud API route groups | Supports station-local workflows and centralized cloud workflows, but available features may differ between route groups. |
| MySQL for persistent data and Redis for cache and queues | Separates durable business records from cache and queue workloads, but configured background processing depends on Redis availability. |
| MQTT for device messaging | Supports station-device messaging and real-time frontend updates, but requires broker access, credentials, and network configuration. |
| ATG integration through a configurable endpoint | Allows integration with deployment-specific tank monitoring services, but availability and data formats depend on the external service. |
| Docker Compose deployment configurations | Provides repeatable service setup for local and production environments, but production database and MQTT services may require separate infrastructure. |
<!-- 
## 11. Related Documentation

- [Project Overview](overview.md)
- [System Requirements](requirements.md)
- [Technology Stack](techstack.md)
- [Database Design](database.md)
- [API Documentation](api.md)
- [Authentication and Authorization](authentication.md)
- Backend setup and deployment: `fuel-management/README.md`
- Backend database migrations: `fuel-management/database/migrations/`
- Local networking: `fuel-management/NETWORK.md`
- MQTT documentation: `fuel-management/MQTT.md`
- Frontend setup: `fuel-management-frontend-nextjs/README.md`
 -->

# Fuel Management System (Sixth Kendra)
## Technology Stack Documentation

## 1. Overview

The **Fuel Management System (Sixth Kendra)** is built using a modern web technology stack that separates the frontend user interface from backend application services.

The frontend is developed with **Next.js, React, and TypeScript**, while the backend uses **Laravel and PHP** to handle business logic, API endpoints, authentication, authorization, and server-side operations.

The system also integrates supporting libraries for UI components, data fetching, caching, API communication, real-time messaging, queue management, and automated testing.

### Architecture Overview

```text
┌──────────────────────────────────────┐
│             Frontend                 │
│                                      │
│  Next.js 15 · React 19 · TypeScript  │
│  Tailwind CSS · Radix UI             │
│  TanStack Query · Axios              │
└──────────────────┬───────────────────┘
                   │
                   │ HTTP / REST API
                   ▼
┌──────────────────────────────────────┐
│              Backend                 │
│                                      │
│       Laravel 11 · PHP 8.2+          │
│  Sanctum · Spatie Permission         │
│  Scramble · Laravel Horizon          │
└──────────────────┬───────────────────┘
                   │
                   ▼
┌──────────────────────────────────────┐
│       Supporting Services            │
│                                      │
│  Database · Redis / Queues · MQTT    │
└──────────────────────────────────────┘
```

*Note: This diagram illustrates the high-level architecture and potential integrations. The actual database, Redis, MQTT broker, and external service configuration should be verified against the application source code.*

---

## 2. Technology Stack Summary

### 2.1 Core Technologies

| Category | Technology | Declared Version |
|---|---|---|
| Frontend Framework | Next.js | 15.1.6 |
| Frontend Library | React | `^19.0.0` |
| Programming Language | TypeScript | `^5` |
| Backend Framework | Laravel | `^11.31` |
| Backend Language | PHP | `^8.2` |
| CSS Framework | Tailwind CSS | `^3.4.1` |
| CSS Processing | PostCSS | `^8` |
| HTTP Client | Axios | `^1.7.9` |
| Data Fetching & Caching | TanStack Query | `^5.65.1` |
| Table Management | TanStack Table | `^8.20.6` |
| Authentication | Laravel Sanctum | `^4.0` |
| API Documentation | Dedoc Scramble | `^0.12.13` |
| Real-Time Messaging | MQTT | `5.9.1` |
| Role & Permission Management | Spatie Laravel Permission | `^6.15` |
| Queue Management | Laravel Horizon | `^5.33` |
| Testing Framework | Pest PHP | `^3.7` |

> **Versioning note:** The versions above represent declared dependency versions or version constraints from `package.json` and `composer.json`. The exact installed versions should be verified using the corresponding lock files and package manager commands.

---

## 3. Frontend Technology

The frontend provides the application's user interface, interactive components, data presentation, and communication with backend APIs.

### 3.1 Core Frameworks

| Technology | Version | Purpose |
|---|---|---|
| Next.js | `15.1.6` | Application framework, routing, rendering, and production builds |
| React | `^19.0.0` | Reusable UI components and interactive interfaces |
| TypeScript | `^5` | Static typing and improved maintainability |
| Tailwind CSS | `^3.4.1` | Utility-first styling and responsive layouts |
| PostCSS | `^8` | CSS transformation and build processing |

### 3.2 UI Components and Styling

The project uses Radix UI packages and supporting utilities to build consistent, accessible user interfaces.

| Library | Purpose |
|---|---|
| Radix UI | Accessible primitives for dialogs, menus, tabs, tooltips, and other UI components |
| Lucide React | Icons |
| Class Variance Authority (CVA) | Component style variants |
| `clsx` | Conditional CSS class names |
| `tailwind-merge` | Resolving conflicting Tailwind CSS classes |
| Sonner | Toast notifications |
| `next-themes` | Theme management |
| GSAP | Animations |

### 3.3 Data Management and Utilities

| Library | Purpose |
|---|---|
| TanStack Query | Server-state fetching, caching, and synchronization |
| TanStack Table | Table state management and data presentation |
| Axios | HTTP requests to backend APIs |
| Zod | Schema validation |
| `date-fns` | Date manipulation and formatting |
| Recharts | Charts and data visualization |
| React Day Picker | Date selection interfaces |
| `mathjs` | Mathematical calculations and expression evaluation |

### 3.4 Authentication and Localization

| Library | Purpose |
|---|---|
| NextAuth.js | Frontend authentication functionality |
| `next-intl` | Internationalization and localized content |

**Implementation note:** The actual authentication flow, supported languages, and integration between NextAuth.js and Laravel Sanctum should be confirmed by examining the frontend and backend source code.

---

## 4. Backend Technology

The backend is developed using Laravel 11 and PHP. It is responsible for server-side processing, business rules, API handling, authentication, authorization, and integration with supporting services.

### 4.1 Core Frameworks and Packages

| Technology | Declared Version | Purpose |
|---|---|---|
| PHP | `^8.2` | Backend programming language |
| Laravel | `^11.31` | Backend application framework |
| Laravel Sanctum | `^4.0` | API token and SPA authentication support |
| Laravel Tinker | `^2.9` | Interactive Laravel command-line environment |
| Dedoc Scramble | `^0.12.13` | API documentation generation |

### 4.2 Authorization and Queue Management

| Package | Declared Version | Purpose |
|---|---|---|
| Spatie Laravel Permission | `^6.15` | Role-based permissions and access control |
| Laravel Horizon | `^5.33` | Monitoring and management of Redis-backed queues |

These packages provide the foundation for access control and background job processing. Their actual configuration and use depend on the application implementation.

### 4.3 Real-Time Messaging

The project includes MQTT libraries for messaging-related functionality.

| Package | Declared Version | Environment | Purpose |
|---|---|---|---|
| `hakhant/mqtt-client` | `^1.3` | Backend | PHP MQTT client integration |
| `mqtt` | `5.9.1` | Frontend | MQTT messaging functionality |

MQTT can be used to exchange messages between applications, connected systems, and devices.

The following implementation details should be documented separately after verification:

- MQTT broker address and configuration
- Topic naming conventions
- Publishing and subscription flows
- Device communication protocols
- Authentication and connection security
- Message payload formats

---

## 5. Development and Testing Tools

The project includes tools for code quality, automated testing, debugging, local development, and environment management.

| Tool | Declared Version | Purpose |
|---|---|---|
| ESLint | `^9` | JavaScript and TypeScript linting |
| `eslint-config-next` | `15.1.6` | Next.js ESLint configuration |
| `@types/node` | `^20` | Node.js type definitions |
| `@types/react` | `^19` | React type definitions |
| `@types/react-dom` | `^19` | React DOM type definitions |
| Laravel Pint | `^1.13` | PHP code formatting |
| Pest PHP | `^3.7` | PHP application testing |
| Pest Laravel Plugin | `^3.0` | Laravel testing integration |
| Mockery | `^1.6` | Mocking objects in tests |
| Laravel Sail | `^1.26` | Docker-based Laravel development |
| Laravel Pail | `^1.1` | Application log monitoring |
| Faker PHP | `^1.23` | Test data generation |

---

## 6. Project Configuration

The frontend and backend use separate dependency management systems.

| Configuration File | Description |
|---|---|
| `package.json` | Frontend dependencies, scripts, and project metadata |
| `package-lock.json` | Exact npm dependency resolution, when present |
| `yarn.lock` | Exact Yarn dependency resolution, when present |
| `pnpm-lock.yaml` | Exact pnpm dependency resolution, when present |
| `composer.json` | PHP requirements, dependencies, autoloading, and scripts |
| `composer.lock` | Exact resolved PHP dependency versions |

### 6.1 Frontend Commands

Run these commands from the frontend project directory.

| Command | Description |
|---|---|
| `npm install` | Installs dependencies using npm |
| `npm run dev` | Starts the development server with the configured Turbopack option |
| `npm run build` | Builds the frontend for production |
| `npm run start` | Starts the production server |
| `npm run lint` | Runs the configured lint command |

### 6.2 Backend Development

The Composer configuration includes a `dev` script that runs multiple development processes concurrently.

The configured processes include:

- Laravel development server
- Laravel queue listener
- Laravel Pail log monitoring
- Frontend development server

The script uses `concurrently` through `npx` to coordinate these processes.

Run the following command from the backend project directory:

```bash
composer run dev
```

*The exact behavior depends on the scripts configured in `composer.json`.*

---

## 7. Runtime and Version Verification

The following runtime versions should be verified before finalizing the development environment documentation.

| Component | Declared Requirement | Verification Command |
|---|---|---|
| Node.js | Not specified | `node --version` |
| npm | Not specified | `npm --version` |
| PHP | `^8.2` | `php --version` |
| Composer | Not specified | `composer --version` |

### 7.1 Verify Installed Frontend Packages

Run the following command from the frontend project directory:

```bash
npm ls --depth=0
```

This lists the installed top-level npm packages and their resolved versions.

### 7.2 Verify Installed Backend Packages

Run the following command from the backend project directory:

```bash
composer show --direct
```

This lists the installed direct PHP dependencies and their resolved versions.

### 7.3 Verify Lock Files

For reproducible environments, check the dependency lock files:

- Frontend: `package-lock.json`, `yarn.lock`, or `pnpm-lock.yaml`
- Backend: `composer.lock`

The lock files provide the resolved dependency versions and are more suitable than version constraints alone for documenting the exact installed packages.

---

## 8. Summary

The Fuel Management System (Sixth Kendra) uses a modern full-stack architecture built around **Next.js 15, React 19, TypeScript, Laravel 11, and PHP 8.2 or a compatible version**.

The frontend uses Tailwind CSS, Radix UI, TanStack Query, TanStack Table, and Axios to support interface development, data presentation, and API communication.

The backend uses Laravel Sanctum for authentication support, Spatie Laravel Permission for role and permission management, Dedoc Scramble for API documentation, and Laravel Horizon for queue monitoring.

Additional tools and libraries support MQTT messaging, automated testing, code quality, development workflows, and data visualization.

**Final verification:** Exact runtime versions, authentication behavior, database configuration, queue infrastructure, and real-time messaging flows should be confirmed against the project’s lock files, environment configuration, and source code.

---

*Document type: Technical Documentation*  
*Project: Fuel Management System (Sixth Kendra)*


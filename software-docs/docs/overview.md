# Fuel Management System

## 2.1 Project Introduction

The Fuel Management System (FMS) is a web-based fuel station operations and
management platform. It provides a Next.js web application and a Laravel API
for managing station configuration, fuel inventory, dispenser and nozzle
records, sales, customer credit, and operational reports. It supports local
station and cloud deployments, with optional MQTT and ATG integrations.

The workspace contains two application repositories:

| Component | Repository directory | Main responsibility |
|---|---|---|
| Backend API | `fuel-management/` | Laravel API, operational data, and integrations with configured services and devices. |
| Web application | `fuel-management-frontend-nextjs/` | Next.js interface for station users and administrators. |

## 2.2 Project Purpose

FMS is intended to bring station operations and related records into a shared
system. It helps staff record and review sales, fuel deliveries, prices,
station equipment, and tank information. Dashboards and reports make
operational data easier to review, while MQTT and ATG integrations can reduce
manual transfer of device readings where those services are configured.

Centralized records and connected device data can reduce repeated manual
entry and help staff identify inconsistencies in operational information.
The repository does not define quantified time-saving or error-reduction
targets.

## 2.3 Project Objectives

- Centralize station, fuel, equipment, sales, and related operational records.
- Support local station operations as well as cloud-based management.
- Provide dashboards and reports for reviewing sales, inventory, and station
  activity.
- Support ATG tank data and MQTT device messaging when configured.
- Provide authenticated access and configurable roles and permissions.
- Reduce duplicate manual data entry by using connected integrations where
  available.

## 2.4 Scope of the Project

### In Scope

- User authentication and management of users, roles, and permissions.
- Management of stations, shops, tanks, fuel types, stock prices, dispensers,
  and nozzles.
- Recording sales, credit sales, station sales, fuel deliveries, price
  changes, and transaction history.
- Daily operating session actions in the local deployment.
- Dashboards and operational reports for local and cloud deployments.
- MQTT messaging and ATG data operations when the necessary external services
  and equipment are configured.
- Local-station and cloud deployment configurations.

### Out of Scope

- A native Android or iOS application; the workspace contains a web application.
- Operating or providing third-party MQTT brokers, ATG services, or station
  hardware. FMS integrates with these when configured.
- Universal uptime, response-time, retention, or recovery guarantees; these
  require deployment-specific targets.
- Payment-provider processing beyond the payment and operational records
  represented by the current application.

The features available to a user depend on deployment mode and configured
permissions.

## 2.5 Target Users & User Roles

The role names below are defined in the backend role seeder. The permission
matrix describes the seeded CRUD permissions (`create`, `read`, `update`,
`delete`); actual access to individual functions can also depend on application
logic and deployment configuration.

| Role | Seeded permissions | Notes |
|---|---|---|
| `admin` | Create, read, update, delete | Full CRUD permission set. |
| `installer` | Create, read, update, delete | Full CRUD permission set. |
| `manager` | Create, read, update | No seeded delete permission. |
| `cashier` | Create, read | No seeded update or delete permission. |
| `pprd` | Read | Read-only seeded permission. |
| `kyawsan` | Read | Read-only seeded permission. |

These are the role names present in the code, not generic job descriptions.
Administrators can manage roles and permissions through the application.

## 2.6 Key Features

- User sign-in, card authentication support, and role/permission management
- Local and cloud operation modes
- Fuel station, shop, tank, fuel type, stock price, dispenser, and nozzle
  management
- Sales, credit sales, fuel delivery, price change, and transaction history
  management
- Daily sales dashboards and local/cloud operational reports
- ATG tank data operations when configured
- MQTT messaging with connected station devices when configured
- Customer, company, member, payment, discount, and vehicle-type management in
  the cloud feature set

## System Architecture

The system uses a web client and a backend API, with supporting data and
messaging services. In a typical deployment, the frontend sends requests to
the Laravel API; the backend reads and writes application data and connects to
configured station integrations. MQTT supports real-time communication where
configured, and ATG endpoints provide tank measurement data.

**Typical data flow:** A station user works in the Next.js web application,
which sends requests to the Laravel API. The API uses MySQL and Redis and can
communicate with an MQTT broker, station devices, and an ATG service when
configured. The services enabled and their network arrangement depend on the
deployment configuration.

## Technology Stack

| Area | Technology |
|---|---|
| Backend | PHP 8.2 or later, Laravel 11 |
| Frontend | Next.js 15, React 19, TypeScript |
| Database | MySQL |
| Cache and queues | Redis, Laravel Horizon |
| Device messaging | MQTT |
| Deployment | Docker Compose; local station and cloud configurations |

## Deployment Modes

### Local station

The application can run on a station server and connect to local station
services and equipment. The backend repository includes station deployment
configuration and setup instructions. Network planning and local service
details are documented in its `NETWORK.md` and `MQTT.md` files.

### Cloud

The system can also run on a cloud server. The frontend and backend are
configured through environment variables to use the deployed API and supporting
services.

## Repository Guide

- `fuel-management/README.md`: backend setup,
  deployment, Make commands, and environment configuration.
- `fuel-management/NETWORK.md`: local station
  networking and service setup.
- `fuel-management/MQTT.md`: MQTT integration
  notes.
- `fuel-management-frontend-nextjs/README.md`:
  frontend setup and environment configuration.

## Documentation Scope

This overview introduces the system and its major components. Detailed
installation, configuration, API, deployment, and operational procedures
should be documented in their own pages as the MkDocs site grows.

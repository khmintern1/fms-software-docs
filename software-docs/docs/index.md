# Fuel Management System (FMS)

Welcome to the **Fuel Management System (FMS) Technical Documentation** for Sixth Kendra.

This documentation provides an overview of the system architecture, technology stack, database design, API interfaces, system workflows, and operational procedures.

It is intended for developers, system administrators, testers, and technical teams involved in developing, maintaining, and operating the FMS.

---

## Project Overview

The Fuel Management System is designed to support fuel station operations and centralized management.

The system documentation covers the technical structure and key processes of the application, including system components, data management, authentication, and fuel transaction workflows.

### System Modes

- **Local Mode** — Supports operations at individual fuel stations and communication with station equipment.
- **Cloud Mode** — Supports centralized management and reporting across stations.

---

## Documentation Guide

### 1. Project Documentation

Understand the system's purpose, requirements, architecture, and technology choices.

- [Project Overview](overview.md) — Introduction to the FMS project and its objectives.
- [Requirements](requirements.md) — Functional and non-functional requirements.
- [System Architecture](architecture.md) — High-level architecture, system components, and data flow.
- [Technology Stack](techstack.md) — Technologies, frameworks, and libraries used in the project.

### 2. Technical Reference

Explore the system's data structure, APIs, and security mechanisms.

- [Database Design](database.md) — Database structure, tables, and relationships.
- [API Documentation](api.md) — API endpoints, request parameters, and responses.
- [Authentication & Authorization](authentication.md) — Login, authentication, and access control.

### 3. System Workflows

Understand how key business processes operate step by step.

- [Authentication Workflow](workflows/authentication.md) — User authentication process.
- [Sales Transaction Workflow](workflows/sale-transaction-sequence.md) — Fuel sales transaction process.
- [Fuel Delivery Workflow](workflows/fuel-delivery.md) — Fuel delivery and recording process.
- [Price Change Workflow](workflows/price-change.md) — Fuel price update process.
- [Start / End Session Workflow](workflows/session.md) — Session start and end processes.
- [Credit Sale Workflow](workflows/credit-sale.md) — Credit sales process.

### 4. User Guide

- [User Manual](user-manual.md) — Instructions for using the system's features.

### 5. Development & Operations

Guides for setting up, testing, deploying, and maintaining the application.

- [Installation & Setup](installation.md) — Development environment setup and installation.
- [Testing](testing.md) — Testing procedures and test execution.
- [Deployment & Maintenance](deployment.md) — Deployment configuration and maintenance procedures.

---

## How to Use This Documentation

1. Start with **Project Overview** to understand the system's purpose.
2. Read **System Architecture** to learn how the components interact.
3. Review **Technology Stack** and **Database Design** for technical details.
4. Explore **System Workflows** to understand individual business processes.
5. Refer to **API Documentation** and **Authentication & Authorization** when working with backend integration and security.
6. Use **Installation & Setup** to prepare the development environment.

---

## Documentation Scope

This documentation focuses on the system's architecture, technical implementation, business workflows, and operational guidance.

The content should be maintained alongside the source code so that documented behavior, API interfaces, and system workflows remain consistent with the actual implementation.

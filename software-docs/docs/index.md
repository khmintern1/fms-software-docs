# Fuel Management System (FMS)

**Technical Documentation | Sixth Kendra**

Welcome to the official technical documentation of the **Fuel Management System (FMS)**.

This documentation provides a comprehensive technical reference for understanding the system architecture, software components, database design, API interfaces, business logic, operational workflows, and installation procedures.

It is intended for developers, system administrators, testers, installers, and technical teams responsible for developing, maintaining, deploying, and operating the FMS.

---

## System Overview

The **Fuel Management System (FMS)** is designed to manage fuel station operations and provide centralized management across multiple stations.

The system integrates software applications with station equipment to support fuel transactions, dispenser operations, fuel inventory management, business calculations, and operational reporting.

### System Modes

| Local Mode | Cloud Mode |
|---|---|
| Manages individual fuel station operations. | Provides centralized management across multiple stations. |
| Supports communication with station equipment and dispensers. | Supports centralized monitoring and reporting. |
| Processes local transactions and operational data. | Consolidates station information and business data. |

---

## Documentation Center

This documentation is organized into eight main sections. Each section covers a specific area of the FMS.

### 01. Project Documentation

Understand the project objectives, requirements, and overall system background.

- [Project Overview](overview.md)
- [Requirements](requirements.md)

### 02. System Architecture

Explore the system architecture, software components, communication structure, and technology choices.

- [Architecture Overview](architecture.md)
- [Technology Stack](techstack.md)

### 03. System Workflows

Understand how the system handles important business operations through step-by-step workflows.

- [Authentication Workflow](workflows/authentication-and-authorization-sequence.md)
- [Sales Transaction Workflow](workflows/sale-transaction-sequence.md)
- [Fuel Delivery Workflow](workflows/fuel-in-sequence.md)
- [Price Change Workflow](workflows/price-change.md)
- [Start / End Session Workflow](workflows/start-end-session.md)
- [Credit Sale Workflow](workflows/credit-sale-and-payment-workflow.md)

### 04. Business Logic & Formulas

Reference the calculation methods and business rules used in fuel sales, inventory management, and reporting.

- [Calculation Reference](formulas/index.md)
- [Fuel Sales Formulas](formulas/sales.md)
- [Fuel Conversion](formulas/unit-conversion.md)
- [Tank Report](formulas/tank-report.md)
- [Dispenser Report](formulas/dispenser-report.md)
- [Fuel Delivery Report](formulas/fuel-delivery.md)
- [Daily Sales Report](formulas/daily-sales.md)

### 05. Technical Reference

Explore the system's database structure, API interfaces, and security mechanisms.

- [Database Design](technical-reference/database.md)
- [API Documentation](technical-reference/api.md)
- [Authentication & Authorization](technical-reference/authentication.md)

### 06. User Guide

Learn how to use the FMS application and manage its main features.

- [User Guide Overview](user-guide/index.md)
- [Application Website](user-guide/application-website.md)
- [Device Control](user-guide/device-control.md)
- [Sales Management](user-guide/sales.md)
- [Reports & Tank Management](user-guide/reports.md)
- [Master Data Management](user-guide/master-data.md)

### 07. Installer Guide

Follow the installation procedures for configuring a fuel station and its related equipment.

- [Installer Guide Overview](installer-guide/index.md)
- [Before You Begin](installer-guide/before-you-begin.md)
- [Shop & Station Setup](installer-guide/shop-station-setup.md)
- [Tank, Fuel Type & History Setup](installer-guide/tank-fuel-history-setup.md)
- [Stock Price Setup](installer-guide/stock-price-setup.md)
- [Dispenser & Nozzle Setup](installer-guide/dispenser-nozzle-setup.md)
- [Cashier Account Setup](installer-guide/cashier-setup.md)
- [Fuel In Setup](installer-guide/fuel-in-setup.md)
- [Firmware OTA Update](installer-guide/firmware-ota-update.md)
- [Installer Checklist](installer-guide/installer-checklist.md)

### 08. Development & Operations

Find technical instructions for setting up, configuring, and maintaining the application.

- [Installation & Setup](installation.md)
- [MQTT Integration](mqtt.md)

---

## Getting Started

Choose the appropriate starting point based on your role and responsibilities.

| Role | Recommended Starting Point |
|---|---|
| Developer | Project Documentation → System Architecture → Technical Reference |
| System Administrator | System Architecture → Installation & Setup → MQTT Integration |
| Tester | Requirements → System Workflows → User Guide |
| Installer | Installer Guide → Installer Checklist |
| Application User | User Guide → Application Website |

---

## Documentation Maintenance

This documentation is maintained alongside the FMS source code to ensure that technical information remains consistent with the actual system implementation.

Any changes to system architecture, database structures, API interfaces, business rules, workflows, or installation procedures should be reflected in their corresponding documentation pages.

---

**Fuel Management System (FMS)**  
*Technical Documentation — Sixth Kendra*

**Version:** V2.2

**Author:** iih (Innovatic IoT House)

**© 2025 Digital Engineering Tech Ltd.
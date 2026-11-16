# Database Design

## 1. Overview

The Fuel Management System (FMS) uses a relational database to manage user accounts, station configuration, fuel inventory, dispenser operations, sales transactions, customer credit, operational history, and application infrastructure.

The database schema is defined by the Laravel migrations in `fuel-management/database/migrations/`. This document summarizes the tables and principal relationships identified in the supplied schema reference.

MySQL is used in the documented deployments. The actual database connection and schema may depend on the deployment configuration, particularly where the `SwitchDatabase` middleware is used.

## 2. Database Scope and Source of Truth

Laravel migration files are the authoritative source for column definitions, data types, nullability, default values, indexes, foreign-key constraints, and schema changes.

The schema has evolved through multiple migrations. Later migrations may add, modify, or remove columns from the initial table definitions. Therefore, the final schema must be determined by reviewing all applicable migrations in timestamp order.

The application supports local-station and cloud API modes. Do not assume that all requests use the same database connection or tenant scope without checking the middleware and deployment configuration.

## 3. Entity Groups and Complete Table Inventory

### 3.1 Organization and Access

| Table | Purpose |
|---|---|
| `shops` | Stores shop or organization information. |
| `users` | Stores application user accounts and station/shop associations. |
| `roles` | Stores role definitions. |
| `permissions` | Stores permission definitions. |
| `model_has_roles` | Assigns roles to models through polymorphic relationships. |
| `model_has_permissions` | Assigns permissions to models through polymorphic relationships. |
| `role_has_permissions` | Associates roles with permissions. |
| `personal_access_tokens` | Stores Laravel Sanctum personal access tokens. |

### 3.2 Station and Equipment

| Table | Purpose |
|---|---|
| `stations` | Stores station configuration and operational settings. |
| `tanks` | Stores fuel tank information and measurements. |
| `fuel_types` | Stores fuel type information associated with tanks. |
| `stock_prices` | Stores station-specific fuel prices. |
| `dispensers` | Stores dispenser and device configuration. |
| `nozzles` | Stores nozzle configuration and associated stock prices. |

### 3.3 Station Operations

| Table | Purpose |
|---|---|
| `sales` | Stores fuel sale transactions and associated transaction information. |
| `station_sales` | Stores station/shop sales summary or transfer information. |
| `fuel_ins` | Stores fuel receipt and fuel-in records. |
| `histories` | Stores operational history records. |
| `history_details` | Stores details associated with operational history records. |

### 3.4 Customers and Credit

| Table | Purpose |
|---|---|
| `companies` | Stores company information associated with station customers. |
| `customers` | Stores customer information, including vehicle and company associations. |
| `members` | Stores customer membership and point-related records. |
| `vehicle_types` | Stores vehicle type information. |
| `customer_credits` | Stores customer credit configuration and limits. |
| `credit_sales` | Stores credit sale amounts, repayment information, and payment status. |
| `payments` | Stores payment method information. |
| `discounts` | Stores discount type information. |

### 3.5 Laravel Framework and Infrastructure

| Table | Purpose |
|---|---|
| `migrations` | Tracks executed Laravel database migrations. |
| `password_reset_tokens` | Stores password reset token information. |
| `sessions` | Stores session data when the configured session driver uses database storage. |
| `cache` | Stores cached values when the configured cache driver uses database storage. |
| `cache_locks` | Stores cache lock information for the database cache driver. |
| `jobs` | Stores queued jobs when the configured queue driver uses database storage. |
| `job_batches` | Stores information about batches of queued jobs. |
| `failed_jobs` | Stores information about failed queued jobs. |

The Laravel infrastructure tables are included in the inventory because they appear in the supplied schema reference. Their use at runtime depends on the application's configuration.

## 4. Entity Relationship Diagram (ERD)

The ERD image illustrates the principal database relationships identified in the supplied schema reference.

![Fuel Management System Entity Relationship Diagram](images/database-erd.png)

*Figure 1: Fuel Management System Entity Relationship Diagram*

The diagram should be treated as a high-level relationship overview, not a complete physical schema. Refer to the Laravel migrations for exact foreign keys, indexes, nullability, uniqueness, and delete behavior.

## 5. Principal Relationships

The following relationships are based on the foreign-key fields described in the supplied schema reference. A matching ID column does not necessarily mean a database-enforced foreign-key constraint exists; the migrations must explicitly define that constraint.

### 5.1 Organization and Station

- `shops` is associated with `users` through `users.shop_id`.
- `shops` is associated with `stations` through `stations.shop_id`.
- `stations` is associated with `users` through `users.station_id`.

### 5.2 Tanks and Fuel Configuration

- `stations` is associated with `tanks` through `tanks.station_id`.
- `tanks` is associated with `fuel_types` through `fuel_types.tank_id`.
- `stations` is associated with `stock_prices` through `stock_prices.station_id`.
- `fuel_types` is associated with `stock_prices` through `stock_prices.fuel_type_id`.
- `stations` is associated with `dispensers` through `dispensers.station_id`.
- `dispensers` is associated with `nozzles` through `nozzles.dispenser_id`.
- `stock_prices` is associated with `nozzles` through `nozzles.stock_price_id`.

### 5.3 Sales and Operational History

- `stations` is associated with `sales` through `sales.station_id`.
- `dispensers` is associated with `sales` through `sales.dispenser_id`.
- `nozzles` is associated with `sales` through `sales.nozzle_id`.
- `stations` is associated with `fuel_ins` through `fuel_ins.station_id`.
- `tanks` is associated with `fuel_ins` through `fuel_ins.tank_id`.
- `stations` is associated with `histories` through `histories.station_id`.
- `tanks` is associated with `histories` through `histories.tank_id`.
- `histories` is associated with `history_details` through `history_details.history_id`. The supplied schema reference indicates that `history_details.history_id` is unique in the final migration.
- `station_sales` contains both `shop_id` and `station_id` fields.

### 5.4 Customers and Credit

- `stations` is associated with `companies` through `companies.station_id`.
- `companies` is associated with `customers` through `customers.company_id`.
- `customers` is associated with `members` through `members.customer_id`.
- `customers` is associated with `customer_credits` through `customer_credits.customer_id`.
- `customer_credits` is associated with `credit_sales` through `credit_sales.customer_credit_id`.
- `sales` is associated with `credit_sales` through `credit_sales.sale_id`.
- `sales` contains optional references to `payments`, `discounts`, `customers`, `vehicle_types`, `fuel_types`, and `tanks`, as described in the supplied schema reference.

### 5.5 Authentication and Authorization

- `users` can receive role assignments through `model_has_roles`.
- Models can receive direct permission assignments through `model_has_permissions`.
- `roles` are associated with `permissions` through `role_has_permissions`.
- `personal_access_tokens` stores token ownership using the polymorphic `tokenable_type` and `tokenable_id` fields.

The Spatie Laravel Permission assignments are polymorphic. Their model identifiers should not be treated as ordinary foreign keys to `users` alone.

## 6. Important Schema Notes

### 6.1 Users and Stations

The `users` table includes `shop_id`, `station_id`, `cashier_code`, `card_id`, and other application-specific fields. Some fields were added by later migrations.

The `stations` table includes station configuration such as `station_no`, contact details, opening and expiry dates, operating hours, ATG and PPRD flags, and network/device-related fields.

### 6.2 Tanks, Fuel Types, and Stock Prices

The supplied schema defines `fuel_types.tank_id`, `stock_prices.fuel_type_id`, and `stock_prices.station_id`.

The `nozzles` table references `stock_prices` through `stock_price_id`. Therefore, the ERD should not show a direct `nozzles.fuel_type_id` relationship unless that field is confirmed in the actual schema.

### 6.3 Sales and Credit Sales

The `sales` table contains transaction fields, including station, dispenser, nozzle, price, quantity, payment, and optional customer-related references.

The `credit_sales` table contains `sale_id` and `customer_credit_id`, as well as credit amount, return amount, return date, and payment status.

The customer credit relationship should therefore be represented through `customer_credits`, rather than assuming that `credit_sales` has a direct `customer_id` column.

### 6.4 History Records

The `histories` table stores operational history information, while `history_details` stores the associated details, including sales and tank-related measurements.

The supplied schema reference indicates that the final migration makes `history_details.history_id` unique and modifies the precision of selected numeric columns.

### 6.5 Discounts

The supplied schema reference indicates that the initial `discounts` migration contained `amount` and `percent_amount`, but a later migration removes those columns.

They should not be documented as part of the final schema unless a later migration reintroduces them.

### 6.6 Framework Tables

The `migrations` table is maintained by Laravel's migration repository. The `password_reset_tokens`, `sessions`, `cache`, `cache_locks`, `jobs`, `job_batches`, and `failed_jobs` tables support framework features when their corresponding database-backed drivers or features are configured.

## 7. Schema Changes and Migrations

Before changing the database schema:

1. Review the relevant table-creation migration.
2. Review all subsequent migrations that add, modify, rename, or remove columns.
3. Verify primary keys, foreign keys, indexes, unique constraints, nullability, and defaults.
4. Confirm whether the change affects local-station mode, cloud mode, or both.
5. Test the migration in the intended environment before deploying it.

The standard Laravel command to run pending migrations is:

```bash
php artisan migrate
```

Run this command only in the intended environment and follow the project's deployment procedure.

## 8. Related Source Files

- `fuel-management/database/migrations/`
- `fuel-management/app/Models/`
- `fuel-management/app/Http/Resources/`
- `fuel-management/app/Http/Middleware/Cloud/SwitchDatabase.php`

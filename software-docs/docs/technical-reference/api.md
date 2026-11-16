# API Documentation

## Base URL and conventions

Laravel registers the API under `v1/api` (see `fuel-management/bootstrap/app.php`). Replace `{host}` with the backend host, for example `https://api.example.test`:

| Route family | Base path | Authentication |
|---|---|---|
| Service root | `/v1/api` | Public |
| Authentication and users | `/v1/api/users` | Login, registration, and card-auth are guest routes; user management and logout require Sanctum. |
| Local operations | `/v1/api/local` | Sanctum required for the entire group. |
| Cloud operations | `/v1/api/cloud` | Sanctum required for the entire group. |

Requests and responses are JSON where supported. Resource endpoints follow Laravel `apiResource` conventions: `GET /{resource}`, `POST /{resource}`, `GET /{resource}/{id}`, `PUT|PATCH /{resource}/{id}`, and `DELETE /{resource}/{id}`. Request fields and response resources are endpoint-specific; consult the matching Form Request, controller, and Resource class.

For the generated, interactive API reference, the backend includes Dedoc Scramble. Availability and URL depend on the installed package configuration and deployment.

## Service root

`GET /v1/api` returns API version/status metadata and links to the local, cloud, and login route families.

## Authentication routes

| Method | Path | Description |
|---|---|---|
| POST | `/users/login` | Authenticate with the login request credentials and issue a Sanctum bearer token. |
| POST | `/users/card-auth` | Authenticate using the submitted card value and issue a token. |
| POST | `/users/register` | Register via the guest registration handler. |
| POST | `/users/logout` | Revoke the authenticated user's tokens. |
| GET | `/users/all` | List users. |
| GET | `/users/show/{id}` | Read a user. |
| POST | `/users/create` | Create a user. |
| PUT | `/users/update/{id}` | Update a user. |
| DELETE | `/users/delete/{id}` | Delete a user. |
| API resource | `/users/roles`, `/users/permissions` | Role and permission CRUD routes. |
| DELETE | `/users/{userId}/roles/{roleId}` | Remove role assignment from user. |
| DELETE | `/users/{userId}/permissions/{permissionId}` | Remove direct permission from user. |
| DELETE | `/users/roles/{roleId}/permissions/{permissionId}` | Remove permission from role. |

The resource routes above use the standard Laravel `apiResource` methods and URL patterns for each named resource.

## Local API

All paths below are relative to `/v1/api/local` and require Sanctum authentication.

| Resource or route | Available operations |
|---|---|
| `tanks`, `fuel-types`, `stock-prices`, `nozzles`, `shops`, `stations`, `dispensers` | Standard API resource CRUD. |
| `histories`, `history-details`, `fuelins` | Standard API resource CRUD. |
| `fuelins/atg/create` | POST: create fuel-in record from ATG data. |
| `fuelins/{id}/atg/update` | PUT: update ATG data for a fuel-in record. |
| `price-changes` | POST: record a price change. |
| `sales` | Standard API resource CRUD. |
| `sales/preset/{type}` | POST: preset sale operation. |
| `sales/approve-by/cashier` | POST: cashier sale approval operation. |
| `credit-sales`, `station-sales` | Standard API resource CRUD. |
| `credit-sales/customer/by-amount` | PUT: update credit-sale customer by amount. |
| `today-sales/dashboard` | GET: dashboard data. |
| `daily-start`, `daily-end` | POST: start/end daily session. |
| `reports/daily-sales`, `reports/daily-lists`, `reports/sale-summary`, `reports/sale-summary-detail` | GET: daily and summary reports. |
| `reports/totalizer-difference`, `reports/daily-sales-report`, `reports/sale-ledger-report` | GET: operational reports. |
| `reports/sale-ledger-report/today-tank/update/{id}` | PUT: update today's tank ledger data. |

## Cloud API

All paths below are relative to `/v1/api/cloud` and require Sanctum authentication. Routes inside the `switch` middleware group may also switch database context.

| Resource or route | Available operations |
|---|---|
| `shops`, `stations`, `members`, `payments`, `discounts`, `vehicle-types`, `customer-credits` | Standard API resource CRUD. |
| `info/dashboard` | GET: cloud dashboard. |
| `station-sales/by-shop` | GET: station sales grouped by shop. |
| `tanks`, `sales`, `fuelins`, `nozzles`, `histories`, `fuel-types` | Standard API resource CRUD (switch middleware). |
| `dispensers`, `stock-prices`, `station-sales`, `history-details` | Standard API resource CRUD (switch middleware). |
| `companies`, `customers`, `credit-sales` | Standard API resource CRUD (switch middleware). |
| `customers/car-numbers`, `customers/top-amount` | GET: customer lookup and top-customer operation. |
| `customer-credits` | Resource CRUD is also registered inside the switch group. |
| `credit-sales/customer/by-amount` | PUT: update credit-sale customer by amount. |
| `reports/tank-report`, `reports/pump-report`, `reports/weekly-report`, `reports/fuelin-report` | GET: operational reports. |
| `reports/daily-sale-report`, `reports/sale-detail-report`, `reports/daily-sale-category-report` | GET: sales reports. |
| `reports/different-totalizer-report` | GET: totalizer difference report. |

## Responses and errors

The shared response helper commonly returns `message`, `code`, and `data`; login returns a bearer token and user resource in `data`. Laravel validation, authentication, authorization, and not-found exceptions may use framework response shapes. Treat each endpoint's implementation as authoritative for its exact response schema and status codes.

## Source of truth

- Route definitions: `fuel-management/routes/api.php`, `routes/auth/api.php`, `routes/local/api.php`, and `routes/cloud/api.php`.
- Validation: `fuel-management/app/Http/Requests/`.
- Response shape: corresponding controllers and `app/Http/Resources/`.

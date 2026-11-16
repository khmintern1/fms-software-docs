# Authentication & Authorization

## Authentication mechanisms

The backend uses Laravel Sanctum for API authentication and includes the Next.js frontend's NextAuth session configuration. These are separate layers: frontend session setup does not itself prove that a backend request is authenticated. Backend route middleware is the authority for API access.

### Password login

`POST /v1/api/users/login` accepts the fields validated by `App\Http\Requests\Auth\LoginRequest`. On success, the backend creates a Sanctum personal access token and returns a response containing `token`, `token_type` (`Bearer`), and a serialized user under `data`.

Send the token on protected API requests:

```http
Authorization: Bearer <token>
Accept: application/json
```

### Card login

`POST /v1/api/users/card-auth` accepts the card value validated by `CardRequest`. The handler looks up a user by the submitted card ID and issues a Sanctum token when found. An unknown card returns a not-found error. This endpoint is in the guest route group.

### Registration and logout

`POST /v1/api/users/register` is also in the guest route group. Its accepted fields and account-creation policy are defined by `RegisteredUserController` and its request validation.

`POST /v1/api/users/logout` requires Sanctum authentication. The current implementation deletes all personal access tokens belonging to the authenticated user, then returns a success response.

## Route protection

| Route family | Middleware | Access |
|---|---|---|
| `/v1/api/users/login`, `/card-auth`, `/register` | `guest` | Public to unauthenticated callers. |
| User administration and role/permission routes | `auth:sanctum`, `switch` | Authenticated token required; database context middleware also runs. |
| `/v1/api/local/*` | `auth:sanctum` | Authenticated token required. |
| `/v1/api/cloud/*` | `auth:sanctum` | Authenticated token required; selected routes add `switch`. |

The `/v1/api` service root is public. A successful login establishes authentication but does not, by itself, establish authorization for every operation.

## Authorization and roles

The backend includes Spatie Laravel Permission tables and APIs for roles, permissions, and their assignments. The migration defines `roles`, `permissions`, `model_has_roles`, `model_has_permissions`, and `role_has_permissions`; model assignments are polymorphic.

The repository seeds role names including `admin`, `installer`, `manager`, `cashier`, `pprd`, and `kyawsan`. Role names alone do not define effective access: enforcement depends on permission assignments and controller/middleware checks. Review the current seeders and route/controller authorization before changing access policy. This reference does not claim a fixed role-to-endpoint matrix.

## Token storage and revocation

Sanctum tokens are stored in `personal_access_tokens`. Login and card-auth create tokens named `auth_token`. Logout revokes every token belonging to the current user, rather than only the token used for that request.

## Frontend integration and transport

The Next.js application has NextAuth configuration and a shared Axios client. Follow the frontend's current API client configuration for token handling; do not assume browser session cookies are interchangeable with bearer tokens. Use HTTPS in deployments that expose the API beyond a trusted local network, and keep application secrets in environment configuration rather than source control.

## Implementation references

- `fuel-management/routes/auth/api.php`
- `fuel-management/routes/api.php`
- `fuel-management/bootstrap/app.php`
- `fuel-management/app/Http/Controllers/Auth/AuthenticatedSessionController.php`
- `fuel-management/app/Http/Controllers/Auth/CardAuthController.php`
- `fuel-management/app/Http/Requests/Auth/`
- `fuel-management/database/migrations/2024_12_13_081341_create_personal_access_tokens_table.php`
- `fuel-management/database/migrations/2025_02_19_132650_create_permission_tables.php`

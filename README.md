# MPESA APP

## Complete Project Documentation, System Architecture, Developer Reference & Offline Learning Manual

**Project Name:** MPESA APP  
**Project Type:** Multi-Tenant M-Pesa Payment Gateway  
**Backend:** PHP  
**Database:** MySQL  
**Integration:** Safaricom Daraja API  
**Architecture:** REST API + Web Dashboard + Payment Processing Engine  
**Development Environment:** Visual Studio Code, PHP and MySQL  
**Project Status:** Under Development

---

# TABLE OF CONTENTS

1. Project Overview
2. Project Background and Purpose
3. Project Objectives
4. Scope of the Application
5. High-Level System Architecture
6. Application Components
7. Multi-Tenant Architecture
8. Organization Management
9. User Management and Role-Based Access Control
10. Merchant Client Management
11. API Architecture
12. Authentication and Authorization
13. M-Pesa Payment Integration
14. STK Push Request Processing
15. Safaricom Callback Processing
16. Merchant Webhook Processing
17. STK Requests and Transactions
18. Merchant Dashboard
19. Complete Database Architecture
20. Database Relationships and Data Ownership
21. Application Directory Structure
22. Detailed Explanation of Every Folder and File
23. How the Application Components Communicate
24. Security Architecture
25. Reliability, Error Handling and Recovery
26. Logging and Monitoring
27. Development Environment and Configuration
28. API Documentation Standards
29. Testing Strategy
30. Deployment Architecture
31. Backup and Disaster Recovery
32. Development Workflow
33. Developer Implementation Guide
34. How to Build New Features Independently
35. Practical Feature Development Examples
36. Offline Programming and Study Guide
37. Recommended Programming Concepts and Books
38. Git and Version Control Guide
39. Feature Development Roadmap
40. Development Standards
41. Architectural Decisions
42. Future Expansion
43. Final Project Vision

---

# PART A — COMPLETE SYSTEM DOCUMENTATION

# 1. Project Overview

MPESA APP is a standalone, multi-tenant M-Pesa payment gateway designed to allow multiple organizations to receive M-Pesa payments through their websites and applications using a centralized payment processing platform.

The application acts as an intermediary between external merchant websites, Safaricom Daraja and customers making payments.

Instead of every merchant independently implementing and maintaining its own Safaricom integration, merchants can connect to MPESA APP through its API.

The gateway handles payment initiation, Safaricom communication, callback reception, transaction processing, payment records and merchant notifications.

The application also provides a web-based administration dashboard where organizations can manage their merchant integrations, users, transactions and payment activity.

MPESA APP is not simply an API. It is a complete payment management platform consisting of:

1. A payment API for external websites and applications.
2. A multi-tenant organization management system.
3. A merchant client management system.
4. An STK Push processing engine.
5. A Safaricom callback processing engine.
6. A transaction management system.
7. A merchant webhook notification system.
8. An administrative dashboard.
9. A user, role and permission management system.
10. A logging, monitoring and recovery system.

The system is being developed as a new independent application.

The existing production M-Pesa application is only a reference for understanding existing functionality. It must remain operational and untouched throughout development of the new MPESA APP.

The new application will have:

- Its own source code.
- Its own database.
- Its own environment configuration.
- Its own application structure.
- Its own API architecture.
- Its own deployment configuration.

The two applications must not accidentally share development tables, credentials or deployment configurations.

# 2. Project Background and Purpose

The purpose of MPESA APP is to create a reusable payment gateway that can serve different organizations and their merchant websites from one centralized platform.

Consider an organization that operates an online shop.

The shop wants customers to pay using M-Pesa.

Instead of implementing the entire Daraja integration inside the shop, the shop connects to MPESA APP.

The process becomes:

1. The customer visits the merchant website.
2. The customer selects M-Pesa as the payment method.
3. The merchant website sends a payment request to MPESA APP.
4. MPESA APP authenticates the merchant.
5. MPESA APP initiates an STK Push through Safaricom.
6. Safaricom sends the payment prompt to the customer's phone.
7. The customer completes the payment process.
8. Safaricom sends the payment result to MPESA APP.
9. MPESA APP processes and records the outcome.
10. MPESA APP updates its dashboard.
11. MPESA APP notifies the original merchant website through its webhook mechanism.

The merchant can then update its own order or service status based on the verified payment notification.

The gateway centralizes payment integration while preserving separation between organizations and their merchant clients.

# 3. Project Objectives

The application should achieve the following objectives.

## 3.1 Payment processing

- Initiate M-Pesa STK Push requests.
- Communicate with Safaricom Daraja.
- Receive Safaricom callbacks.
- Process successful and unsuccessful payment outcomes.
- Maintain transaction records.
- Prevent duplicate transaction processing.
- Handle delayed callbacks.
- Recover from processing failures.

## 3.2 Multi-tenant management

- Support multiple organizations.
- Maintain organization-specific merchant clients.
- Maintain organization-specific users.
- Separate organization data logically.
- Allow platform-wide administration.
- Allow organizations to manage their own users and operations.

## 3.3 Merchant integration

- Provide an API for external websites.
- Authenticate merchant requests.
- Associate every merchant with an organization.
- Generate unique merchant credentials.
- Support merchant webhook configuration.
- Allow merchants to retrieve authorized transaction information.

## 3.4 Dashboard

- Provide transaction monitoring.
- Display payment summaries.
- Allow transaction searching.
- Allow date and status filtering.
- Provide transaction details.
- Manage organizations.
- Manage merchant clients.
- Manage users and permissions.

## 3.5 Security and reliability

- Protect credentials.
- Enforce role-based authorization.
- Enforce organization-level data isolation.
- Preserve callback records.
- Prevent duplicate financial records.
- Maintain audit logs.
- Support error recovery.
- Provide reliable backup and restoration procedures.

# 4. Scope of the Application

MPESA APP has three primary operational areas.

## 4.1 Platform administration

This area is controlled by the Super Admin.

It is responsible for managing the entire gateway.

## 4.2 Organization administration

This area is controlled by Client Admins and other authorized organization users.

It is responsible for managing an individual organization's payment operations.

## 4.3 Merchant integration

This area allows external merchant websites and applications to connect to the gateway.

Merchant integration happens through authenticated API requests and webhook notifications.

These three areas must work together while maintaining their own responsibilities.

# 5. High-Level System Architecture

The following diagram represents the intended payment architecture.

```text
                 EXTERNAL MERCHANT WEBSITE
                           |
                           |
                    Customer Payment
                           |
                           v
                  MPESA APP PAYMENT API
                           |
                           v
                    MERCHANT AUTHENTICATION
                           |
                           v
                 ORGANIZATION IDENTIFICATION
                           |
                           v
                    REQUEST VALIDATION
                           |
                           v
                     STK REQUEST RECORD
                           |
                           v
                     STK PUSH SERVICE
                           |
                           v
                    SAFARICOM DARAJA
                           |
                           v
                     CUSTOMER PHONE
                           |
                           v
                    PAYMENT COMPLETION
                           |
                           v
                  SAFARICOM CALLBACK
                           |
                           v
                   CALLBACK PROCESSOR
                           |
                           v
                   TRANSACTION PROCESSING
                           |
                           v
                    CENTRAL DATABASE
                           |
                 +---------+----------+
                 |                    |
                 v                    v
          MERCHANT WEBHOOK      ADMIN DASHBOARD
                 |                    |
                 v                    v
        MERCHANT WEBSITE       PAYMENT MONITORING
```

## 5.1 Main architectural principle

The gateway is responsible for processing payment information and maintaining the authoritative payment records.

Merchant websites should not be trusted to declare a payment successful merely because a customer has returned to a success page.

The merchant should rely on a verified server-side notification or an authorized transaction-status response from MPESA APP.

# 6. Application Components

The system is divided into several logical components.

| Component | Responsibility |
| --- | --- |
| API Layer | Receives external merchant requests |
| Authentication Layer | Identifies and authenticates users and merchants |
| Organization Layer | Manages organizations and tenant ownership |
| User Management | Manages users, roles and permissions |
| Merchant Management | Registers and manages merchant websites |
| STK Service | Initiates Safaricom STK Push requests |
| Callback Processor | Receives and processes Safaricom callbacks |
| Transaction Service | Manages payment transaction records |
| Webhook Service | Notifies merchant websites |
| Dashboard | Provides browser-based management |
| Database Layer | Stores application data |
| Logging Layer | Records errors and operational activity |
| Recovery Layer | Supports retrying recoverable operations |

Each component should have a clearly defined responsibility.

Controllers should coordinate requests, while services should contain the main business operations.

# 7. Multi-Tenant Architecture

MPESA APP uses a centralized multi-tenant architecture.

This means all organizations use the same application and centralized database, but each organization can only access its own authorized data.

The application does not need a separate database for every organization.

Instead, shared database tables contain records belonging to different organizations.

## 7.1 Example organization structure

```text
MPESA APP
    |
    +--- Organization A
    |       |
    |       +--- Merchant Website A1
    |       +--- Merchant Website A2
    |       +--- Users
    |       +--- STK Requests
    |       +--- Transactions
    |
    +--- Organization B
    |       |
    |       +--- Merchant Website B1
    |       +--- Merchant Website B2
    |       +--- Users
    |       +--- STK Requests
    |       +--- Transactions
    |
    +--- Organization C
            |
            +--- Merchant Website C1
            +--- Users
            +--- STK Requests
            +--- Transactions
```

## 7.2 Centralized database

All organizations share the same database tables.

For example, the `transactions` table may contain:

| Transaction ID | Organization ID | Amount | Status |
| --- | --- | ---: | --- |
| TX001 | ORG001 | 500 | SUCCESS |
| TX002 | ORG002 | 1000 | FAILED |
| TX003 | ORG001 | 250 | SUCCESS |
| TX004 | ORG003 | 750 | PENDING |

The `organization_id` determines which organization owns each record.

## 7.3 Organization isolation

Every organization-owned record must be associated with an organization.

Examples include:

- Users.
- Merchant clients.
- STK requests.
- Transactions.
- Merchant webhook configurations.
- Organization settings.
- Organization audit records where applicable.

When an organization user logs in, the backend must identify the user's organization and apply that organization restriction to every relevant query.

For example:

```sql
SELECT *
FROM transactions
WHERE organization_id = ?;
```

The organization ID must come from the authenticated user's trusted server-side identity or authenticated merchant record.

It must not be accepted from an untrusted browser parameter as proof of access.

## 7.4 Why this architecture is preferred

A shared database provides:

- Centralized management.
- Easier maintenance.
- Consistent database structure.
- Easier platform-wide monitoring.
- Simpler database migrations.
- Easier reporting.
- Reduced infrastructure duplication.

The trade-off is that tenant isolation must be implemented carefully in every relevant application operation.

# 8. Organization Management

Organizations are the primary tenants of MPESA APP.

Each organization represents an entity using the gateway.

Examples might include:

- An online store operator.
- An educational institution.
- A service provider.
- A booking platform.
- A business operating multiple websites.

## 8.1 Organization information

The `organizations` table should store information such as:

- Organization ID.
- Organization name.
- Registration or reference information where applicable.
- Contact email.
- Contact phone.
- Status.
- Created timestamp.
- Updated timestamp.

The exact fields can be refined during database design.

## 8.2 Organization operations

The Super Admin should be able to:

- Register organizations.
- View organizations.
- Update organization information.
- Activate organizations.
- Suspend organizations.
- Monitor organization activity.

An organization administrator should only manage their own organization.

# 9. User Management and Role-Based Access Control

User management is a central part of the application.

There are two primary roles.

## 9.1 Super Admin

The Super Admin is the platform-level administrator.

The Super Admin is responsible for the entire MPESA APP platform.

Responsibilities include:

- Managing organizations.
- Monitoring all organizations.
- Managing platform configuration.
- Monitoring platform-wide transactions.
- Reviewing system errors.
- Managing platform-level users and permissions.
- Investigating operational issues.

A platform Super Admin may have a `NULL` organization ID because the account operates at platform level rather than belonging to an individual tenant.

## 9.2 Client Admin

The Client Admin is an administrator belonging to one organization.

Responsibilities include:

- Managing the organization's merchant clients.
- Managing organization users.
- Assigning organization roles.
- Managing organization permissions.
- Viewing organization transactions.
- Monitoring payment requests.
- Managing permitted payment settings.

A Client Admin must always be associated with an organization.

## 9.3 Organization-defined roles

Other roles are not fixed system-wide roles.

Each organization may add roles according to its operational requirements.

For example:

```text
Organization A
    |
    +--- Client Admin
    +--- Accountant
    +--- Cashier
    +--- Auditor

Organization B
    |
    +--- Client Admin
    +--- Support Staff
    +--- Transaction Viewer
```

One organization might need an Accountant, while another might not.

The system should therefore support configurable roles and permissions rather than hardcoding every possible job title.

## 9.4 Suggested role and permission tables

The system should use a flexible role-based access control structure.

| Table | Responsibility |
| --- | --- |
| `users` | Stores user accounts |
| `roles` | Stores platform and organization-defined roles |
| `permissions` | Stores available permissions |
| `user_roles` | Associates users with roles |
| `role_permissions` | Associates roles with permissions |

The system should distinguish platform-level roles from organization-level roles.

The exact database constraints and role-assignment rules should be finalized during schema implementation.

## 9.5 Example user records

| User | Role | Organization |
| --- | --- | --- |
| Platform Admin | Super Admin | NULL |
| John | Client Admin | ORG001 |
| Mary | Accountant | ORG001 |
| Peter | Client Admin | ORG002 |

John and Mary belong to ORG001.

Peter belongs to ORG002.

The platform Super Admin has platform-level authority.

## 9.6 Permission examples

Potential permissions include:

```text
organizations.view
organizations.create
organizations.update
organizations.suspend

clients.view
clients.create
clients.update
clients.revoke_credentials

transactions.view
transactions.export
transactions.view_details

users.view
users.create
users.update
users.disable

roles.view
roles.create
roles.update
roles.assign

payments.view
payments.initiate
payments.configure

audit_logs.view
```

These are examples of permission identifiers, not a final fixed list.

The application should allow appropriate permission combinations for organization-defined roles.

# 10. Merchant Client Management

A merchant client is an external website or application that connects to MPESA APP.

An organization may operate multiple merchant clients.

For example:

```text
Organization A
    |
    +--- Online Store
    |
    +--- School Website
    |
    +--- Booking Platform
    |
    +--- Customer Portal
```

Each merchant client belongs to one organization.

## 10.1 Merchant registration

Authorized organization users should be able to register a merchant website or application.

The registration process should capture relevant information such as:

- Merchant/client name.
- Organization ID.
- Website or application identifier.
- Callback or notification configuration.
- Merchant status.
- Integration settings.

The exact fields will be determined during database design.

## 10.2 Merchant API credentials

Each merchant should have its own API credentials.

The credentials allow MPESA APP to identify which merchant is making a request.

A merchant should not be able to use another merchant's credentials to access another organization's information.

The system should support:

- Credential generation.
- Secure credential storage.
- Credential rotation.
- Credential revocation.
- Credential status management.

Secret credentials must not be exposed in browser-side JavaScript or committed to Git.

# 11. API Architecture

The API is the communication interface between external merchant websites and MPESA APP.

The API should be versioned to support future changes without unnecessarily breaking existing integrations.

Example:

```text
/api/v1/
```

## 11.1 Proposed API endpoints

| Method | Endpoint | Purpose |
| --- | --- | --- |
| POST | `/api/v1/stk/push` | Initiate an STK Push |
| POST | `/api/v1/callbacks/safaricom` | Receive Safaricom callback |
| GET | `/api/v1/transactions/{id}` | Retrieve an authorized transaction |
| GET | `/api/v1/transactions` | Retrieve authorized transaction history |

These are proposed endpoint examples.

The final API contract must specify:

- Authentication.
- Required request fields.
- Optional request fields.
- Validation rules.
- Response structure.
- Error codes.
- Transaction references.
- Idempotency behavior.
- Authorization rules.

## 11.2 API request flow

A typical merchant request follows this process:

1. The merchant sends an HTTP request.
2. The API receives the request.
3. Authentication middleware identifies the merchant.
4. The system identifies the merchant's organization.
5. Request validation checks the submitted data.
6. The controller calls the relevant service.
7. The service performs the business operation.
8. The result is stored where appropriate.
9. The API returns a structured response.

## 11.3 API response consistency

API responses should use a consistent format.

For example, a successful response could contain:

```json
{
  "success": true,
  "message": "STK Push request accepted",
  "data": {
    "request_id": "REQUEST_REFERENCE",
    "checkout_request_id": "CHECKOUT_REFERENCE",
    "status": "PENDING"
  }
}
```

An error response could contain:

```json
{
  "success": false,
  "message": "Invalid payment request",
  "error": {
    "code": "VALIDATION_ERROR"
  }
}
```

These are illustrative response structures. Final field names and status semantics must be standardized in the API specification.

# 12. Authentication and Authorization

The application must distinguish authentication from authorization.

**Authentication** answers:

"Who is making this request?"

**Authorization** answers:

"What is this authenticated user or merchant allowed to do?"

## 12.1 User authentication

Dashboard users should authenticate using secure login credentials.

The system should:

- Verify passwords securely.
- Hash passwords using an appropriate password-hashing algorithm.
- Manage sessions securely.
- Support logout.
- Protect authenticated routes.
- Handle expired sessions.

## 12.2 Merchant authentication

External merchant applications should authenticate using their assigned API credentials.

The gateway should identify the merchant and derive the organization from the authenticated merchant record.

## 12.3 Authorization

The application should verify:

- The user is authenticated.
- The user has the required permission.
- The user belongs to the correct organization.
- The requested record belongs to that organization.
- The operation is permitted for the user's role.

A user must not gain access to another organization's records by changing a transaction ID, organization ID or URL parameter.

# 13. M-Pesa Payment Integration

MPESA APP communicates with Safaricom through the Daraja API.

The integration is responsible for initiating STK Push requests and receiving payment outcomes.

The payment integration should be isolated from controllers so that it can be maintained independently.

## 13.1 Daraja integration responsibilities

The integration layer should handle:

- Authentication with Daraja.
- Access-token management.
- STK Push request construction.
- Request transmission.
- Safaricom response processing.
- Error handling.
- Callback configuration.
- Communication logging without exposing secrets.

## 13.2 Configuration

Safaricom configuration should be stored using environment variables.

Examples of configuration categories include:

- Consumer key.
- Consumer secret.
- Business shortcode.
- Passkey.
- Transaction type.
- Callback URL.
- Environment selection.

Actual credentials must never be written directly into committed source files.

# 14. STK Push Request Processing

The STK Push process begins when a merchant website requests a payment.

## 14.1 Complete STK Push flow

```text
Merchant Website
       |
       v
POST /api/v1/stk/push
       |
       v
Merchant Authentication
       |
       v
Organization Identification
       |
       v
Request Validation
       |
       v
Create STK Request Record
       |
       v
STK Push Service
       |
       v
Safaricom Daraja
       |
       v
Safaricom Response
       |
       v
Update STK Request Status
       |
       v
Return API Response
```

## 14.2 Processing responsibilities

The system should:

1. Authenticate the merchant.
2. Identify the organization.
3. Validate the phone number.
4. Validate the amount.
5. Validate the merchant reference.
6. Create an STK request record.
7. Communicate with Daraja.
8. Store the Safaricom response.
9. Return the relevant request reference to the merchant.

The STK request record allows the application to track the initiation attempt independently of the eventual payment outcome.

## 14.3 Possible request states

The system may use states such as:

- RECEIVED
- VALIDATING
- INITIATED
- FAILED
- ACCEPTED
- UNKNOWN

The final state model must distinguish between an STK request being accepted by Safaricom and the actual payment being completed.

An accepted STK Push request does not mean the payment has succeeded.

# 15. Safaricom Callback Processing

Safaricom sends callback information to MPESA APP after the customer completes or abandons the payment process.

The callback processor is responsible for receiving and interpreting that information.

## 15.1 Callback processing flow

```text
Safaricom
    |
    v
Callback Endpoint
    |
    v
Receive Callback
    |
    v
Preserve Original Payload
    |
    v
Identify STK Request
    |
    v
Validate and Interpret Result
    |
    v
Update Transaction Records
    |
    v
Record Processing Outcome
    |
    v
Trigger Merchant Webhook
```

## 15.2 Callback responsibilities

The callback processor should:

- Receive Safaricom callback requests.
- Preserve the original payload.
- Associate callbacks with the relevant STK request.
- Interpret the payment result.
- Record the transaction outcome.
- Prevent duplicate processing.
- Record processing errors.
- Allow recovery of failed processing attempts.
- Trigger merchant notification where appropriate.

## 15.3 Duplicate callback protection

Safaricom may send repeated callback notifications.

The application must not create duplicate financial transaction records.

Use suitable identifiers, database constraints and idempotent processing logic.

A callback should be safe to process more than once without creating duplicate transactions or repeating unintended side effects.

## 15.4 Callback recovery

The application should support recovery of callbacks that could not be processed successfully.

A recovery mechanism may:

1. Identify callbacks awaiting processing.
2. Retrieve the original protected callback payload.
3. Retry processing.
4. Record the retry result.
5. Prevent duplicate transaction creation.

The callback recovery mechanism must distinguish between a temporary failure and a permanent processing error.

# 16. Merchant Webhook Processing

Merchant webhooks are separate from Safaricom callbacks.

This distinction is important.

## 16.1 Safaricom callback

```text
Safaricom ---> MPESA APP
```

Safaricom sends payment outcome information to the gateway.

## 16.2 Merchant webhook

```text
MPESA APP ---> Merchant Website
```

MPESA APP sends the processed payment outcome to the merchant website.

## 16.3 Merchant notification flow

```text
Safaricom Callback
        |
        v
Callback Processor
        |
        v
Transaction Updated
        |
        v
Merchant Webhook Service
        |
        v
Merchant Notification Request
        |
        v
Merchant Website
        |
        v
Merchant Order Updated
```

## 16.4 Webhook responsibilities

The webhook system should:

- Identify the correct merchant.
- Retrieve the configured notification endpoint.
- Construct a consistent notification payload.
- Send the notification.
- Record delivery attempts.
- Record successful delivery.
- Retry eligible failures.
- Prevent duplicate notification side effects where possible.

Webhook signing or another verification mechanism should be considered so that merchant websites can verify that a notification genuinely originated from MPESA APP.

## 16.5 Why webhooks need their own records

A payment can be successfully recorded in MPESA APP even if the merchant website is temporarily unavailable.

The system must preserve the payment outcome and retry merchant notification independently.

A merchant webhook failure must not reverse a successfully recorded payment.

# 17. STK Requests and Transactions

The `stk_requests` and `transactions` tables must be separate.

They represent different stages of the payment process.

## 17.1 STK Requests

The `stk_requests` table stores payment initiation attempts.

It answers:

"Did we attempt to initiate an STK Push, and what happened during initiation?"

Potential information includes:

- Request ID.
- Organization ID.
- Merchant client ID.
- Merchant request reference.
- Phone number.
- Amount.
- Account reference.
- Checkout request ID.
- Merchant request ID.
- Request status.
- Safaricom response details.
- Creation timestamp.
- Update timestamp.

An STK request can fail before an actual payment transaction exists.

## 17.2 Transactions

The `transactions` table stores the resulting payment transaction and its outcome.

It answers:

"What payment transaction was recorded, and what is its payment status?"

Potential information includes:

- Transaction ID.
- Organization ID.
- Merchant client ID.
- Related STK request ID.
- Safaricom receipt number.
- Amount.
- Phone number.
- Payment status.
- Merchant reference.
- Transaction timestamp.
- Creation timestamp.
- Update timestamp.

## 17.3 Why the separation matters

Consider three scenarios:

**Scenario A: STK initiation fails**

The application creates an STK request record, but Safaricom rejects the initiation.

There may be no resulting payment transaction.

**Scenario B: STK initiation succeeds, but payment fails**

The STK request is accepted, the customer does not complete a successful payment, and the callback communicates the unsuccessful outcome.

The system can retain the initiation record and the relevant outcome information according to the finalized transaction-recording policy.

**Scenario C: Payment succeeds**

The STK request is accepted, the customer completes payment, Safaricom sends a successful callback and the application records the resulting transaction.

Keeping the two entities separate makes reporting and reconciliation more accurate.

# 18. Merchant Dashboard

The dashboard is a core component of MPESA APP.

It provides a browser-based interface for managing the gateway and monitoring payment activity.

## 18.1 Dashboard users

The dashboard supports:

- Super Admin.
- Client Admin.
- Organization-defined roles with dashboard permissions.

Each user should see only the information and actions permitted by their role and organization.

## 18.2 Dashboard overview

The overview should display useful payment information, such as:

- Total transactions.
- Successful transactions.
- Failed transactions.
- Pending transactions.
- Total successful payment amount.
- Recent transactions.
- Payment activity over time.

All organization-level metrics must be scoped to the authenticated user's organization.

The Super Admin may have platform-wide metrics.

## 18.3 Transaction management

Authorized users should be able to:

- View transactions.
- Search by transaction identifier.
- Search by Safaricom receipt number.
- Search by merchant reference.
- Filter by payment status.
- Filter by merchant client.
- Filter by date range.
- Sort transaction records.
- View transaction details.
- Inspect relevant payment processing information.

## 18.4 Merchant management

Authorized users should be able to:

- View merchant clients.
- Register merchant clients.
- Update merchant information.
- Activate or deactivate merchants.
- Manage merchant credentials.
- Configure merchant webhook settings.
- Review merchant-specific payment activity.

## 18.5 User management

Client Admins should be able to:

- Create organization users.
- Update user information.
- Disable users.
- Assign roles.
- Manage permitted permissions.

The application should prevent organization administrators from creating or modifying platform Super Admin accounts.

## 18.6 Future dashboard functionality

Future features may include:

- Transaction exports.
- Pagination.
- Advanced analytics.
- Reconciliation reports.
- Merchant performance reports.
- Audit history.
- Notification delivery history.

# 19. Complete Database Architecture

The application will use one centralized MySQL database.

Organizations will share database tables, with organization ownership represented through `organization_id`.

The following is the proposed logical database design.

## 19.1 Core organization and user tables

| Table | Responsibility |
| --- | --- |
| `organizations` | Stores registered organizations |
| `users` | Stores platform and organization users |
| `roles` | Stores platform and organization-defined roles |
| `permissions` | Stores available system permissions |
| `user_roles` | Associates users with roles |
| `role_permissions` | Associates roles with permissions |

## 19.2 Merchant tables

| Table | Responsibility |
|---|---|
| `clients` | Stores merchant websites and applications |

Each client belongs to an organization.

## 19.3 Payment tables

| Table | Responsibility |
| --- | --- |
| `stk_requests` | Stores STK Push initiation attempts |
| `transactions` | Stores resulting payment transaction records |
| `callbacks` | Stores received Safaricom callback records |
| `merchant_webhooks` | Stores merchant notification records and delivery attempts |

## 19.4 Operational tables

| Table | Responsibility |
|---|---|
| `audit_logs` | Records important administrative activities |

Additional tables may be introduced for webhook attempts, credential history, notifications, reconciliation or other functionality when justified by the implementation.

## 19.5 Database design principles

The database should use:

- Primary keys.
- Foreign keys.
- Unique constraints.
- Appropriate indexes.
- Consistent data types.
- Timestamps.
- Organization ownership fields.
- Appropriate deletion and update rules.
- Database transactions for related operations.

## 19.6 Suggested indexes

Frequently searched fields may require indexes.

Examples include:

- `organization_id`.
- `client_id`.
- `transaction_id`.
- `checkout_request_id`.
- `merchant_request_id`.
- `receipt_number`.
- `status`.
- `created_at`.

The final indexes must be determined from actual query patterns and uniqueness requirements.

## 19.7 Important database constraints

The database should enforce important integrity rules.

Examples:

- A merchant client must belong to an existing organization.
- An organization user must belong to a valid organization.
- A transaction must reference a valid organization and relevant request where applicable.
- Duplicate Safaricom receipt numbers must be prevented where the business rules require uniqueness.
- A user-role assignment should not be duplicated.
- A role-permission assignment should not be duplicated.

The exact unique constraints must account for the identifiers' actual scope.

# 20. Database Relationships and Data Ownership

The following diagram illustrates the intended logical relationships.

```text
organizations
    |
    +---- users
    |       |
    |       +---- user_roles
    |                 |
    |                 +---- roles
    |                          |
    |                          +---- role_permissions
    |                                      |
    |                                      +---- permissions
    |
    +---- clients
    |       |
    |       +---- stk_requests
    |       |          |
    |       |          +---- transactions
    |       |
    |       +---- merchant_webhooks
    |
    +---- transactions
    |
    +---- callbacks
    |
    +---- audit_logs
```

This is a conceptual diagram. Actual foreign keys and cardinalities should be defined in the migrations.

## 20.1 Organization ownership

Every organization-owned record must have a reliable ownership path.

For example:

- A client belongs to an organization.
- An STK request belongs to an organization and merchant client.
- A transaction belongs to an organization and merchant client.
- A user belongs to an organization unless the account is platform-level.
- Organization-level roles belong to their organization where applicable.

## 20.2 Tenant-safe queries

Application queries must enforce organization ownership.

For example:

```sql
SELECT *
FROM clients
WHERE organization_id = ?;
```

For transactions:

```sql
SELECT *
FROM transactions
WHERE organization_id = ?
ORDER BY created_at DESC;
```

The organization ID must be obtained from authenticated server-side context.

A user must never be able to bypass tenant isolation by modifying a request parameter.

---

# PART B — APPLICATION STRUCTURE AND DEVELOPER REFERENCE

# 21. Application Directory Structure

The current project skeleton is:

```text
MPESA APP/
│
├── .env.example
├── .gitignore
├── README.md
│
├── app/
│   ├── Http/
│   │   ├── Controllers/
│   │   │   └── .gitkeep
│   │   │
│   │   ├── Middleware/
│   │   │   └── .gitkeep
│   │   │
│   │   └── Requests/
│   │       └── .gitkeep
│   │
│   ├── Models/
│   │   └── .gitkeep
│   │
│   ├── Services/
│   │   └── .gitkeep
│   │
│   └── Support/
│       └── .gitkeep
│
├── bootstrap/
│   └── app.php
│
├── config/
│   ├── app.php
│   ├── database.php
│   ├── mail.php
│   └── mpesa.php
│
├── database/
│   ├── migrations/
│   │   └── .gitkeep
│   │
│   └── seeders/
│       └── .gitkeep
│
├── public/
│   └── index.php
│
├── routes/
│   ├── api.php
│   └── web.php
│
├── storage/
│   ├── callbacks/
│   │   └── .gitkeep
│   │
│   └── logs/
│       └── .gitkeep
│
└── tests/
    ├── Feature/
    │   └── .gitkeep
    │
    └── Unit/
        └── .gitkeep
```

The `.gitkeep` files are placeholders that allow Git to retain empty directories.

The directory structure represents the initial application foundation. Additional subdirectories and files should be introduced as implementation requires them.

# 22. Detailed Explanation of Every Folder and File

This section explains the purpose of every existing directory and file, why it exists and where new functionality should be placed.

## 22.1 Root directory

The root directory contains the main project-level files.

### `.env.example`

This file documents environment variables required by the application.

It may contain variable names and safe example values for:

- Application environment.
- Application URL.
- Database host.
- Database name.
- Database username.
- Database password placeholder.
- Safaricom Daraja configuration.
- Mail configuration.
- Encryption configuration.

It must not contain real production credentials.

A developer uses it as a template when creating a local `.env` file.

### `.gitignore`

Defines files and directories that Git should not track.

Examples of files that should generally be excluded include:

- `.env`.
- Private credentials.
- Temporary files.
- Runtime logs.
- Local cache files.
- Private callback archives.
- Local dependencies where they are reproducible from dependency manifests.

The actual rules should reflect the project's dependency and deployment strategy.

### `README.md`

The main project documentation.

It explains:

- What the application does.
- Why it exists.
- How its architecture works.
- How the database is organized.
- How the API works.
- How payments are processed.
- What each folder and file does.
- How developers can extend the application.
- How to test and deploy the system.
- What concepts to study when working offline.

It should be updated whenever important architecture or implementation decisions change.

## 22.2 `app/`

This is the main application code directory.

It contains the controllers, middleware, request validation, models, services and support utilities.

The purpose of separating these components is to prevent the application from becoming one large collection of unrelated PHP files.

### `app/Http/Controllers/`

Controllers receive and coordinate HTTP requests.

A controller should:

1. Receive a request.
2. Obtain validated input.
3. Call the appropriate service.
4. Handle the result.
5. Return a response or render a page.

Potential future organization:

```text
Controllers/
│
├── Api/
│   ├── StkController.php
│   ├── TransactionController.php
│   └── CallbackController.php
│
├── Dashboard/
│   ├── DashboardController.php
│   ├── TransactionController.php
│   ├── ClientController.php
│   └── UserController.php
│
└── Admin/
    ├── OrganizationController.php
    ├── PlatformController.php
    └── AuditLogController.php
```

These are proposed future files, not files that currently exist.

**Why controllers matter:**

Without controllers, request-handling logic can become mixed with database queries, validation and business logic.

Controllers provide a clear entry point for application operations.

### `app/Http/Middleware/`

Middleware processes requests before they reach the relevant controller.

Potential middleware:

```text
Middleware/
│
├── Authenticate.php
├── MerchantAuthentication.php
├── OrganizationAccess.php
├── CheckPermission.php
└── RateLimit.php
```

Responsibilities include:

- Checking whether a dashboard user is authenticated.
- Authenticating merchant API requests.
- Establishing trusted organization context.
- Checking permissions.
- Applying request restrictions.

Middleware is especially important for protecting organization boundaries.

### `app/Http/Requests/`

Contains request validation classes.

Potential future files:

```text
Requests/
│
├── StkPushRequest.php
├── MerchantRegistrationRequest.php
├── UserRegistrationRequest.php
└── TransactionSearchRequest.php
```

These classes or equivalent validation mechanisms ensure that incoming data meets application requirements before business processing begins.

For example, an STK Push request should validate:

- Required phone number.
- Valid amount.
- Merchant reference.
- Required account reference.
- Allowed request format.

### `app/Models/`

Models represent database entities.

Potential models include:

```text
Models/
│
├── User.php
├── Organization.php
├── Role.php
├── Permission.php
├── Client.php
├── StkRequest.php
├── Transaction.php
├── Callback.php
└── MerchantWebhook.php
```

Models help organize:

- Database queries.
- Relationships.
- Entity-level behavior.
- Data access.

For example, a `Transaction` model represents a transaction record and can provide a consistent way to retrieve or update transaction information.

### `app/Services/`

Services contain the main business logic.

This is where operations that are more complicated than simple request handling should live.

Suggested organization:

```text
Services/
│
├── Mpesa/
│   ├── DarajaService.php
│   ├── StkPushService.php
│   └── CallbackProcessor.php
│
├── Transactions/
│   ├── TransactionService.php
│   └── TransactionStatusService.php
│
├── Merchants/
│   ├── MerchantService.php
│   └── MerchantWebhookService.php
│
├── Dashboard/
│   └── DashboardService.php
│
└── Security/
    ├── ApiKeyService.php
    └── EncryptionService.php
```

These are suggested files for future implementation.

**DarajaService.php**

Responsible for communication with Safaricom Daraja, including authentication and API requests.

**StkPushService.php**

Coordinates STK Push initiation and the creation or update of STK request records.

**CallbackProcessor.php**

Processes Safaricom callback information and coordinates payment outcome recording.

**TransactionService.php**

Handles transaction-related business operations.

**MerchantService.php**

Handles merchant client management.

**MerchantWebhookService.php**

Handles merchant notification delivery and retry management.

**DashboardService.php**

Provides dashboard-specific business data, summaries and statistics.

**ApiKeyService.php**

Handles merchant API credential operations.

**EncryptionService.php**

Provides reusable encryption and decryption functionality where required.

### `app/Support/`

Contains shared utilities that do not belong to one particular business service.

Potential contents include:

```text
Support/
│
├── ApiResponse.php
├── Constants.php
├── DateHelper.php
└── Helpers.php
```

Use this directory for genuinely reusable support functionality.

Do not place every function here merely because it is convenient.

## 22.3 `bootstrap/`

This directory contains application startup logic.

### `bootstrap/app.php`

This file initializes the application.

Depending on the chosen architecture, it may:

- Load environment configuration.
- Load application configuration.
- Register routes.
- Initialize shared services.
- Prepare request handling.
- Set up error handling.

The application entry point should use the bootstrap process rather than duplicating initialization logic.

## 22.4 `config/`

This directory contains configuration files.

### `config/app.php`

Contains general application settings, such as:

- Application name.
- Application environment.
- Application URL.
- Debug configuration.
- Timezone.
- General application options.

### `config/database.php`

Contains database connection configuration.

It should read credentials from environment variables.

It should not contain committed production passwords.

### `config/mail.php`

Contains email-related configuration.

This may later be used for:

- User invitations.
- Password reset notifications.
- Administrative notifications.
- Operational alerts.

### `config/mpesa.php`

Contains Safaricom integration configuration.

It may define:

- Daraja environment.
- API base URL.
- Consumer key.
- Consumer secret.
- Shortcode.
- Passkey.
- Transaction type.
- Callback URL.
- Other integration settings.

Sensitive values should be loaded from the environment.

## 22.5 `database/`

This directory contains database structure and initial data definitions.

### `database/migrations/`

Migrations define database changes in a controlled and repeatable way.

Examples:

- Create organizations.
- Create users.
- Create roles.
- Create permissions.
- Create clients.
- Create STK requests.
- Create transactions.
- Create callbacks.
- Create merchant webhook records.

Migrations make it easier to reproduce the database structure on another development machine or production environment.

They should also define appropriate indexes, foreign keys and constraints.

### `database/seeders/`

Seeders insert initial records.

Examples:

- Initial platform permissions.
- Initial platform roles.
- Development test organizations.
- Development test users.
- Default configuration records.

Production seeding must be controlled carefully.

Real credentials and private user information should never be embedded in seed files.

## 22.6 `public/`

This directory contains files accessible through the web server.

### `public/index.php`

The main HTTP entry point.

Incoming browser and API requests should enter the application through this file.

It should initialize the application and delegate request processing to the appropriate routing and application components.

The web server document root should point to `public/`.

The following directories must not be directly exposed as public web content:

- `app/`
- `bootstrap/`
- `config/`
- `database/`
- `storage/`

The `.env` file must also remain private.

## 22.7 `routes/`

This directory defines application routes.

### `routes/api.php`

Contains API routes used by external merchant websites and Safaricom.

Examples:

```text
POST /api/v1/stk/push
POST /api/v1/callbacks/safaricom
GET  /api/v1/transactions/{id}
GET  /api/v1/transactions
```

These are proposed endpoints.

The routes should connect requests to the correct API controllers and middleware.

### `routes/web.php`

Contains browser-facing routes.

Examples:

```text
/login
/logout
/dashboard
/transactions
/transactions/{id}
/organizations
/clients
/users
/roles
/settings
```

These routes should be protected according to authentication and permission requirements.

## 22.8 `storage/`

Contains private application-generated files.

It should not be directly accessible through the public web server.

### `storage/callbacks/`

Can store protected callback archives or other callback recovery data.

The callback system should preserve original callback information so that processing can be investigated and recovered when necessary.

Sensitive information should be encrypted or otherwise protected according to the data classification and security design.

### `storage/logs/`

Contains application logs.

Examples:

- Application errors.
- Database errors.
- Payment processing errors.
- Callback processing errors.
- Merchant webhook failures.
- Authentication and authorization events.

Logs should be useful for troubleshooting without exposing passwords, API secrets or unnecessary sensitive customer information.

## 22.9 `tests/`

Contains automated tests.

### `tests/Unit/`

Contains tests for individual classes and functions.

Examples:

- Transaction status interpretation.
- Request validation.
- Permission checking.
- Merchant credential verification.
- Webhook signature generation.

### `tests/Feature/`

Contains tests for complete application workflows.

Examples:

- User login.
- Merchant authentication.
- Merchant registration.
- STK Push initiation.
- Safaricom callback processing.
- Transaction retrieval.
- Organization isolation.
- Merchant webhook delivery.

## 22.10 How the directories fit together

A typical request may pass through the following structure:

```text
public/index.php
       |
       v
bootstrap/app.php
       |
       v
routes/api.php or routes/web.php
       |
       v
Middleware
       |
       v
Request Validation
       |
       v
Controller
       |
       v
Service
       |
       v
Model / Database
       |
       v
Controller Response
```

Not every request needs every component.

For example, a static health-check endpoint may not require a model or service.

However, payment and administrative operations should follow a consistent structure.

---

# PART C — DEVELOPER LEARNING AND OFFLINE IMPLEMENTATION MANUAL

# 23. How the Application Components Communicate

Understanding the relationship between components is essential for developing new features independently.

The following example demonstrates the flow of an STK Push request.

## 23.1 STK Push request communication

```text
Merchant Website
       |
       | HTTP POST
       v
public/index.php
       |
       v
bootstrap/app.php
       |
       v
routes/api.php
       |
       v
MerchantAuthentication Middleware
       |
       v
StkPushRequest Validation
       |
       v
StkController
       |
       v
StkPushService
       |
       +------> StkRequest Model
       |                |
       |                v
       |             MySQL
       |
       v
DarajaService
       |
       v
Safaricom API
       |
       v
Response
       |
       v
StkPushService
       |
       v
Controller
       |
       v
Merchant API Response
```

The important lesson is that each component has a specific job.

The controller should not be responsible for implementing every part of the payment integration.

## 23.2 Callback communication

```text
Safaricom
    |
    v
public/index.php
    |
    v
routes/api.php
    |
    v
CallbackController
    |
    v
CallbackProcessor
    |
    +------> Callback Model
    |
    +------> StkRequest Model
    |
    +------> Transaction Model
    |
    +------> MerchantWebhookService
    |
    v
Processing Result
```

## 23.3 Dashboard communication

```text
Browser
   |
   v
public/index.php
   |
   v
routes/web.php
   |
   v
Authentication Middleware
   |
   v
Permission Middleware
   |
   v
DashboardController
   |
   v
DashboardService
   |
   v
Models
   |
   v
MySQL
   |
   v
Dashboard Response
```

# 24. Security Architecture

Security must be designed into the application rather than added only after the main features have been implemented.

## 24.1 Password security

Passwords must never be stored in plain text.

Use PHP's password hashing and verification facilities.

The application should support:

- Secure password hashing.
- Password verification.
- Session protection.
- Logout.
- Appropriate password reset procedures.

## 24.2 Merchant API credentials

Merchant credentials identify external merchant applications.

The system should:

- Generate credentials securely.
- Store secret material using an appropriate secure storage strategy.
- Avoid returning secrets unnecessarily.
- Support rotation.
- Support revocation.
- Record relevant credential management activities.

Where possible, secret API credentials should be stored in a form that allows verification without retaining the original secret.

## 24.3 Organization isolation

All organization-owned queries must be scoped to the authenticated organization.

For example:

```php
$organizationId = $authenticatedUser->organization_id;
```

The application should not use an organization ID supplied by the browser as the sole authorization check.

## 24.4 SQL injection prevention

Use prepared statements or a secure database abstraction.

Avoid building SQL queries by directly concatenating untrusted user input.

## 24.5 Input validation

Validate all incoming data.

Validation should cover:

- Data type.
- Required fields.
- Allowed values.
- Length.
- Numeric ranges.
- Identifiers.
- Request format.

Validation does not replace authorization.

A valid transaction ID does not mean the user is allowed to view the transaction.

## 24.6 Browser security

Dashboard operations should account for:

- Session management.
- CSRF protection.
- Secure cookies.
- Output escaping.
- Authorization.
- HTTPS.

## 24.7 Callback security

Callback endpoints should be designed according to Safaricom's callback contract and the security controls available for the integration.

Do not assume that an incoming request is trustworthy merely because it claims to be a callback.

Where supported, use appropriate request verification and correlate callbacks with existing payment requests.

## 24.8 Rate limiting

The application should apply appropriate rate limits to sensitive endpoints.

Examples:

- Login.
- Merchant API requests.
- STK Push initiation.
- Credential-related endpoints.

Rate limits should be designed around legitimate merchant traffic and operational requirements.

## 24.9 Secret management

Never commit:

- Database passwords.
- Daraja consumer secrets.
- Passkeys.
- Merchant API secrets.
- Encryption keys.
- Production environment files.

Use environment variables and appropriate protected secret storage.

# 25. Reliability, Error Handling and Recovery

A payment gateway must account for communication failures and interruptions.

The system should not assume that every network request succeeds.

## 25.1 Possible failures

Examples include:

- Daraja timeout.
- Invalid STK Push request.
- Database connection failure.
- Safaricom callback delay.
- Duplicate callback.
- Callback processing failure.
- Merchant webhook timeout.
- Merchant website unavailable.
- Application restart.
- Temporary network interruption.

## 25.2 Error handling principles

The application should:

1. Detect errors.
2. Record useful diagnostic information.
3. Preserve relevant original payment information.
4. Avoid creating duplicate transaction records.
5. Retry operations where retrying is safe.
6. Distinguish temporary errors from permanent failures.
7. Provide clear operational visibility.

## 25.3 Idempotency

Idempotency means that repeating the same operation does not create unintended duplicate effects.

This is particularly important for:

- STK Push requests.
- Safaricom callbacks.
- Transaction recording.
- Merchant webhook delivery.

For example, processing the same successful callback twice must not create two payment records for the same payment.

## 25.4 Database transactions

Related database changes should be grouped into appropriate database transactions.

For example, when processing a callback, the application may need to:

- Record the callback processing result.
- Update the STK request.
- Create or update the transaction.
- Record an audit event.

The exact grouping must be designed so that partial failures do not leave inconsistent payment data.

External HTTP requests should not be treated as though they participate in the MySQL transaction.

Merchant webhook delivery should be managed separately from the database operation that records the payment.

## 25.5 Retry strategy

Retries should be controlled.

The system should consider:

- Maximum attempts.
- Retry intervals.
- Failure classification.
- Duplicate prevention.
- Final failure status.
- Manual investigation.

A failed merchant notification should not change a successfully recorded payment into a failed payment.

# 26. Logging and Monitoring

Logs are essential for understanding what happens inside the payment gateway.

## 26.1 Application logs

Record relevant application errors and operational events.

## 26.2 Database logs

Record database-related failures with enough context for troubleshooting.

Do not expose database passwords or sensitive connection information.

## 26.3 Payment logs

Record useful payment-processing events, such as:

- STK request creation.
- Daraja response.
- Callback reception.
- Callback processing result.
- Transaction update.
- Merchant notification result.

Sensitive fields should be excluded or protected.

## 26.4 Audit logs

Audit logs should record important administrative actions.

Examples:

- Organization created.
- Organization suspended.
- Merchant registered.
- Merchant credentials rotated.
- User created.
- Role assigned.
- Permission changed.

An audit record should identify the actor, action, relevant resource and timestamp.

## 26.5 Monitoring

Future monitoring should provide visibility into:

- Application availability.
- API response errors.
- Payment initiation failures.
- Callback processing failures.
- Webhook delivery failures.
- Database availability.
- Queue or recovery backlog where applicable.

# 27. Development Environment and Configuration

The application is being developed as a PHP and MySQL project.

The development environment may use:

- Visual Studio Code.
- PHP.
- MySQL.
- A local development web server.
- Git.
- Composer where dependencies require it.
- Safaricom Daraja sandbox credentials.

## 27.1 Independent development database

The new MPESA APP must use a new database.

The existing production M-Pesa database must not be used for development migrations or testing.

This separation prevents accidental changes to the operational application.

## 27.2 Environment configuration

The `.env.example` file documents the required variables.

The developer creates a local `.env` file with actual development values.

The `.env` file must remain excluded from Git.

## 27.3 Configuration separation

Different environments should use different credentials and settings.

For example:

- Local development.
- Testing.
- Staging.
- Production.

Production credentials must not be reused casually in development.

# 28. API Documentation Standards

Every API endpoint should have documented behavior.

For each endpoint, document:

1. Endpoint URL.
2. HTTP method.
3. Authentication requirement.
4. Required permissions.
5. Required request headers.
6. Request body.
7. Validation rules.
8. Successful response.
9. Error responses.
10. Side effects.
11. Idempotency behavior.
12. Rate limits where applicable.

## 28.1 Example STK Push endpoint documentation

**Endpoint:**

```text
POST /api/v1/stk/push
```

**Purpose:**

Allows an authenticated merchant website to request an M-Pesa STK Push.

**Authentication:**

Requires valid merchant API credentials.

**Processing:**

- Authenticate merchant.
- Resolve organization.
- Validate request.
- Record STK request.
- Initiate Daraja request.
- Store response.
- Return request reference.

**Important distinction:**

An accepted STK Push request does not confirm successful payment.

Payment success must be established from the appropriate verified payment outcome.

# 29. Testing Strategy

Testing should happen throughout development.

A payment gateway should not rely solely on manually checking whether pages load.

## 29.1 Unit tests

Unit tests check individual classes or functions.

Examples:

- Request validation.
- Transaction status interpretation.
- Permission checking.
- Merchant credential verification.
- Idempotency logic.
- Webhook payload construction.

## 29.2 Feature tests

Feature tests check complete workflows.

Examples:

- User login.
- Merchant registration.
- Merchant authentication.
- STK Push initiation.
- Callback processing.
- Transaction retrieval.
- Dashboard filtering.
- Merchant webhook delivery.

## 29.3 Organization isolation tests

These tests are particularly important.

Test that:

- ORG001 can access its own transactions.
- ORG001 cannot access ORG002 transactions.
- ORG001 cannot modify ORG002 merchant clients.
- An organization user cannot access platform administration.
- A Client Admin cannot assign platform-level permissions.
- Super Admin can access authorized platform-wide information.

## 29.4 Payment tests

Test:

- Valid STK Push request.
- Invalid phone number.
- Invalid amount.
- Daraja rejection.
- Daraja timeout.
- Successful callback.
- Unsuccessful callback.
- Duplicate callback.
- Delayed callback.
- Callback processing failure.

## 29.5 Merchant webhook tests

Test:

- Successful delivery.
- Merchant endpoint unavailable.
- Timeout.
- Retry.
- Invalid webhook configuration.
- Repeated notification.
- Delivery status recording.

## 29.6 Regression testing

When adding a new feature, test existing features that could be affected.

For example, changing the transaction model should not break:

- Callback processing.
- Dashboard transaction listing.
- Merchant transaction queries.
- Organization isolation.

# 30. Deployment Architecture

The new MPESA APP will have its own deployment configuration.

Deployment should be separate from the existing production M-Pesa application.

## 30.1 Deployment requirements

The deployment environment should provide:

- PHP runtime.
- MySQL database.
- Web server.
- HTTPS.
- Private environment configuration.
- Writable private storage directories.
- Logging.
- Backup facilities.
- Safaricom callback reachability.

## 30.2 Deployment sequence

A recommended deployment sequence:

1. Prepare the production environment.
2. Create the independent production database.
3. Configure environment variables.
4. Configure the web server document root.
5. Set private directory permissions.
6. Install application dependencies.
7. Run database migrations.
8. Seed required initial records.
9. Configure Safaricom integration.
10. Register the callback URL.
11. Test merchant authentication.
12. Test STK Push initiation.
13. Test callback processing.
14. Test merchant notifications.
15. Test dashboard authorization.
16. Enable monitoring.
17. Confirm backup and recovery procedures.

## 30.3 Deployment isolation

Deployment of MPESA APP must not overwrite the existing production application.

The two applications should have separate:

- Application directories.
- Database configurations.
- Environment variables.
- Deployment workflows.
- Runtime configuration.

# 31. Backup and Disaster Recovery

The application should have a documented backup strategy.

Important data includes:

- Organizations.
- Users.
- Merchant clients.
- STK requests.
- Transactions.
- Callback records.
- Webhook delivery records.
- Audit logs.

The backup plan should define:

- Backup frequency.
- Backup retention.
- Backup storage.
- Access restrictions.
- Encryption where appropriate.
- Restoration procedures.
- Recovery testing.

A backup is only useful if it can be restored successfully.

Restoration should be tested in a separate environment before being relied upon during an incident.

---

# PART D — DEVELOPER IMPLEMENTATION AND OFFLINE LEARNING GUIDE

# 32. Development Workflow

The developer should follow a consistent process when adding functionality.

A feature should not be implemented by randomly creating files and inserting code until it appears to work.

Instead, use a structured workflow.

## 32.1 Understand the requirement

Start by describing what the feature should accomplish.

Example:

"An organization administrator should be able to register a new merchant website."

Determine:

- Who uses the feature?
- What should the user be able to do?
- What information is required?
- What should happen after submission?
- What permissions are required?
- What should happen if something fails?

## 32.2 Identify affected components

Determine which parts of the application need to change.

For example, merchant registration may involve:

- Database migration.
- Model.
- Request validation.
- Controller.
- Service.
- Route.
- Authentication middleware.
- Permission checks.
- Tests.

Not every feature needs every component.

## 32.3 Understand existing code

Before writing code:

- Inspect related files.
- Understand their responsibilities.
- Follow existing naming conventions.
- Check existing database relationships.
- Identify reusable services.
- Avoid duplicating existing functionality.

## 32.4 Design database changes

Determine:

- Whether a new table is necessary.
- Required columns.
- Primary key.
- Foreign keys.
- Unique constraints.
- Indexes.
- Organization ownership.
- Nullable fields.
- Default values.
- Timestamp requirements.

## 32.5 Implement business logic

Place the actual business operation in the appropriate service.

Keep controllers focused on HTTP request handling.

## 32.6 Connect the feature

Add:

- Route.
- Middleware.
- Validation.
- Controller action.
- Service call.
- Model or database operation.
- Appropriate response.

## 32.7 Test

Test:

- Expected successful behavior.
- Invalid input.
- Missing information.
- Unauthorized access.
- Cross-organization access.
- Failure conditions.

## 32.8 Document

Update the relevant documentation whenever the new feature changes:

- Database architecture.
- API behavior.
- Security rules.
- Folder structure.
- Application workflows.

# 33. How to Build New Features Independently

This section explains how to decide where a new feature belongs.

## 33.1 New browser page

If the feature introduces a browser page:

1. Define the page's purpose.
2. Identify who can access it.
3. Add the appropriate route in `routes/web.php`.
4. Create or update the relevant dashboard controller.
5. Create or update the service if business logic is required.
6. Retrieve information through the appropriate models.
7. Apply authorization.
8. Implement the page.
9. Test access and behavior.

## 33.2 New API endpoint

If the feature introduces an external API operation:

1. Define the endpoint.
2. Choose the HTTP method.
3. Define authentication.
4. Define request validation.
5. Define response structure.
6. Add the route to `routes/api.php`.
7. Create or update the API controller.
8. Implement the operation in a service.
9. Apply authorization and organization scoping.
10. Write API tests.
11. Document the endpoint.

## 33.3 New database feature

If the feature requires new database information:

1. Identify the entity.
2. Determine its relationship to existing entities.
3. Design its table.
4. Choose data types.
5. Define primary and foreign keys.
6. Add indexes.
7. Define uniqueness requirements.
8. Create a migration.
9. Create or update the model.
10. Test the database behavior.

## 33.4 New permission

If the feature requires a new permission:

1. Identify the protected action.
2. Define a permission identifier.
3. Register the permission in the appropriate system configuration or seeder.
4. Determine which roles can receive it.
5. Add the authorization check.
6. Test allowed access.
7. Test denied access.

## 33.5 New payment operation

If the feature changes payment processing:

1. Identify the payment stage affected.
2. Inspect the relevant STK or callback flow.
3. Determine database changes.
4. Implement the business logic in the payment service.
5. Preserve idempotency.
6. Add error handling.
7. Test successful and failed scenarios.
8. Test duplicate and delayed processing.

# 34. Practical Feature Development Examples

## 34.1 Example: Merchant registration

**Requirement:**

An authorized Client Admin should be able to register a new merchant website.

**Possible component flow:**

```text
Merchant Registration Form
          |
          v
routes/web.php
          |
          v
ClientController
          |
          v
MerchantRegistrationRequest
          |
          v
MerchantService
          |
          v
Client Model
          |
          v
clients Table
```

**Concepts to understand:**

- HTML forms.
- HTTP POST requests.
- PHP classes.
- Input validation.
- SQL INSERT.
- Prepared statements.
- Foreign keys.
- Organization authorization.
- API key generation.
- Secure credential storage.

**Tests:**

- Valid merchant registration.
- Missing merchant name.
- Invalid website information.
- Unauthorized user.
- Merchant assigned to the wrong organization.
- Duplicate merchant identifier.

## 34.2 Example: Transaction search

**Requirement:**

An authorized user should be able to search transactions.

**Possible component flow:**

```text
Dashboard Search
       |
       v
routes/web.php
       |
       v
TransactionController
       |
       v
TransactionService
       |
       v
Transaction Model
       |
       v
transactions Table
```

**Concepts to understand:**

- SQL SELECT.
- WHERE.
- LIKE.
- Prepared statements.
- ORDER BY.
- LIMIT.
- OFFSET.
- Pagination.
- Date filtering.
- Database indexes.

**Important security requirement:**

The organization restriction must be applied by the backend, regardless of what the browser submits.

## 34.3 Example: Adding an Accountant role

**Requirement:**

An organization wants to introduce an Accountant role.

The role should not require permanently hardcoding Accountant into the application.

Instead:

1. The authorized administrator creates the role.
2. The role is associated with the organization.
3. The administrator assigns permissions.
4. A user is assigned the role.
5. The authorization system checks the user's effective permissions.

**Possible component flow:**

```text
Role Management Page
        |
        v
RoleController
        |
        v
RoleService
        |
        v
Role Model
        |
        v
roles Table
        |
        v
role_permissions Table
        |
        v
Authorization Middleware
```

**Concepts to understand:**

- Many-to-many relationships.
- Database joins.
- Role-based access control.
- Permission assignment.
- Authorization middleware.
- Database transactions.

## 34.4 Example: Dashboard transaction totals

**Requirement:**

An organization wants to see the total number of successful payments.

The dashboard service should retrieve the correct records for the authenticated organization.

Example SQL:

```sql
SELECT COUNT(*) AS successful_transactions
FROM transactions
WHERE organization_id = ?
  AND status = 'SUCCESS';
```

**Concepts to understand:**

- SQL COUNT.
- WHERE.
- Aggregate functions.
- Database indexes.
- Query optimization.
- Tenant isolation.

## 34.5 Example: Merchant webhook retry

**Requirement:**

If a merchant website is temporarily unavailable, MPESA APP should retry delivery of the payment notification.

The implementation should:

1. Preserve the payment result.
2. Record the webhook delivery attempt.
3. Detect a failed delivery.
4. Determine whether the failure is retryable.
5. Schedule or perform a controlled retry.
6. Record the new delivery result.
7. Stop retrying after the configured policy is exhausted.

**Concepts to understand:**

- HTTP clients.
- HTTP response codes.
- Timeouts.
- Exception handling.
- Retry policies.
- Idempotency.
- Background jobs or scheduled tasks.
- Database status tracking.

# 35. Offline Programming and Study Guide

This project should remain understandable and developable even when the developer has no internet access or AI assistance.

The README is intended to act as a reference point while programming books and offline learning resources provide the deeper technical explanations.

The objective is to understand the system rather than blindly copy code.

## 35.1 PHP fundamentals

Study the following:

### Variables and data types

Understand:

- Variables.
- Strings.
- Integers.
- Floating-point numbers.
- Booleans.
- Arrays.
- Objects.
- Null values.

### Conditional statements

Understand:

- `if`.
- `else`.
- `elseif`.
- `switch`.

These are useful for checking payment statuses, permissions and validation results.

### Loops

Understand:

- `for`.
- `foreach`.
- `while`.
- `do while`.

These are useful for processing collections of records and retry operations.

### Functions

Understand:

- Function declarations.
- Parameters.
- Return values.
- Scope.
- Reusable functions.

### Object-oriented PHP

Study:

- Classes.
- Objects.
- Properties.
- Methods.
- Constructors.
- Visibility.
- Encapsulation.
- Inheritance.
- Interfaces.
- Abstract classes.
- Namespaces.
- Traits.
- Exceptions.

These concepts are important because the application uses models, controllers and services.

### PHP database interaction

Study:

- PDO.
- Prepared statements.
- Database connections.
- Query execution.
- Fetching records.
- Error handling.
- Transactions.

## 35.2 Object-oriented programming

Understand the major object-oriented principles.

**Encapsulation**

Keeping an object's internal data and operations organized behind a defined interface.

**Inheritance**

Allowing one class to inherit functionality from another where appropriate.

**Polymorphism**

Allowing different implementations to be used through a common interface.

**Abstraction**

Exposing essential functionality while hiding unnecessary implementation details.

**Interfaces**

Defining a contract that different classes can implement.

**Dependency injection**

Providing an object with the dependencies it needs rather than making it create every dependency internally.

**Separation of concerns**

Ensuring that different application components have different responsibilities.

For example:

- Controller: handles HTTP requests.
- Service: implements business operations.
- Model: represents and accesses database entities.
- Middleware: handles request-level checks.

## 35.3 MySQL and database design

Study:

### Database fundamentals

- Databases.
- Tables.
- Rows.
- Columns.
- Data types.
- Primary keys.
- Foreign keys.
- Unique constraints.
- NULL values.

### SQL queries

- SELECT.
- INSERT.
- UPDATE.
- DELETE.
- WHERE.
- ORDER BY.
- GROUP BY.
- HAVING.
- JOIN.
- COUNT.
- SUM.
- AVG.

### Database relationships

Understand:

- One-to-one.
- One-to-many.
- Many-to-many.

Examples in MPESA APP:

- One organization can have many users.
- One organization can have many merchant clients.
- One merchant client can have many STK requests.
- One merchant client can have many transactions.
- Users and roles can have a many-to-many relationship.
- Roles and permissions can have a many-to-many relationship.

### Database integrity

Study:

- Foreign keys.
- Unique indexes.
- Composite indexes.
- Constraints.
- Transactions.
- Normalization.
- Referential integrity.

### Query safety

Understand:

- SQL injection.
- Prepared statements.
- Parameter binding.
- Query validation.

## 35.4 Web development

Study:

- HTML.
- CSS.
- JavaScript fundamentals.
- Forms.
- HTTP.
- Request methods.
- Response codes.
- Headers.
- Cookies.
- Sessions.
- JSON.
- REST API design.
- CORS.
- CSRF protection.

These concepts are necessary for understanding how the dashboard and external merchant websites communicate with the PHP backend.

## 35.5 API development

Study:

- REST architecture.
- HTTP GET.
- HTTP POST.
- HTTP PUT.
- HTTP PATCH.
- HTTP DELETE.
- Authentication.
- Authorization.
- API versioning.
- Request validation.
- Response formatting.
- Error handling.
- Rate limiting.
- Idempotency.

## 35.6 Payment gateway concepts

Study:

- Payment initiation.
- Payment references.
- Payment statuses.
- Callback processing.
- Webhooks.
- Transaction recording.
- Idempotency.
- Retry mechanisms.
- Reconciliation.
- Audit trails.
- Payment state transitions.

These are especially important for a payment application because the system must preserve accurate financial records.

## 35.7 Application security

Study:

- Password hashing.
- Authentication.
- Authorization.
- Role-based access control.
- SQL injection prevention.
- Cross-site scripting prevention.
- CSRF protection.
- Session security.
- API credential management.
- HTTPS.
- Input validation.
- Secret management.

## 35.8 Software engineering

Study:

- Modular design.
- Separation of concerns.
- Code organization.
- Naming conventions.
- Error handling.
- Logging.
- Unit testing.
- Feature testing.
- Debugging.
- Version control.
- Documentation.
- Deployment.

# 36. Recommended Programming Concepts and Books

The following subjects should guide offline study.

| Subject | What to learn | Where it applies |
| --- | --- | --- |
| PHP | Syntax, functions, classes, PDO | Backend development |
| OOP | Classes, interfaces, abstraction | Application architecture |
| MySQL | SQL, joins, indexes, constraints | Database |
| Web development | HTTP, HTML, forms, sessions | Dashboard and API |
| REST APIs | Requests, responses, authentication | Merchant integration |
| Software engineering | Modular design and testing | Whole application |
| Cybersecurity | Authentication and secure data handling | Payment security |
| Git | Commits, branches, merging | Source code management |

When studying a subject, connect it to a real feature in MPESA APP.

For example, when studying SQL joins, examine how a transaction relates to a merchant client and an organization.

When studying interfaces, examine how a payment service might depend on a common payment-provider contract.

When studying exception handling, examine how the application handles a Daraja timeout.

This approach makes the learning practical.

# 37. Git and Version Control Guide

Git should be used to preserve the application's development history.

## 37.1 Important Git commands

Check the current working state:

```bash
git status
```

Stage changes:

```bash
git add .
```

Commit changes:

```bash
git commit -m "Add merchant registration"
```

Push changes:

```bash
git push
```

Pull changes:

```bash
git pull
```

View commit history:

```bash
git log --oneline
```

## 37.2 Development branches

For larger features, use separate branches.

Example:

```text
main
  |
  +--- feature/organization-management
  |
  +--- feature/merchant-registration
  |
  +--- feature/stk-push
  |
  +--- feature/transaction-dashboard
```

Merge completed and tested changes into the main branch.

## 37.3 Commit standards

Commit messages should describe the change.

Examples:

```text
Add organization database migration
Implement merchant authentication
Add STK Push request validation
Implement Safaricom callback processing
Add transaction search
Fix organization access checks
```

## 37.4 Secrets and Git

Never commit:

- `.env`.
- Database passwords.
- Daraja credentials.
- Merchant API secrets.
- Encryption keys.
- Production callback secrets.
- Private customer data.

# 38. Feature Development Roadmap

The recommended implementation order is as follows.

## Phase 1: Application foundation

Establish the application framework and infrastructure.

Tasks:

- Application bootstrap.
- Configuration loading.
- Database connection.
- Routing.
- Request handling.
- Error handling.
- Basic logging.

## Phase 2: Database foundation

Create the core database structure.

Tasks:

- Organizations.
- Users.
- Roles.
- Permissions.
- User-role assignments.
- Role-permission assignments.
- Merchant clients.

## Phase 3: Authentication and authorization

Implement:

- User login.
- User logout.
- Session management.
- Super Admin authentication.
- Client Admin authentication.
- Organization-defined roles.
- Permission checks.
- Organization isolation.

## Phase 4: Merchant management

Implement:

- Merchant registration.
- Merchant management.
- Merchant API credentials.
- Credential rotation.
- Merchant activation and suspension.
- Merchant webhook configuration.

## Phase 5: M-Pesa integration

Implement:

- Daraja authentication.
- Access-token handling.
- STK Push initiation.
- STK request recording.
- Safaricom response handling.
- Callback reception.
- Callback processing.
- Transaction recording.

## Phase 6: Merchant notifications

Implement:

- Merchant webhook delivery.
- Webhook status tracking.
- Retry management.
- Notification verification.
- Delivery logs.

## Phase 7: Dashboard

Implement:

- Dashboard overview.
- Transaction listing.
- Transaction details.
- Search.
- Filtering.
- Sorting.
- Organization management.
- Merchant management.
- User management.
- Role and permission management.

## Phase 8: Reliability and security

Implement and test:

- Duplicate callback prevention.
- Idempotent payment processing.
- Callback recovery.
- Webhook retries.
- Audit logs.
- Rate limiting.
- Secure secret management.
- Tenant isolation testing.
- Backup and restoration.

## Phase 9: Production deployment

Complete:

- Production database.
- Environment configuration.
- Web server configuration.
- HTTPS.
- Database migrations.
- Safaricom production integration.
- Callback registration.
- Merchant integration testing.
- Monitoring.
- Backup configuration.

---

# PART E — DEVELOPMENT STANDARDS AND LONG-TERM DIRECTION

# 39. Development Standards

Every developer working on MPESA APP should follow these standards.

## 39.1 Code organization

1. Keep controllers focused on request handling.
2. Put business operations in services.
3. Use models for database entities and data access.
4. Keep middleware focused on request-level checks.
5. Use request validation before processing untrusted input.
6. Keep reusable support functions organized.
7. Avoid unnecessarily duplicating business logic.

## 39.2 Database standards

1. Use migrations for database structure changes.
2. Define primary keys.
3. Define appropriate foreign keys.
4. Add indexes where justified.
5. Use unique constraints for identifiers requiring uniqueness.
6. Use appropriate data types.
7. Preserve organization ownership.
8. Use database transactions for related database operations.
9. Avoid destructive changes without a migration and recovery plan.

## 39.3 Security standards

1. Never hardcode secrets.
2. Never commit `.env`.
3. Never expose merchant secrets to browser JavaScript.
4. Never trust browser-supplied organization IDs for authorization.
5. Enforce permissions on the backend.
6. Use prepared statements or secure database abstractions.
7. Validate incoming data.
8. Protect private storage.
9. Use HTTPS in production.
10. Record important administrative actions.

## 39.4 Payment processing standards

1. Do not treat an accepted STK request as a successful payment.
2. Use Safaricom payment outcomes to establish transaction results.
3. Keep STK requests separate from transactions.
4. Prevent duplicate transaction recording.
5. Preserve callback information securely.
6. Make failed processing recoverable.
7. Keep merchant webhook delivery separate from payment recording.
8. Record important payment-processing events.
9. Do not expose sensitive payment information unnecessarily.

## 39.5 Testing standards

1. Test new functionality.
2. Test invalid inputs.
3. Test unauthorized access.
4. Test organization isolation.
5. Test failure scenarios.
6. Test duplicate processing.
7. Test existing functionality after significant changes.

## 39.6 Documentation standards

1. Update the README when architecture changes.
2. Document new API endpoints.
3. Document new database tables.
4. Explain new services and their responsibilities.
5. Record important architectural decisions.
6. Document environment variables.
7. Keep deployment instructions current.

# 40. Architectural Decisions

The following decisions define the current direction of MPESA APP.

| Area | Agreed decision |
| --- | --- |
| Project | Separate MPESA APP |
| Existing application | Must remain untouched |
| Database | New independent database |
| Database architecture | One shared centralized database |
| Tenant isolation | `organization_id` |
| Primary roles | Super Admin and Client Admin |
| Additional roles | Defined by individual organizations |
| Organization administration | Client Admin |
| Platform administration | Super Admin |
| Merchant integration | External websites connect through API |
| Merchant identification | Individual merchant credentials |
| Payment initiation | STK Push |
| Payment provider | Safaricom Daraja |
| Safaricom notification | Callback to MPESA APP |
| Merchant notification | Separate webhook from MPESA APP |
| STK requests | Separate database table |
| Transactions | Separate database table |
| Dashboard | Core component |
| User permissions | Flexible role-based permissions |
| Logging | Private application storage |
| Callback recovery | Supported by the processing architecture |
| Deployment | Independent of existing production application |

# 41. Future Expansion

The architecture should allow additional functionality without requiring a complete redesign.

Possible future enhancements include:

## 41.1 Advanced reporting

- Organization-level payment reports.
- Merchant-level reports.
- Date-based summaries.
- Payment success rates.
- Transaction exports.
- Reconciliation reports.

## 41.2 Improved merchant integration

- Merchant API documentation portal.
- Merchant sandbox environment.
- Integration testing tools.
- Webhook configuration interface.
- Merchant credential management interface.

## 41.3 Improved administration

- Advanced audit logs.
- Organization activity reports.
- User activity monitoring.
- Permission management interface.
- Platform operational dashboard.

## 41.4 Reliability improvements

- Background processing.
- Dedicated job queues.
- Improved callback recovery.
- Webhook delivery scheduling.
- Monitoring and alerting.
- Automated backup verification.

## 41.5 Additional payment capabilities

The architecture may later be extended to support additional payment operations or providers.

Such extensions should be implemented through clear service boundaries rather than mixing every provider's logic into one controller.

# 42. Final Project Vision

MPESA APP is intended to become a secure, reliable and extensible multi-tenant payment gateway.

It should allow multiple organizations to connect their merchant websites and applications to one centralized payment platform.

The platform should provide:

- Organization management.
- Merchant management.
- User and permission management.
- STK Push payment initiation.
- Safaricom callback processing.
- Transaction recording.
- Merchant webhook notifications.
- A complete administration dashboard.
- Operational logging and recovery.

The architecture must make it possible to add new features without rewriting the entire application.

The documentation must also make the project understandable to another developer or to its original developer returning after months away.

## The developer independence objective

This project is also intended to be a practical software engineering learning experience.

The developer should be able to:

- Understand every major application component.
- Explain how the files communicate.
- Understand the database relationships.
- Trace a payment from merchant request to final transaction.
- Locate the correct folder when adding a feature.
- Read PHP and MySQL references independently.
- Implement new features without relying on an AI assistant.
- Debug application errors.
- Test changes.
- Maintain the application.
- Extend the architecture as requirements grow.

**The ultimate objective is not simply to have a working M-Pesa payment gateway. It is to build a system whose architecture, code, database and operational behavior are fully understood by the developer responsible for it.**

---

**End of MPESA APP Project Documentation**

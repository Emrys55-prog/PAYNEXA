# MPESA APP — MVP Implementation Roadmap

## 1. Purpose

The Minimum Viable Product (MVP) is the first complete, functional version of MPESA APP.

Its purpose is to establish a working multi-tenant M-Pesa payment gateway that allows multiple organizations to manage merchant applications, initiate STK Push payments, process Safaricom callbacks, track transactions, and receive payment notifications.

This document defines the first implementation scope. The main `README.md` remains the complete project specification and technical reference.

## 2. MVP Scope

The MVP must include the following core components:

1. **Application foundation**
   - PHP application setup.
   - Environment configuration.
   - Database connection.
   - Routing and application structure.
   - Database migrations.

2. **Database**
   - Organizations.
   - Users.
   - Roles and permissions.
   - Merchant clients.
   - STK requests.
   - Transactions.
   - Callbacks.
   - Merchant webhooks.
   - Audit logs.

3. **Authentication and authorization**
   - Super Admin and Client Admin accounts.
   - Secure login and logout.
   - Password hashing and session management.
   - Role-based permissions.
   - Organization-level access restrictions.

4. **Organization management**
   - Create and manage organizations.
   - Assign Client Admins.
   - Activate and suspend organizations.

5. **Merchant client management**
   - Register merchant websites and applications.
   - Assign clients to organizations.
   - Generate and manage API credentials.
   - Enable and disable merchant clients.

6. **STK Push API**
   - Authenticate merchant requests.
   - Validate payment information.
   - Record STK requests.
   - Communicate with Safaricom Daraja.
   - Handle initiation responses and errors.

7. **Callback and transaction processing**
   - Receive Safaricom callbacks.
   - Preserve callback records.
   - Match callbacks to STK requests.
   - Process payment outcomes.
   - Create and update transactions.
   - Prevent duplicate processing.
   - Support failed callback recovery.

8. **Merchant webhook notifications**
   - Configure merchant notification endpoints.
   - Notify merchant websites of payment outcomes.
   - Authenticate outgoing notifications.
   - Record delivery attempts.
   - Retry failed deliveries.

9. **Dashboards**
   - Super Admin dashboard.
   - Client Admin dashboard.
   - Transaction overview and details.
   - Transaction search and filtering.
   - Merchant client management.
   - Organization-specific data visibility.

10. **Testing and deployment**
    - Authentication and authorization tests.
    - Tenant isolation tests.
    - STK Push integration tests.
    - Callback and duplicate-processing tests.
    - Transaction status tests.
    - Webhook delivery tests.
    - Production configuration, backups, and monitoring.

## 3. Development Order

Implement the MVP in this sequence:

| Phase | Work |
| --- | --- |
| 1 | Application foundation |
| 2 | Database design and migrations |
| 3 | Authentication and authorization |
| 4 | Organization management |
| 5 | Merchant client management |
| 6 | STK Push integration |
| 7 | Callback and transaction processing |
| 8 | Merchant webhook system |
| 9 | Dashboards |
| 10 | Testing and deployment |

Complete and test each phase before moving to the next.

## 4. Outside the MVP

The following features are reserved for future versions:

- Multiple payment providers.
- Mobile applications.
- Advanced analytics and reporting.
- CSV and Excel exports.
- Subscription billing.
- Automated accounting integrations.
- Advanced fraud detection.
- White-label dashboards.
- Multiple currencies.
- Complex workflow automation.

These features are not abandoned; they are postponed to keep the first release focused.

## 5. MVP Completion Criteria

The MVP is complete when:

1. The Super Admin can create and manage organizations.
2. Each organization can have its own Client Admin.
3. A Client Admin can register merchant applications.
4. A merchant application can authenticate and request an STK Push.
5. MPESA APP can communicate with Safaricom Daraja.
6. Safaricom callbacks can be received and processed reliably.
7. Transactions are recorded with the correct organization and merchant.
8. Duplicate callbacks do not create duplicate transactions.
9. Merchant websites receive payment outcome notifications.
10. Organization users can view their permitted transactions through the dashboard.
11. The Super Admin can monitor activity across organizations.
12. Automated tests demonstrate tenant isolation and payment-processing reliability.

## 6. Development Principles

- Keep the existing production M-Pesa application untouched.
- Develop MPESA APP as a separate application with its own database.
- Use one centralized database for all organizations.
- Enforce organization-level data isolation on the server.
- Keep STK requests and payment transactions separate.
- Treat Safaricom callbacks as the primary source of payment outcomes.
- Never expose API credentials or sensitive configuration.
- Document important implementation decisions.
- Write readable, maintainable PHP and SQL.
- Test every major component before deployment.

## 7. Relationship to the Main README

`README.md` is the complete project blueprint. It explains the architecture, database concepts, application structure, security requirements, business logic, and developer learning resources.

`MVP.md` defines what must be implemented first and the order in which the work should be completed.

**The MVP is the first release of the larger MPESA APP project, not a replacement for the original project specification.**

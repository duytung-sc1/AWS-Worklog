---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
# IrisAuth — Code Protector Platform

## Project Proposal

### Student Information / Project Team

- **Vo Tan Phat (Team Leader)** – Student ID: **SE194484**
- **Bui Minh Hien** – Student ID: **SE190829**
- **Duong Nguyen Binh** – Student ID: **SE194067**
- **Vinh Tran** – Student ID: **SE193927**
- **Nguyen Duy Tung** – Student ID: **SE196572**
- **Nguyen Tri** – Student ID: **[ID]**

---

## Source Code Protection and Secure Multi-language Script Distribution Platform

### Deployed on AWS Cloud Infrastructure

**AWS Lambda** · **S3** · **DynamoDB** · **CloudFront** · **CloudWatch** · **SES**

---

# I. PROJECT INTRODUCTION

## 1.1. Context and Problem Statement

In modern software development, protecting source code is an urgent challenge. When developers distribute Python or JavaScript scripts to end-users, they face the risk of their source code being copied, modified, or redistributed unauthorizedly without an effective control mechanism.

Existing solutions, such as simple obfuscation or packaging into `.exe` files, have many limitations: they are vulnerable to reverse engineering, cannot track who is executing the code, and lack the ability to revoke access when necessary. The market lacks an integrated solution that comprehensively protects source code while managing distribution and access control.

## 1.2. Proposed Solution

**Code Protector** (System name: **IrisAuth**) is a SaaS platform that allows developers to upload their source code to a server and distribute it to users via an encrypted loader mechanism. Instead of sharing the source code directly, end-users only receive a small 1–2 line loader. The server returns the encrypted code only when the user passes all security checks, and the code exists only in RAM during execution—it is never saved to a local file.

The system is deployed on **AWS Cloud**, leveraging a **serverless** architecture and **managed** services to ensure high scalability, low global latency, and operations without manual infrastructure management.

---

# II. PROJECT OBJECTIVES

## 2.1. General Objective

Build a complete web platform providing secure source code protection and distribution services for Python and JavaScript, featuring a multi-layered authentication system, an intuitive admin interface, and a globally scalable AWS cloud infrastructure.

## 2.2. Specific Objectives

- Design and implement a code distribution system via an encrypted loader, ensuring the source code is never exposed to end-users in plaintext.
- Build an **ECDH X25519** key exchange mechanism combined with **AES-256-GCM** encryption to secure the data transmission channel.
- Develop a license management system featuring **HWID Lock** (hardware binding), expiration dates, and execution limits.
- Build a workspace and team collaboration system with role-based access control.
- Develop a responsive administrative web interface, distributed via **Amazon CloudFront** for low global latency.
- Integrate monitoring and alerting via **Amazon CloudWatch**, centralized logging, and Discord Webhook notifications.
- Deploy a serverless architecture on **AWS Lambda**, object storage on **Amazon S3**, and structured data on **Amazon DynamoDB**.
- Integrate **Amazon SES** to send workspace invitation emails and automated system notifications.

---

# III. PROJECT SCOPE

## 3.1. In Scope

### Backend & API

- RESTful API and WebSocket with **Node.js / Express.js** running on **AWS Lambda** (via Lambda Function URL or API Gateway).
- Handling **ECDH Handshake**, **AES-256-GCM encryption**, and **XOR protocol** for loader distribution.
- Rate limiting, IP access control, request signature validation.

### Frontend

- 5 interface pages: **Landing**, **Login**, **Register**, **Dashboard**, **Workspace**.
- Static assets distributed globally via **Amazon CloudFront CDN**.
- **Vanilla HTML5/CSS3/JS**, no heavy framework dependencies.
- WebSocket real-time sync with **AWS API Gateway WebSocket API**.

### AWS Infrastructure (New — replacing self-hosted)

- **Amazon S3**: Storage for encrypted source code (replacing local filesystem storage).
- **Amazon DynamoDB**: NoSQL database replacing SQLite — storing users, workspaces, projects, licenses, logs, rate_limits, config.
- **AWS Lambda**: Runtime for Express.js API (serverless, auto-scaling).
- **Amazon CloudFront**: CDN for static file distribution, S3 asset caching, SSL termination, custom domain.
- **Amazon CloudWatch**: Monitoring, log aggregation, metric alarms, system dashboard.
- **Amazon SES**: Sending workspace invitation emails via tokens.

### Loader & Client

- Python loader (`urllib`, Python 3.7+).
- Node.js loader.
- Browser Userscript (Tampermonkey).
- **ECDH handshake v3** and **XOR execute v2** — operating through CloudFront endpoints.

### Business Features

- License system: Single/bulk creation, HWID binding, CSV export.
- IP Blacklist/Whitelist, Rate Limiting (sliding window on DynamoDB), Workspace PIN.
- Team management: Member invitations via email tokens (sent via SES), 3 roles (**owner / editor / viewer**).
- Multi-file project management with bundle generation (**Python & Node.js**).
- Python AST Obfuscator (standalone module).
- Real-time synchronization via WebSocket, Discord Webhook notifications.

## 3.2. Out of Scope

- Native mobile applications.
- Support for programming languages other than Python and JavaScript.
- Payment gateway integration.
- CI/CD automated deployment pipelines beyond basic **AWS SAM / CDK**.

---

# IV. SYSTEM ARCHITECTURE & TECHNOLOGY

## 4.1. Technology Stack (AWS-native)

| Layer | Service / Technology | Details |
|------|----------------------|----------|
| API Runtime | Node.js v18+ (AWS Lambda) | ES Module, Express.js wrapped with aws-serverless-express |
| Database | Amazon DynamoDB | NoSQL key-value, on-demand capacity, auto TTL for `rate_limits` & `sessions` |
| Object Storage | Amazon S3 | Stores encrypted + gzipped scripts (`key = UUID`), versioning enabled, AES-256 server-side encryption |
| CDN & Edge | Amazon CloudFront | Distributes static frontend, SSL termination, S3 caching, custom domain, integrated WAF |
| Compute | AWS Lambda | Auto-scale 0→∞, cold start <1s (Node.js 18), function URL or API Gateway |
| Email | Amazon SES | Sends workspace invitation emails, DKIM + SPF config, sandbox → production |
| Monitoring | Amazon CloudWatch | Log Groups per Lambda function, metric alarms (error rate, latency), dashboard |
| Real-time | API Gateway WebSocket API | Broadcast per-workspace updates, replaces standalone `ws` server |
| Frontend | HTML5, CSS3, Vanilla JS | 5 static pages, deployed to S3 bucket, distributed via CloudFront |
| Data Compression| Pako (Gzip) | Compresses code before uploading to S3 |

## 4.2. AWS Architecture Overview

The **IrisAuth** system is deployed using a pure **serverless** model on AWS, entirely eliminating physical server management. Typical request flow:

- Client (browser / Python loader) → **CloudFront Distribution** (SSL termination, cache layer).
- Static assets (HTML/CSS/JS) are served directly from the **S3 bucket** via CloudFront.
- API requests `/api/*` are forwarded to **API Gateway** → **AWS Lambda function**.
- Lambda function processes business logic, reads/writes to **DynamoDB** (metadata) and **S3** (script content).
- Email invitations are sent via **Amazon SES** with templates stored on SES.
- All Lambda logs are automatically streamed to **CloudWatch Logs**; **CloudWatch Alarms** trigger when error rates spike.
- WebSocket connections (execute endpoint) go through **API Gateway WebSocket API**, routed to a Lambda handler.

## 4.3. Authentication System (Self-built, No external JWT libraries)

The system does not rely on external JWT libraries.

- Tokens are signed using **HMAC-SHA256**, with a 48-byte random secret stored in **DynamoDB** (`app_config` table).
- Token TTL: **7 days**.
- When a user changes their password, all old tokens are instantly invalidated (comparing `iat` with `password_changed_at`).
- Sent via **HttpOnly Cookie** (named `token`) combined with the `Authorization` header.
- Passwords are hashed using **PBKDF2-SHA256**, **210,000 iterations**, with a 16-byte random salt — meeting **OWASP** standards.
- Legacy accounts using plain SHA-256 are automatically upgraded upon their next login.
- Workspace PINs also use **PBKDF2**, generating isolated PIN session tokens stored in the `pin_verifications` DynamoDB table with automatic TTL.

## 4.4. Two Code Distribution Protocols

### Protocol v2 — `GET /api/v5/execute` (XOR)

- Client sends: `id`, `license key`, `HWID`, `timestamp`, `nonce`, `HMAC-SHA256 signature`.
- Server validates the signature and checks the timestamp within ±5 minutes (preventing replay attacks).
- Response is encrypted via XOR with key = `SHA256(derivedSecret:hwid:nonce:id)`.
- Response includes a 16-byte header (magic bytes + content length + timestamp + random) appended with script bytes, then fully XORed.
- Script content is fetched from **Amazon S3**.

### Protocol v3 — `POST /api/v5/handshake` (ECDH + AES-GCM)

- Client generates an **X25519** key pair, sends the public key to the server along with a signature.
- Lambda function generates its own X25519 key pair, calculates the shared secret via `diffieHellman()`.
- Derives AES key using **HKDF-SHA256** (RFC 5869), `salt = "IrisAuth-Salt"`, `info = "IrisAuth-{nonce}"`.
- Encrypts script using **AES-256-GCM**, returning server public key + ciphertext + validation signature.
- Each session has a completely unique session key — ensuring **perfect forward secrecy**.

## 4.5. Algorithms and Security Standards

| Algorithm | Specific Application in the System |
|-----------|--------------------------------|
| PBKDF2-SHA256 (210,000 iter) | Hashing user passwords, Workspace PINs — stored in DynamoDB |
| HMAC-SHA256 | Signing auth tokens, loader requests, server responses |
| ECDH X25519 | Key exchange for Protocol v3 (handshake) |
| HKDF-SHA256 | Deriving AES key from ECDH shared secret (RFC 5869) |
| AES-256-GCM | Encrypting scripts stored on S3 + transit encryption for Protocol v3 |
| XOR + SHA-256 | Transit encryption for Protocol v2 |
| Gzip (Pako) | Compressing content before storing to S3 |
| `crypto.timingSafeEqual` | Comparing tokens/signatures — preventing timing attacks |
| Nonce + Timestamp ±5 mins | Preventing replay attacks on all loader requests |
| S3 Server-Side Encryption | Encrypting all objects at-rest on S3 using AWS KMS (SSE-KMS) |
| CloudFront + ACM | Enforcing TLS 1.2+ minimum on all client-server connections |

## 4.6. Database — Amazon DynamoDB

Replaced SQLite with **Amazon DynamoDB** (fully managed NoSQL). Table design follows a single-table or multi-table model depending on access patterns:

| DynamoDB Table | Partition Key / Sort Key | Function |
|--------------|---------------------------|-----------|
| users | PK: `userId` | User accounts, roles, statuses, `password_changed_at` |
| workspaces | PK: `workspaceId` | Workspaces, `loader_key`, languages, `encryption_key`, PIN hash |
| projects | PK: `workspaceId`, SK: `projectId` | Projects (scripts), security settings, execution counts |
| project_files | PK: `projectId`, SK: `fileId` | Files/folders within projects, entry points, language, `sort_order` |
| licenses | PK: `workspaceId`, SK: `licenseKey` | License keys, HWID, expiration dates, usage counts, activated OS |
| access_lists | PK: `workspaceId`, SK: `ip#type` | IP blacklists / whitelists per workspace |
| workspace_members | PK: `workspaceId`, SK: `userId` | Invited members, roles |
| workspace_invitations | PK: `token` | Email invitation tokens (SES), automatic TTL |
| pin_verifications | PK: `sessionToken` | Session tokens post PIN verification, automatic TTL |
| logs | PK: `workspaceId`, SK: `timestamp#uuid` | Event logs: IP, country, action — GSI on country, timestamp |
| app_config | PK: `configKey` | System configuration (HMAC secrets, loader secrets...) |
| rate_limits | PK: `rateLimitKey` | Rate limit sliding window with auto TTL cleanup |

**Design Note:** DynamoDB TTL is enabled on `workspace_invitations`, `pin_verifications`, and `rate_limits` tables to automatically delete expired records without cronjobs. A GSI (Global Secondary Index) is created on the `country` and `timestamp` fields of the `logs` table to support analytical queries.

## 4.7. Amazon S3 — Object Storage for Script Content

- Completely replaces local filesystem storage (`/storage` directory).
- Each script file is stored with a key format: `{workspaceId}/{uuid}_{timestamp}_{hash}.gz`.
- Content is compressed with **Gzip** (Pako), encrypted with **AES-256-GCM**, and then PUT to **S3**.
- S3 bucket has **versioning**, **server-side encryption SSE-KMS**, and absolute **Block Public Access** enabled.
- Lambda functions access S3 via **IAM Roles** with **least-privilege** policies.
- Static frontend assets are stored in a separate S3 bucket and distributed via **CloudFront**.

## 4.8. Amazon CloudFront — CDN & Edge

- A single Distribution serves both static frontend and API proxying.
- Behavior rules: `/api/*` → API Gateway origin; `/*` → S3 static bucket origin.
- Enforces HTTPS, TLS 1.2 minimum, HTTP/2 enabled by default.
- Optimized Cache TTL for static assets (`86400s`); `no-cache` for API responses.
- Custom domain mapping with certificates from **AWS Certificate Manager (ACM)**.
- Integrates **AWS WAF** to block bad bots, SQL injections, and XSS at the edge layer.
- CloudFront header `cf-ipcountry` is forwarded to log the country of origin in the system.

## 4.9. Amazon CloudWatch — Monitoring & Observability

- Automated Log Groups for each Lambda function with a 30-day retention period.
- Metric Filters to extract custom metrics: `execution_count`, `license_error_rate`, `ecdh_handshake_rate`.
- CloudWatch Alarms: Alerting when Lambda error rate > 1%, p99 latency > 2s, or DynamoDB throttle events occur.
- CloudWatch Dashboards: Aggregating request volumes, latency distributions, error breakdowns, and S3 storage usage.
- Structured JSON logging from Lambda enables rich queries via CloudWatch Logs Insights (filtering by `workspaceId`, `action`, `IP`).

## 4.10. Amazon SES — Email Service

- Sends workspace invitation emails containing token links.
- Email templates are stored as SES Templates or inline HTML.
- Configures **DKIM**, **SPF**, and **DMARC** to ensure high deliverability.
- SES Event destinations connect to CloudWatch to monitor bounce and complaint rates.
- Transitions from Sandbox mode to Production after verifying the sending domain.

## 4.11. Role-Based Access Control (RBAC)

| Role | Project Management | License Management | View Logs | Workspace Settings | Invite Members |
|--------|------------------|------------------|---------|-------------------|----------------|
| Owner | ✓ Full Access | ✓ Full Access | ✓ Full Access | ✓ Full Access | ✓ Full Access |
| Admin | ✓ Full Access | ✓ Full Access | ✓ Full Access | ✓ Limited | ✓ Allowed |
| Editor | ✓ CRUD | ✓ Create/View | ✓ View | ✗ | ✗ |
| Viewer | ✗ Read-only | ✗ View-only | ✓ View | ✗ | ✗ |

---

# V. DETAILED FEATURES

## 5.1. Core Workflow (with AWS)

- Developer logs in, creates a workspace → metadata saved to **DynamoDB**.
- Developer uploads or imports source code → code is **Gzip** compressed (Pako), encrypted via **AES-256-GCM** with the workspace's `encryption_key`, and PUT to **Amazon S3** with a UUID key.
- Developer retrieves a 1–2 line loader (Python / JS), with the CloudFront endpoint embedded.
- End-user runs the loader → loader generates HWID from MAC address, machine name, CPU, OS, username → hashed with **SHA-256**.
- Loader performs ECDH Handshake or calls `/api/v5/execute` attaching **HMAC signature + nonce + timestamp** via CloudFront → API Gateway → Lambda.
- Lambda validates the signature and checks all access conditions (**DynamoDB**: license, HWID, rate_limit, access_list).
- If valid: Lambda GETs the object from **S3**, decrypts it, and returns the encrypted code based on protocol v2/v3. Loader decrypts and executes it in RAM via `exec()` — no file is saved.
- Lambda writes logs to **DynamoDB** (`logs` table), updates `execution_count`, and broadcasts WebSocket events via **API Gateway WebSocket API**.
- **CloudWatch** automatically ingests the log stream, updating metrics in real-time.

## 5.2. License System

- Create single license keys with the format `[prefix]LIC-XXXXXXXX-XXXXXXXX[suffix]`, supporting custom keys.
- Bulk create up to 100 licenses per batch (**DynamoDB batch write**).
- **HWID Lock**: Upon the first run, the user's HWID is written to DynamoDB. Subsequent executions are rejected if the HWID mismatches.
- Enable/disable `hwid_lock` individually for each license.
- Set expiration dates (`expiration_date`), verified in real-time on every execution.
- Bind licenses to a specific project or the entire workspace.
- Reset HWID when users switch machines.
- Export full lists to CSV (`key`, `note`, `status`, `HWID`, `OS`, `usage count`, `last used`).

## 5.3. Access Control

- **IP Blacklist**: Permanently block specific IPs from a workspace — stored in the `access_lists` DynamoDB table.
- **IP Whitelist**: Only allow listed IPs, toggled per project.
- **CloudFront WAF**: Additional edge protection layer (rate-based rules, geo-blocking if necessary).
- **Max executions**: Limit the total number of times a project can be run before auto-locking.
- **Rate limiting** (sliding window, stored in DynamoDB with TTL):
  - Login: max 10 requests/min/IP, 5 requests/min/email.
  - Register: max 5 requests/min/IP.
  - Loader execute: max 120 requests/min/IP (global), custom limits per script (1–600/min).
  - ECDH handshake: max 60 requests/min/IP.
  - Create license: 30/min; batch create: 5/min.
- DynamoDB TTL automatically cleans up expired `rate_limit entries` — no cronjob needed.
- Workspace PIN: Hashed with **PBKDF2**, issues separate PIN session tokens stored in DynamoDB with auto TTL.
- Request signature: Every request to the loader must include a valid **HMAC-SHA256 signature**.

## 5.4. Multi-file Project Management

- Organized by directory structure (`type: file | folder`), stored in DynamoDB's `project_files` table with `parent_id`, `sort_order`.
- Specify entry point: Mark the main file (`is_entry_point`) for multi-file projects.
- Bundle generation: Lambda automatically creates bundles during distribution:
  - **Python**: Merges all files into a single script, resolving import paths according to the folder structure.
  - **Node.js**: Similarly wraps files based on module structure.
- Supports 30+ languages (detected via extensions: `.py`, `.js`, `.ts`, `.lua`, `.go`, `.rs`, `.java`, `.c`, `.cpp`, `.sql`...).
- Operations: Create, rename, move, copy, delete, upload, full-text search.
- Batch operations, single-file downloads.
- Large content stored in S3 (`key format: s3:<bucket>/<uuid>`), decrypted + decompressed upon reading.

## 5.5. Workspace & Team Collaboration

- Each user can own multiple independent workspaces, each with a unique `loader_key` (public ID).
- Invite members via email tokens — Lambda calls **Amazon SES** to send automated emails.
- Tokens are stored in DynamoDB's `workspace_invitations` table with auto TTL. Invitees receive an acceptance link via email.
- 4 permission roles: `owner / admin / editor / viewer`.
- Real-time updates via **API Gateway WebSocket API**: License, project, or team changes are broadcast to all connected clients in the workspace.
- **Discord Webhook**: Automatically sends notifications for critical events (configured per workspace).
- Dashboard summary statistics: total `execution count`, number of licenses, number of members.

## 5.6. Logging & Monitoring

- Logs all events: `LOAD_SCRIPT`, `ECDH_HANDSHAKE`, `BLOCK_ACCESS`, `INVALID_LICENSE`, `INVALID_HWID`, `INVALID_SIGNATURE`, `CREATE_LICENSE`, `BATCH_CREATE_LICENSE`, `TOGGLE_LICENSE`...
- Log entry details: `workspace_id`, `action`, `details`, `IP`, `country`, `timestamp` — stored in **DynamoDB**.
- Lambda automatically streams logs to **CloudWatch Logs** (structured JSON) — queryable via **CloudWatch Logs Insights**.
- CloudWatch Alarms provide proactive alerts: Lambda error spikes, DynamoDB throttling, high SES bounce rates.
- CloudWatch Dashboards summarize KPIs: request rates, latency percentiles, error rates, and execution counts per workspace.
- View and delete logs per workspace via the admin interface.
- All changes are broadcast in real-time via the **API Gateway WebSocket API**.

## 5.7. Python AST Obfuscator (Standalone module)

The `obfuscator.py` file is a completely standalone Python obfuscation tool operating at the AST layer:

- Parses syntax and checks for compatibility issues prior to obfuscation.
- Renames variables, functions, and parameters to meaningless randomized strings.
- Injects dead code to add noise and confuse readers.
- Provides detailed reports on compatibility issues (`severity: error / warning`) including line and column numbers.

---

# VI. IMPLEMENTATION PLAN

| Phase | Content | Estimated Time |
|----------|----------|-------------------|
| 1. Analysis & Design | Define detailed requirements, design DynamoDB schema, API, and CloudFront behavior rules | Weeks 1–2 |
| 2. AWS Infrastructure | Provision Lambda, DynamoDB, S3, CloudFront, SES, CloudWatch (IaC via AWS SAM or CDK) | Week 3 |
| 3. Core Backend | Auth, Workspace, Project, File management APIs — migrate from SQLite to DynamoDB, filesystem to S3 | Weeks 4–6 |
| 4. Security & Loader | ECDH handshake, AES encryption, Python/JS loaders operating via CloudFront endpoints | Weeks 7–8 |
| 5. License & Access | License system, HWID, Rate limits (DynamoDB TTL), Blacklisting, IP Whitelisting | Week 9 |
| 6. Frontend & Email | 5 UI pages, WebSocket integration (API GW), SES email invitations | Weeks 10–11 |
| 7. Obfuscator | Python AST obfuscator, bundle generation | Week 12 |
| 8. Testing & Deploy | Unit testing, integration testing, load testing, CloudWatch alarm tuning, production deployment | Weeks 13–14 |

---

# VII. EXPECTED OUTCOMES

Upon completion, the project will deliver:

- A fully operational web platform deployed on **AWS** with a **serverless** architecture (`Lambda + DynamoDB + S3 + CloudFront`).
- A source code protection system with 2 encryption protocols (**v2 XOR** and **v3 ECDH/AES-GCM**), safely storing script content on **S3**.
- An intuitive admin interface distributed via **CloudFront** with low global latency.
- Loader clients compatible with **Python 3.7+**, **Node.js v14+**, and **Tampermonkey** — communicating via CloudFront endpoints.
- Automated email invitations via **Amazon SES** — members receive direct links to their inboxes.
- Comprehensive monitoring & alerting via **CloudWatch**: centralized logs, automated alarms, and KPI dashboards.
- Complete technical documentation: AWS architecture, API specs, and deployment guides using **AWS SAM/CDK**.

---

# VIII. TECHNOLOGIES USED

**Node.js**, **Express.js**, **AWS Lambda**, **Amazon DynamoDB**, **Amazon S3**, **Amazon CloudFront**, **Amazon CloudWatch**, **Amazon SES**, **AWS API Gateway (REST + WebSocket)**, **AWS WAF**, **AWS KMS**, **AWS IAM**, **ECDH X25519**, **AES-256-GCM**, **HMAC-SHA256**, **HKDF-SHA256**, **PBKDF2-SHA256**, **Python urllib**, **Python AST Manipulation**, **Gzip Compression**, **CORS**, **Rate Limiting**, **Role-Based Access Control (RBAC)**, **Discord Webhook**.

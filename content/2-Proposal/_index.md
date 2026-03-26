---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# IrisAuth — Code Protector Platform

## Project Proposal

### Team Members

- **Vo Tan Phat (Team Leader)** – Student ID: **SE194484**
- **Bui Minh Hien** – Student ID: **SE190829**
- **Duong Nguyen Binh** – Student ID: **SE194067**
- **Vinh Tran** – Student ID: **SE193927**
- **Nguyen Duy Tung** – Student ID: **SE196572**
- **Nguyen Tri** – Student ID: **[ID]**

---

## Multi-language Secure Code Protection & Script Distribution Platform

### Deployed on AWS Cloud Infrastructure

**AWS Lambda** · **S3** · **DynamoDB** · **CloudFront** · **CloudWatch** · **SES**

---

# I. PROJECT INTRODUCTION

## 1.1. Background and Problem

In modern software development, protecting source code is a critical challenge. When developers distribute Python or JavaScript scripts to end users, they face risks such as unauthorized copying, modification, or redistribution without effective control mechanisms.

Existing solutions like simple obfuscation or packaging into `.exe` files have significant limitations: they are vulnerable to reverse engineering, lack execution control, and cannot revoke access when necessary. The market lacks an integrated solution that provides both code protection and comprehensive distribution management.

---

## 1.2. Proposed Solution

**Code Protector** (system name: **IrisAuth**) is a SaaS platform that allows developers to upload source code to a server and distribute it via an encrypted loader mechanism.

Instead of sharing raw source code, end users only receive a lightweight loader (1–2 lines). The server returns encrypted code only after all security checks are passed. The code exists **only in memory during execution** and is never written to disk.

The system is deployed on **AWS Cloud**, leveraging a **serverless architecture** and managed services to ensure high scalability, low latency, and minimal operational overhead.

---

# II. PROJECT OBJECTIVES

## 2.1. General Objective

To build a complete web platform that provides secure code protection and distribution for Python and JavaScript, with multi-layer authentication, an intuitive admin interface, and globally scalable AWS infrastructure.

---

## 2.2. Specific Objectives

- Design and implement a loader-based code distribution system to prevent plaintext exposure.
- Implement secure key exchange using **ECDH X25519** and **AES-256-GCM encryption**.
- Develop a license system with **HWID Lock**, expiration, and execution limits.
- Build workspace and team collaboration features with role-based access.
- Develop a responsive web interface delivered via **Amazon CloudFront**.
- Integrate monitoring and alerting via **Amazon CloudWatch** and Discord Webhooks.
- Deploy serverless architecture using **AWS Lambda**, **S3**, and **DynamoDB**.
- Integrate **Amazon SES** for email invitations and notifications.

---

# III. PROJECT SCOPE

## 3.1. In Scope

### Backend & API

- RESTful API and WebSocket using **Node.js / Express.js** on **AWS Lambda**
- Implementation of **ECDH handshake**, **AES-256-GCM**, and **XOR protocol**
- Rate limiting, IP control, request signature validation

---

### Frontend

- 5 pages: **Landing, Login, Register, Dashboard, Workspace**
- Global distribution via **Amazon CloudFront**
- Built with **Vanilla HTML/CSS/JS**
- Real-time sync via WebSocket API

---

### AWS Infrastructure

- **Amazon S3**: encrypted code storage  
- **Amazon DynamoDB**: NoSQL database  
- **AWS Lambda**: serverless backend  
- **Amazon CloudFront**: CDN  
- **Amazon CloudWatch**: monitoring  
- **Amazon SES**: email service  

---

### Loader & Client

- Python loader (3.7+)  
- Node.js loader  
- Browser userscript (Tampermonkey)  
- Protocol v2 (XOR) and v3 (ECDH + AES-GCM)  

---

### Core Features

- License system (single/bulk, HWID binding, CSV export)  
- IP blacklist/whitelist, rate limiting  
- Team management with roles  
- Multi-file project bundling  
- Python AST obfuscator  
- Real-time updates & Discord webhook  

---

## 3.2. Out of Scope

- Mobile applications  
- Languages beyond Python and JavaScript  
- Payment integration  
- Advanced CI/CD pipelines  

---

# IV. SYSTEM ARCHITECTURE & TECHNOLOGY

## 4.1. Technology Stack

| Layer | Technology | Description |
|------|------------|------------|
| API Runtime | Node.js (Lambda) | Serverless Express |
| Database | DynamoDB | NoSQL with TTL |
| Storage | S3 | Encrypted storage |
| CDN | CloudFront | Global distribution |
| Compute | Lambda | Auto-scaling |
| Email | SES | Email delivery |
| Monitoring | CloudWatch | Logs & metrics |
| Real-time | WebSocket API | Live updates |
| Frontend | HTML/CSS/JS | Static site |
| Compression | Gzip | Data compression |

---

## 4.2. Architecture Overview

- Client → CloudFront  
- Static content → S3  
- API → API Gateway → Lambda  
- Data → DynamoDB + S3  
- Logs → CloudWatch  
- Real-time → WebSocket API  

---

## 4.3. Authentication

- HMAC-SHA256 token  
- 7-day expiration  
- PBKDF2 password hashing (210k iterations)  
- HttpOnly cookies  
- Token invalidation on password change  

---

## 4.4. Code Distribution Protocols

### Protocol v2 (XOR)

- Lightweight encryption  
- Timestamp + nonce protection  

### Protocol v3 (ECDH + AES-GCM)

- X25519 key exchange  
- AES-256-GCM encryption  
- Perfect Forward Secrecy  

---

## 4.5. Security Standards

- PBKDF2-SHA256  
- HMAC-SHA256  
- ECDH X25519  
- HKDF-SHA256  
- AES-256-GCM  
- TLS 1.2+  
- Replay attack protection  

---

# V. KEY FEATURES

## 5.1. Core Workflow

- Developer uploads encrypted code  
- User runs loader  
- Server validates request  
- Code executed in memory  
- Logs recorded  

---

## 5.2. License System

- HWID lock  
- Expiration  
- Execution limits  
- Bulk generation  

---

## 5.3. Access Control

- IP blacklist/whitelist  
- Rate limiting  
- Workspace PIN  
- Signed requests  

---

## 5.4. Project Management

- Multi-file structure  
- Auto bundling  
- CRUD operations  

---

## 5.5. Collaboration

- Multi-user workspace  
- Role-based access  
- Email invitations  
- Real-time updates  

---

## 5.6. Monitoring

- Event logging  
- CloudWatch dashboards  
- Alerts & metrics  

---

## 5.7. Python Obfuscator

- Rename variables/functions  
- Add dead code  
- Compatibility analysis  

---

# VI. IMPLEMENTATION PLAN

| Phase | Description | Timeline |
|------|------------|----------|
| 1 | Analysis & Design | Weeks 1–2 |
| 2 | Infrastructure Setup | Week 3 |
| 3 | Backend Development | Weeks 4–6 |
| 4 | Security & Loader | Weeks 7–8 |
| 5 | License System | Week 9 |
| 6 | Frontend & Email | Weeks 10–11 |
| 7 | Obfuscator | Week 12 |
| 8 | Testing & Deployment | Weeks 13–14 |

---

# VII. EXPECTED RESULTS

- Fully functional AWS serverless platform  
- Secure code distribution system  
- Global low-latency interface  
- Multi-platform loader support  
- Monitoring & logging system  
- Complete documentation  

---

# VIII. TECHNOLOGIES USED

**Node.js**, **Express.js**, **AWS Lambda**, **Amazon DynamoDB**, **Amazon S3**, **Amazon CloudFront**, **Amazon CloudWatch**, **Amazon SES**, **AWS API Gateway**, **AWS WAF**, **AWS KMS**, **AWS IAM**, **ECDH X25519**, **AES-256-GCM**, **HMAC-SHA256**, **HKDF-SHA256**, **PBKDF2-SHA256**, **Python urllib**, **Python AST**, **Gzip**, **CORS**, **Rate Limiting**, **RBAC**, **Discord Webhook**.

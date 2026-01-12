# Data Flow Diagram - GreenWave Analytics Ltd.

This document illustrates the flow of data through GreenWave Analytics' SaaS platform and highlights where STRIDE threats may occur.

## Data Flow Architecture

```
┌──────────────┐
│   Customer   │
│   User       │
└──────┬───────┘
       │
       │ (STRIDE: Spoofing, Information Disclosure)
       │ Login Credentials / Data Input
       ▼
┌──────────────────────────────────────┐
│     Web Application (Front-end)      │
│  - User Interface                    │
│  - Request Validation                │
│  - Session Management                │
│                                      │
│ STRIDE: Spoofing, Tampering,        │
│         Information Disclosure,      │
│         Denial of Service            │
└──────┬───────────────────────────────┘
       │
       │ (STRIDE: Tampering, Information Disclosure)
       │ Authenticated Requests
       ▼
┌──────────────────────────────────────┐
│    Authentication System             │
│  - User Verification                 │
│  - Token Generation                  │
│  - MFA Validation                    │
│                                      │
│ STRIDE: Spoofing, Tampering,        │
│         Elevation of Privilege,      │
│         Denial of Service            │
└──────┬───────────────────────────────┘
       │
       │ (STRIDE: Tampering, Information Disclosure)
       │ Authorized API Requests with Tokens
       ▼
┌──────────────────────────────────────┐
│      AWS Infrastructure              │
│  ┌─────────────────────────────┐    │
│  │   RDS - Customer Database   │    │
│  │  - Customer PII             │    │
│  │  - Analytics Data           │    │
│  │                             │    │
│  │ STRIDE: Tampering,         │    │
│  │         Information        │    │
│  │         Disclosure,         │    │
│  │         Denial of Service,  │    │
│  │         Elevation of        │    │
│  │         Privilege           │    │
│  └─────────────────────────────┘    │
│  ┌─────────────────────────────┐    │
│  │    S3 - Data Storage        │    │
│  │  - Backups                  │    │
│  │  - Reports                  │    │
│  │                             │    │
│  │ STRIDE: Information        │    │
│  │         Disclosure,         │    │
│  │         Denial of Service   │    │
│  └─────────────────────────────┘    │
│                                      │
│ STRIDE: Spoofing, Tampering,        │
│         Information Disclosure,      │
│         Denial of Service,           │
│         Elevation of Privilege       │
└──────┬───────────────────────────────┘
       │
       │ (STRIDE: Information Disclosure, Denial of Service)
       │ Generated Reports / Analytics Results
       ▼
┌──────────────────────────────────────┐
│    API / Report Generation           │
│  - Dashboard Export                  │
│  - Report Packaging                  │
│  - Result Delivery                   │
│                                      │
│ STRIDE: Tampering,                  │
│         Information Disclosure,      │
│         Denial of Service            │
└──────┬───────────────────────────────┘
       │
       │ (STRIDE: Information Disclosure)
       │ Final Dashboard / Reports
       ▼
┌──────────────┐
│   Customer   │
│  Dashboard   │
│  & Reports   │
└──────────────┘
```

## Data Flow Components

### 1. Entry Point: Customer User
- **Input:** Login credentials, data queries, report requests
- **Threats:** Spoofing attacks, credential interception
- **Mitigation:** HTTPS, MFA enforcement

### 2. Web Application (Front-end)
- **Function:** Receives user requests, validates input, manages sessions
- **Threats:** XSS, CSRF, phishing, session hijacking
- **Mitigation:** CSP headers, input sanitization, secure cookies

### 3. Authentication System
- **Function:** Verifies user identity, generates tokens, enforces MFA
- **Threats:** Brute force attacks, token manipulation, privilege escalation
- **Mitigation:** Rate limiting, JWT with expiry, RBAC

### 4. AWS Infrastructure (Database & Storage)
- **RDS Database:**
  - Stores customer PII and analytics data
  - Encrypted at rest and in transit
  - Threats: SQL injection, data exfiltration, unauthorized access
  - Mitigation: Prepared statements, IAM policies, encryption

- **S3 Storage:**
  - Stores backups and exported reports
  - Threats: Misconfiguration exposure, data theft
  - Mitigation: Bucket policies, versioning, encryption

### 5. API & Report Generation
- **Function:** Processes data, generates outputs
- **Threats:** Data leakage in error messages, DDoS on API endpoints
- **Mitigation:** Rate limiting, proper error handling, monitoring

### 6. Exit Point: Customer Dashboard
- **Output:** Reports and analytics dashboards
- **Threats:** Man-in-the-middle interception, session hijacking
- **Mitigation:** HTTPS enforcement, secure session handling

## Critical Data Flows

| Flow | Data Type | Sensitivity | Threats | Controls |
|------|-----------|-------------|---------|----------|
| Customer → Web App | Credentials | High | Spoofing, Interception | HTTPS, MFA |
| Web App → Auth | Auth Requests | High | Tampering, Spoofing | Token signing, TLS |
| Auth → Database | Query + Token | Critical | Injection, Disclosure | Prepared statements, Encryption |
| Database → API | Customer Data | Critical | Disclosure, Tampering | Encryption, Access controls |
| API → Customer | Reports | High | Disclosure, Tampering | HTTPS, Data masking |

## DFD Creation Guide for Recruitment Portfolio

### Step 1: Tools to Use
- **Draw.io** (Free, browser-based): https://draw.io
- **Lucidchart** (Professional): https://lucidchart.com
- **PowerPoint** (Microsoft): Built-in shapes and connectors

### Step 2: Creating the Diagram
1. **Create Entities:**
   - Rectangle: Web Application, AWS, API
   - Cylinder: Database (RDS, S3)
   - Person/Icon: Customer/External User
   - Circle: Authentication System

2. **Create Data Flows (Arrows):**
   - User → Web App: "Login"
   - Web App → Auth: "Auth Request"
   - Auth → Database: "Query + Token"
   - Database → API: "Results"
   - API → User: "Report"

3. **Add STRIDE Labels:**
   - Color-code arrows or add callouts
   - Red arrows = Critical threats
   - Yellow arrows = High threats
   - Example: "S, I (Spoofing, Information Disclosure)"

### Step 3: Export as PNG
1. **Draw.io:** File → Export as → PNG → "Include Diagram Background" ✓
2. **Lucidchart:** File → Download → PNG
3. **PowerPoint:** Right-click → Save as Picture → PNG

### Step 4: Upload to GitHub
1. Save as: `Data_Flow_Diagram.png`
2. Upload to `03_Threat_Modelling/` folder
3. Commit with message: "Add DFD showing data flows and STRIDE threat mapping"

## Interview-Ready Tips

✅ **Explain the flow:** "Data enters via login, flows through authentication, reaches the database, and exits as reports"

✅ **Map threats:** "Notice how the database interaction has tampering threats (SQL injection) and information disclosure risks (misconfiguration)"

✅ **Show controls:** "We mitigate spoofing with MFA, tampering with input validation, and disclosure with encryption"

✅ **Mention tools:** "I used Draw.io to create this, which is a standard practice in security assessments"

## Next Steps

1. Create diagram in Draw.io using ASCII template above as reference
2. Add STRIDE threat labels to each component
3. Export as PNG
4. Upload alongside this guide
5. Update README to reference the DFD in threat modeling section

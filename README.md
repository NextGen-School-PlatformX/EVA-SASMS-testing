<div align="center">

<img src="https://img.shields.io/badge/SASMS-Test%20Management-FFC600?style=for-the-badge&logoColor=black" />
<img src="https://img.shields.io/badge/Version-V1%20%26%20V2-black?style=for-the-badge" />
<img src="https://img.shields.io/badge/Test%20Cases-282%20Total-2E7D32?style=for-the-badge" />
<img src="https://img.shields.io/badge/Bugs%20Tracked-20-C62828?style=for-the-badge" />
<img src="https://img.shields.io/badge/Risks%20Identified-15-F57F17?style=for-the-badge" />
<img src="https://img.shields.io/badge/Tech-Next.js%20%7C%20Express%20%7C%20SQLite-1565C0?style=for-the-badge" />

<br/><br/>

```
████████╗███████╗███████╗████████╗    ███╗   ███╗ ██████╗ ███╗   ███╗████████╗
╚══██╔══╝██╔════╝██╔════╝╚══██╔══╝    ████╗ ████║██╔════╝ ████╗ ████║╚══██╔══╝
   ██║   █████╗  ███████╗   ██║       ██╔████╔██║██║  ███╗██╔████╔██║   ██║   
   ██║   ██╔══╝  ╚════██║   ██║       ██║╚██╔╝██║██║   ██║██║╚██╔╝██║   ██║   
   ██║   ███████╗███████║   ██║       ██║ ╚═╝ ██║╚██████╔╝██║ ╚═╝ ██║   ██║   
   ╚═╝   ╚══════╝╚══════╝   ╚═╝       ╚═╝     ╚═╝ ╚═════╝ ╚═╝     ╚═╝   ╚═╝   
```

# 🏫 SASMS — Test Management Master Workbook

### *School Admin & Student Management System — Complete QA Test Suite*

**A professional, comprehensive test management document covering 282 test cases across 14 functional modules, 20 tracked bugs (V1 → V2), and a full risk assessment for the SASMS platform.**

[Overview](#-overview) · [Workbook Structure](#-workbook-structure) · [Test Modules](#-test-modules) · [Bug Tracker](#-bug-tracker-v1--v2) · [Risk Register](#️-risk-register) · [How to Use](#-how-to-use-this-workbook) · [Tech Stack](#-tech-stack-under-test)

</div>

---

## 📌 Overview

This workbook is the **single source of truth** for all testing activities on the SASMS (School Admin & Student Management System) project. It documents test cases for two major software versions:

| | V1 — Baseline | V2 — Fixed & Enhanced |
|---|---|---|
| **Purpose** | Initial release — known bugs present | Production-ready release with all critical fixes |
| **Focus** | Functional correctness | Security hardening + Feature additions |
| **Admissions** | Simple Accept / Reject only | Full 5-stage pipeline (Pending → Exam → Interview → Accept) |
| **Dashboard KPIs** | Hardcoded mock data | Live data from real DB queries |
| **Security** | Multiple critical gaps | Rate limiting, RBAC scope, JWT blacklist, XSS sanitization |
| **GPS Attendance** | No location validation | Geo-fencing with distance enforcement |

> **Generated:** 06 March 2026 &nbsp;|&nbsp; **Tester:** Fill before execution &nbsp;|&nbsp; **Environment:** `localhost:3000` (Frontend) / `localhost:5001` (API)

---

## 📂 Workbook Structure

The workbook contains **17 sheets** organized into three categories:

### 🧪 Test Suite Sheets (14 sheets — 282 test cases)

| # | Sheet | Area Covered | V1 Cases | V2 Cases |
|---|---|---|:---:|:---:|
| 1 | 🔐 Auth & RBAC | Login, JWT, Role-Based Access Control | 18 | 18 |
| 2 | 📝 Admissions V1 | Applicant Registration, Document Upload | 22 | — |
| 3 | 📝 Admissions V2 | Multi-Stage Pipeline (Exam / Interview / Accept) | — | 28 |
| 4 | 🎓 Student Affairs | Student Profile, Grades, Document Management | 20 | 20 |
| 5 | 📅 Attendance QR | QR Session Creation, GPS Marking, Auto-Absent | 24 | 24 |
| 6 | 📊 Attendance Analytics | Cross-Session Analytics, Drill-Down Reports | 14 | 14 |
| 7 | 💬 Complaints & Tickets | Create Ticket, Staff Response, Resolve, Chat UI | 18 | 18 |
| 8 | 💰 Finances | Fee Setup, Payments, Overrides, Bus Fees | 26 | 26 |
| 9 | 🗓️ Events & Activities | Event CRUD, Shared Activity Manager, Notifications | 16 | 16 |
| 10 | 👑 SuperAdmin Dashboard | KPI Dashboard, Dept Governance, Audit Logs | 20 | 20 |
| 11 | 🔔 Notifications | In-App & Email Notifications, Delivery Verification | 14 | 14 |
| 12 | 🛡️ Security & Auth Edge | JWT Expiry, CSRF, XSS, Rate Limits, Auth Bypass | 20 | 20 |
| 13 | 📱 UI / UX Regression | Responsive Design, Animations, Theme, MUI | 18 | 18 |
| 14 | ⚡ Performance | Load Testing, API Response Times, DB Queries | 12 | 12 |

### 📋 Analysis & Reporting Sheets (2 sheets)

| Sheet | Purpose |
|---|---|
| 🐛 Bug Report V1→V2 | 20 confirmed V1 defects with root cause, impact, and V2 fix descriptions |
| ⚠️ Risk Analysis | 15 future risks rated by Likelihood × Impact with mitigation recommendations |

### 🗂️ Navigation Sheet (1 sheet)

| Sheet | Purpose |
|---|---|
| 📋 INDEX | Master table of contents with sheet directory and status legend |

---

## 🧪 Test Modules

### 🔐 Auth & RBAC — 18 Test Cases

Tests the entire authentication system and role-based access control across all four user roles.

**Roles Covered:** SuperAdmin · Staff · Student · Applicant

**Key Scenarios:**
- Valid login for each role → correct dashboard redirect
- Invalid credentials, wrong password, non-existent email
- **V1 Bug:** Role mismatch (student credentials + staff role selection) allows partial staff panel access
- **V2 Fix:** Server validates JWT role claim against DB record. Mismatch → 401 returned
- RBAC enforcement — Staff cannot access `/superadmin/*` routes
- Students cannot access any admin endpoints
- JWT token storage, expiry handling, and logout behavior

**Test IDs:** `AUTH-001` through `AUTH-018`

---

### 📝 Admissions V1 — 22 Test Cases

Tests the baseline V1 admissions workflow: registration, document upload, and simple admin review.

**Key Scenarios:**
- Full registration form submission → `role=APPLICANT`, `status=PENDING`
- Document upload: Ministry Result, Birth Certificate, National ID, Fee Receipt
- Invalid file type rejection (.exe, .js blocked)
- **V1 Known Bug (ADM1-007):** No file size validation — files over 10MB crash the upload endpoint
- Admin staff reviews applicant list, opens profiles, accepts or rejects
- Email notification on accept/reject decision

**Test IDs:** `ADM1-001` through `ADM1-022`

---

### 📝 Admissions V2 — 28 Test Cases

Tests the fully redesigned V2 multi-stage admissions pipeline — the most significant feature addition.

**The 5-Stage Pipeline:**

```
PENDING ──► UNDER_REVIEW ──► EXAM_SCHEDULED ──► INTERVIEW_SCHEDULED ──► ACCEPTED
                                                                        └──► REJECTED
```

**Key Scenarios:**
- Application starts at `PENDING` — animated stepper visible on applicant dashboard
- Staff moves application to `UNDER_REVIEW` → in-app notification sent to applicant
- Exam scheduling: date, location, notes saved → `EXAM_SCHEDULED` → email sent
- **Applicant Exam Info Card:** Beautiful UI card shows date, time, location when exam scheduled
- Interview scheduling after exam stage → `INTERVIEW_SCHEDULED`
- Final acceptance → role promoted to `STUDENT` → enrollment button appears
- Rejection at any stage with reason → polite rejection email sent
- Pipeline stepper shows current stage, completed stages, and rejection states

**API Endpoints Tested:**
- `POST /admissions/:id/schedule-exam`
- `POST /admissions/:id/schedule-interview`
- `POST /admissions/:id/accept`
- `POST /admissions/:id/reject`

**Test IDs:** `ADM2-001` through `ADM2-028`

---

### 🎓 Student Affairs — 20 Test Cases

Tests the student profile management and academic records system.

**Key Scenarios:**
- Staff views all enrolled students with search and filter
- Individual student profile drawer: name, ID, department, contact, documents
- Grade management: add, update, and validate grades per subject
- **V1 Bug (AFF-005):** Grades outside 0–100 range accepted (e.g., 150, -10)
- **V2 Fix:** Backend validation enforces 0–100 range on grade creation
- Student views own grades → GPA calculated and displayed in V2
- Document management: view uploaded applicant docs, upload additional files
- **V2 Addition:** Audit log records every `UPDATE_STUDENT` event with staff ID

**Test IDs:** `AFF-001` through `AFF-020`

---

### 📅 Attendance QR — 24 Test Cases

Tests the QR-based attendance session management system with GPS validation.

**How It Works:**
1. Staff creates a session with title, class, GPS location, and duration
2. QR code generated and displayed on screen (periodically refreshed in V2)
3. Students scan QR → GPS location validated against session location
4. Within range → `PRESENT` / Late → `LATE` / Not scanned → auto-marked `ABSENT`

**Key Scenarios:**
- Session creation → QR generation → QR display and scan
- **V1 Gap (ATT-004):** No GPS validation — any student anywhere can mark attendance
- **V2 Fix:** Distance calculated between student GPS and session GPS. Rejection if out of range
- LATE status: scanned after grace period but before session end
- Auto-absent job: all unscanned students marked `ABSENT` after session closes
- **V1 Bug (ATT-010):** Duplicate QR scan creates two attendance records
- **V2 Fix:** Idempotency check — second scan for same student/session is ignored
- Staff views session attendance report with counts: Present X / Late Y / Absent Z

**Test IDs:** `ATT-001` through `ATT-024`

---

### 📊 Attendance Analytics — 14 Test Cases

Tests the cross-session analytics dashboard (repurposed from Employee Attendance module).

**Key Scenarios:**
- Analytics page loads with MiniRing SVG charts per session showing present/absent %
- Click session ring → drill-down to full per-student report (DataTable)
- Summary stat cards: Total Present (green) / Late (gold) / Absent (red) across all sessions
- Date range filter: only sessions within selected period shown
- Class filter: analytics scoped to selected class
- Attendance trend chart: weekly attendance % as line/bar chart
- **At-Risk Students section:** students below 75% threshold highlighted

**Test IDs:** `ANA-001` through `ANA-014`

---

### 💬 Complaints & Support Tickets — 18 Test Cases

Tests the student support ticket system with real-time chat interface.

**Key Scenarios:**
- Student creates ticket (title + description) → `status=OPEN`
- **V1 Bug (CMP-002):** Short descriptions (1 character) accepted
- **V2 Fix:** Minimum 20 characters required (frontend + backend validation)
- Staff views all complaints with real-time status color coding: Open=blue / In Progress=gold / Resolved=green
- Staff sends chat response → AnimatePresence animation on new message (V2)
- Status progression: `OPEN` → `IN_PROGRESS` → `RESOLVED`
- On resolution: ticket locked from further edits, student receives notification
- Student chat view: PersonIcon for student vs SupportAgent icon for staff
- **V1 Bug (CMP-008):** `GET /api/complaints` returns all complaints without user filter
- **V2 Fix:** Only authenticated student's own complaints returned

**Test IDs:** `CMP-001` through `CMP-018`

---

### 💰 Financial Management — 26 Test Cases

Tests the full financial management system for fees, payments, and bus routes.

**Key Scenarios:**
- SuperAdmin sets tuition fee and bus fees per route
- Payment recording: amount, date, method → balance auto-calculated
- Partial payment: outstanding balance = TotalFee − SUM(Payments)
- **V1 Bug (FIN-005):** Overpayment creates negative balance with no error
- **V2 Fix:** Warning shown. Credit balance displayed. No further payment accepted
- **V2-Only Feature:** Fee override for specific students (SuperAdmin only) with audit trail
- Bus fee automatically added to enrolled students on assigned routes
- Outstanding fee list: sorted by amount, color-coded, filterable by dept/class
- **V1 Bug (FIN-007):** No role restriction on fee override endpoint — any staff can override
- **V2 Fix:** SUPER_ADMIN role validation enforced in middleware

**Test IDs:** `FIN-001` through `FIN-026`

---

### 🗓️ Events & Activities — 16 Test Cases

Tests the campus events and school activities management system.

**Key Scenarios:**
- Admin creates event (title, date, location, description) → SharedActivityManager component
- SuperAdmin-specific creation: `isSuperAdmin=true` flag unlocks additional fields
- Event list sorted chronologically, color-coded by event type (V2)
- Edit and delete events (V2: soft delete / archival — not hard delete)
- **V1 Gap (EVT-006):** No event notifications sent to students
- **V2 Fix:** All enrolled students receive in-app notification when new event created
- Student view: formatted card or calendar view, past events greyed out
- Security: student JWT → `POST /api/events` → **403 Forbidden**

**Test IDs:** `EVT-001` through `EVT-016`

---

### 👑 SuperAdmin Dashboard — 20 Test Cases

Tests the governance layer available exclusively to the SuperAdmin role.

**Key Scenarios:**
- **V1 Bug (SA-001 through SA-005):** All KPI cards show hardcoded `MOCK_STATS`:
  - Total Students hardcoded as `1540`
  - Pending Applications hardcoded as `12`
  - Outstanding Fees hardcoded as `45`
  - Open Complaints hardcoded as `7`
  - Application bar chart (`CHART_DATA_APPS`) hardcoded Jan–May mock values
- **V2 Fix:** All KPIs wired to live `/api/stats` endpoint with real DB counts
- Department CRUD: create, assign staff, remove (soft delete in V2)
- Audit log: cryptographically signed entries for every governance action
- Audit log viewer: filterable by action type, user, date range

**Test IDs:** `SA-001` through `SA-020`

---

### 🔔 Notifications — 14 Test Cases

Tests both in-app and email notification delivery across all user roles.

**In-App Notifications (V2 only):**
- Notification bell icon with red unread count badge
- Click bell → panel opens with message, timestamp, action link per notification
- Mark single notification read → badge count decreases
- Mark all read → badge disappears
- Privacy: `GET /api/notifications` returns only authenticated user's notifications

**Email Notifications (V2 only — SMTP integration tests):**
- Exam scheduled → applicant receives email within 60 seconds, no spam classification
- Welcome email on account creation with login instructions
- Auto-absent job fires → in-app only (no email — avoids inbox spam)
- Rejection email: polite tone, no further action links

**V1 Status:** No notification system present. All notification tests are V2-only.

**Test IDs:** `NOT-001` through `NOT-014`

---

### 🛡️ Security & Auth Edge Cases — 20 Test Cases

Tests security vulnerabilities, edge cases, and hardening measures.

**Coverage Areas:**

| Category | What's Tested |
|---|---|
| **XSS** | `<script>` injection in name field; `<img onerror>` in complaint description |
| **SQL Injection** | `' OR 1=1--` in search fields (Prisma ORM parameterization validation) |
| **CSRF** | Cross-origin POST requests without CSRF token |
| **Rate Limiting** | Login: 5 failures → 15-minute lockout; API: 100 req/min per user |
| **IDOR** | Student accessing another student's profile via ID manipulation |
| **IDOR (Dept)** | Staff accessing students from a different department |
| **JWT Expiry** | Expired/modified tokens — must return 401 (not accept) |
| **Concurrent Sessions** | Multiple simultaneous logins with same account |
| **Auth Bypass** | Direct URL navigation to protected routes without valid JWT |

**Test IDs:** `SEC-001` through `SEC-020`

---

### 📱 UI / UX Regression — 18 Test Cases

Tests visual consistency, responsiveness, and animation quality across the application.

**Key Scenarios:**
- **Responsive Design:** Dashboard at 375px (mobile) — MUI Grid stacks correctly
- **Table Scrolling:** DataTables have `overflow-x: auto` on mobile
- **Theme Consistency:** `GOLD = #FFC600` used uniformly across all components
- **Framer Motion:** Smooth 60fps animations on attendance and complaints pages
- **Button Hover:** `translateY(-1px)` lift effect defined in `theme.ts`
- **Drawer Animation:** Smooth slide-in / slide-out (V2 upgrade)
- **Loading States:** MUI `CircularProgress` shown on Slow 3G (no blank screen)
- **Error States:** MUI Alert shown on API failure (no white-screen-of-death)
- **Empty States:** Friendly empty state UI when no data available

**Test IDs:** `UI-001` through `UI-018`

---

### ⚡ Performance — 12 Test Cases

Tests response times, load capacity, and query optimization benchmarks.

| Test ID | Benchmark | V1 Actual | V2 Target |
|---|---|---|---|
| PERF-001 | Login API response | 800–1200ms | < 500ms |
| PERF-002 | GET students list (500 records) | 1–3s (no pagination) | < 200ms (paginated) |
| PERF-003 | Admin dashboard FCP | 3–5s | < 2s (SSR/ISR + skeleton) |
| PERF-004 | 50 concurrent users error rate | > 5% | < 1% |
| PERF-005 | DB queries on student list | N+1 queries | 1 query (JOIN/include) |
| PERF-006 | QR code generation | 1–2s | < 300ms |
| PERF-007 | 5MB document upload | Hangs (no timeout) | < 5s (with progress indicator) |
| PERF-008 | JS bundle size (gzipped) | Not analyzed | < 1MB |

**Test IDs:** `PERF-001` through `PERF-012`

---

## 🐛 Bug Tracker: V1 → V2

Complete log of all **20 confirmed defects** discovered in V1 and their resolution status in V2.

| Bug ID | Severity | Module | Title | V2 Status |
|---|:---:|---|---|:---:|
| BUG-001 | 🔴 Critical | Auth | Role mismatch login allows wrong dashboard access | ✅ Fixed |
| BUG-002 | 🔴 Critical | Auth | Missing department scope on student queries | ✅ Fixed |
| BUG-003 | 🟠 High | Admissions | V1 simple accept/reject — no pipeline stages | ✅ Fixed (New Feature) |
| BUG-004 | 🟠 High | Admissions | Duplicate email registration not blocked | ✅ Fixed |
| BUG-005 | 🟠 High | Admissions | Duplicate applications possible per user | ✅ Fixed |
| BUG-006 | 🔴 Critical | Auth | Expired JWT not invalidated — still grants access | ✅ Fixed |
| BUG-007 | 🔴 Critical | Finances | No fee override role restriction | ✅ Fixed |
| BUG-008 | 🟠 High | Finances | Overpayment creates negative balance with no error | ✅ Fixed |
| BUG-009 | 🟠 High | Attendance | GPS location not validated — mark from anywhere | ✅ Fixed |
| BUG-010 | 🟠 High | Attendance | Duplicate QR scan creates multiple attendance records | ✅ Fixed |
| BUG-011 | 🟠 High | Complaints | Student can see all complaints (not just own) | ✅ Fixed |
| BUG-012 | 🟡 Medium | Dashboard | KPI cards show hardcoded mock data | ✅ Fixed |
| BUG-013 | 🟡 Medium | Attendance | Absent not auto-marked when session ends | ✅ Fixed |
| BUG-014 | 🟡 Medium | XSS | XSS injection via name field in profile | ✅ Fixed |
| BUG-015 | 🟠 High | Security | No rate limiting on login endpoint | ✅ Fixed |
| BUG-016 | 🟡 Medium | Files | Invalid file types accepted in document upload | ✅ Fixed |
| BUG-017 | 🟡 Medium | Audit | Admission acceptance not logged in AuditLog | ✅ Fixed |
| BUG-018 | 🟠 High | Auth | Session not fully destroyed on logout — JWT reusable | ✅ Fixed |
| BUG-019 | 🟡 Medium | Grades | Grade value out of range (0–100) accepted | ✅ Fixed |
| BUG-020 | 🔴 Critical | IDOR | Student can access other students profile via ID manipulation | ✅ Fixed |

**Bug Severity Summary:**

| Severity | Count | Description |
|---|:---:|---|
| 🔴 Critical | 5 | Data exposure, auth bypass, financial manipulation |
| 🟠 High | 9 | Security gaps, data integrity, privacy violations |
| 🟡 Medium | 6 | Validation, audit logging, UX issues |

---

## ⚠️ Risk Register

**15 identified risks** for future sprints, each scored by `Likelihood (1–5) × Impact (1–5)`.

### 🔴 Critical Risks (Score 20–25)

| Risk ID | Risk Title | L | I | Score | Recommended Action |
|---|---|:---:|:---:|:---:|---|
| RSK-001 | SQLite single-file DB bottleneck at scale | 5 | 5 | **25** | Migrate to PostgreSQL + PgBouncer connection pooling |
| RSK-002 | JWT stored in localStorage — XSS token theft | 4 | 5 | **20** | Move to HttpOnly cookies + CSRF token + short-lived tokens (15min) + refresh rotation |
| RSK-004 | File storage on local disk — not production-ready | 5 | 4 | **20** | Migrate to AWS S3 / Cloudflare R2 / MinIO with signed URLs |
| RSK-005 | No 2FA for admin/SuperAdmin accounts | 4 | 5 | **20** | TOTP-based 2FA (Google Authenticator). Mandatory for SUPER_ADMIN |

### 🟠 High Risks (Score 9–19)

| Risk ID | Risk Title | L | I | Score | Recommended Action |
|---|---|:---:|:---:|:---:|---|
| RSK-003 | No Content Security Policy (CSP) header | 3 | 4 | **12** | Define strict CSP via Helmet.js. Test all MUI components with nonce-based CSP |
| RSK-006 | No database backup strategy | 3 | 5 | **15** | Automated daily backups. Post-Postgres migration: `pg_dump` → S3 |
| RSK-008 | SMTP credentials in .env — no secrets management | 3 | 4 | **12** | HashiCorp Vault / AWS Secrets Manager / Doppler. `.env` in `.gitignore` |
| RSK-009 | Single-point-of-failure Express server | 4 | 4 | **16** | PM2 cluster mode + health checks + reverse proxy (Nginx) |
| RSK-010 | No data retention policy — sensitive data kept indefinitely | 3 | 4 | **12** | Define PDPA/FERPA retention schedule. Auto-archive/purge old records |
| RSK-012 | Synchronous email sending blocks API responses | 4 | 3 | **12** | Introduce message queue (BullMQ/Redis) for async email delivery |
| RSK-014 | No PDPA/FERPA compliance documentation | 3 | 4 | **12** | Document data handling, consent flows, and student data rights |
| RSK-015 | Winston logs may capture sensitive request bodies | 3 | 4 | **12** | Add field masking for passwords, tokens, and personal data in Winston config |

### 🟡 Medium Risks (Score ≤ 9)

| Risk ID | Risk Title | L | I | Score | Recommended Action |
|---|---|:---:|:---:|:---:|---|
| RSK-007 | No input length limits on large text fields | 3 | 3 | **9** | Add `maxLength` on all text inputs in both MUI `inputProps` and Zod schemas |
| RSK-011 | QR session tokens not rate-limited per IP | 3 | 3 | **9** | Rate limit `/api/attendance/scan` to 10 req/min per IP |
| RSK-013 | Playwright test credentials may be hardcoded | 3 | 3 | **9** | Move test credentials to `.env.test` file. Never commit test secrets |

---

## 📊 Test Coverage Summary

```
Total Test Cases:     282
├── V1 Tests:         242  (18 modules × V1 scenarios)
└── V2 Tests:         262  (including 28 new pipeline tests)

By Priority:
├── 🔴 Critical:       ~38%   (authentication, security, data integrity)
├── 🟠 High:           ~41%   (functional flows, performance)
├── 🟡 Medium:         ~14%   (edge cases, UX)
└── 🟢 Low:             ~7%   (cosmetic, minor behaviors)

By Type:
├── Functional:        ~52%
├── Security:          ~18%
├── Negative/Edge:     ~16%
├── UI/UX:              ~8%
└── Performance:        ~6%
```

---

## ✅ Status Legend

Every test case has two status columns — one for V1 and one for V2:

| Status | Symbol | Meaning |
|---|:---:|---|
| **Pass** | ✅ | Test executed and all assertions met |
| **Fail** | ❌ | Test executed — expected result not achieved |
| **Blocked** | 🔶 | Cannot execute due to dependency or environment issue |
| **Not Run** | ⬜ | Test not yet executed in this cycle |
| **N/A** | ➡️ | Test not applicable to this version |

---

## 🚀 How to Use This Workbook

### Before Testing

1. **Fill in the tester name and date** in each sheet header:
   ```
   Tester: ___________  |  Date: ___________
   ```

2. **Confirm your environment** is running:
   - Frontend: `http://localhost:3000`
   - Backend API: `http://localhost:5001`
   - Database: SQLite `dev.db` with seed data

3. **Set up test accounts** for each role:
   - SuperAdmin account
   - Staff (Admissions dept) account
   - Staff (Affairs dept) account
   - Student account
   - Applicant account (PENDING status)

### During Testing

4. **Execute test cases** in sheet order (follow the INDEX for recommended sequence)

5. **For each test case**, fill in:
   - `Actual Result (V1)` column — what actually happened in V1
   - `Status V1` — ✅ / ❌ / 🔶 / ⬜
   - `Actual Result (V2)` — what happened after the fix
   - `Status V2` — ✅ / ❌ / 🔶 / ⬜
   - `Notes / Defect ID` — link to issue tracker if failed

6. **For new bugs found**, add a row to the `🐛 Bug Report` sheet with:
   - New Bug ID (e.g., `BUG-021`)
   - Severity, Module, Title, Root Cause, Impact
   - Steps to Reproduce, Expected vs Actual Behavior
   - V2 Fix Description (fill after fix is deployed)

### After Testing

7. **Update the Risk Register** (`⚠️ Risk Analysis`) if new risks are identified during testing

8. **Generate test summary** by counting ✅ / ❌ / 🔶 / ⬜ per module from the INDEX sheet

---

## 🛠 Tech Stack Under Test

| Layer | Technology | Version |
|---|---|---|
| **Frontend** | Next.js | 14 (App Router) |
| **UI Library** | MUI (Material UI) | v5 |
| **Animation** | Framer Motion | Latest |
| **Backend** | Express.js | — |
| **ORM** | Prisma | — |
| **Database** | SQLite (`dev.db`) | — |
| **Auth** | JWT (JSON Web Tokens) | — |
| **Email** | SMTP (Nodemailer) | — |
| **E2E Testing** | Playwright | — |
| **Logger** | Winston | — |

### Key API Patterns

```
Base URL:         http://localhost:5001/api
Auth Header:      Authorization: Bearer <jwt_token>
Content-Type:     application/json

Role Hierarchy:   SUPER_ADMIN > STAFF > STUDENT > APPLICANT

Protected Routes:
  /api/admin/*        → SUPER_ADMIN + STAFF
  /api/superadmin/*   → SUPER_ADMIN only
  /api/student/*      → STUDENT only
  /api/applicant/*    → APPLICANT only
```

---

## 📋 Test Case Columns Reference

Each test case sheet contains these standardized columns:

| Column | Description |
|---|---|
| **Test ID** | Unique identifier (e.g., `AUTH-001`, `ADM2-005`) |
| **Category** | Sub-module within the sheet (e.g., Login, RBAC, Pipeline) |
| **Test Case Name** | Clear, action-oriented test title |
| **Priority** | Critical / High / Medium / Low |
| **Type** | Functional / Negative / Security / UI / Performance / Integration |
| **Pre-conditions** | System state required before test execution |
| **Test Steps** | Numbered, reproducible steps |
| **Expected Result (V1)** | Expected behavior in V1 baseline (may note known bugs) |
| **Expected Result (V2)** | Expected behavior after V2 fixes |
| **Actual Result (V1)** | ← **Fill during V1 test execution** |
| **Status V1** | ← **✅ / ❌ / 🔶 / ⬜** |
| **Actual Result (V2)** | ← **Fill during V2 test execution** |
| **Status V2** | ← **✅ / ❌ / 🔶 / ⬜** |
| **Notes / Defect ID** | Known issues, bug references, special instructions |

---

<div align="center">

---

**SASMS Test Management Master** · Generated: 06 March 2026 · Version: V1 (Baseline) & V2 (Fixed)

*Built for the SASMS QA Team — Next.js 14 · Express.js · SQLite · Playwright*

</div>

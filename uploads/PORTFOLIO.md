# Rohith Raj Baggam — Portfolio Content & Design Brief

> **Purpose of this file:** A single, self-contained source of truth for designing a
> personal portfolio website. It contains the full narrative, every professional and
> personal project, and an in-depth skills breakdown for each project. A designer (human
> or AI) should be able to build the entire portfolio from this file alone — no other
> document is required.

---

## 0. How To Use This File (for the designer)

- **Sections 1–3** give identity, positioning, and the global skills matrix — use these for
  the hero, about, and skills sections.
- **Section 4** is professional experience — one company, six projects. Each project is a
  case study: summary, role, what was built, and an **in-depth skills** list.
- **Section 5** is personal / open-source projects — five projects, each a case study with
  its own deep skills list.
- **Section 6** is education, certifications, and soft skills.
- **Section 7** gives design direction (suggested information architecture, tone, ordering,
  and per-project "hero metric" hooks) so the portfolio reads well.
- Every project has a **`skills:`** block written as a flat, comma-scannable list so it can
  be rendered directly as tags/chips.

---

## 1. Identity

| Field | Value |
|---|---|
| **Name** | Rohith Raj Baggam |
| **Headline** | Technical Lead · Backend Engineer · Solution Architect · Database Engineer |
| **Location** | Hyderabad, India |
| **Email** | rohithrajbaggam@gmail.com · baggamrohithraj@gmail.com |
| **Phone** | +91 70933 89378 |
| **GitHub** | https://github.com/rohith-baggam |
| **LinkedIn** | https://www.linkedin.com/in/rohith-raj-baggam |
| **PyPI** | https://pypi.org/user/rohith-baggam |
| **Experience** | 3 years (Jul 2023 – Present) at Aptagrim Limited |
| **Open-source** | 2 published PyPI packages |

---

## 2. Professional Summary / Positioning

Technical Lead and Backend Engineer with **3 years** at Aptagrim, building scalable,
high-performance systems in **Python** and **Java**. Hands-on across **Django/DRF, FastAPI,
Spring Boot and Quarkus**, with deep work in multi-tenant microservices, real-time
platforms, and database engineering over **PostgreSQL, MSSQL, MongoDB and Neo4j**. Have owned
the full delivery arc — backend developer, technical lead, solution architect, database
engineer and research analyst. Author of two published PyPI packages.

**One-line version (for hero):**
> Backend engineer and technical lead who designs multi-tenant, real-time platforms end to
> end — architecture, database engineering, and delivery.

**Three things that define the work:**
1. **Multi-tenant, distributed backends** — per-tenant databases, runtime DB routing,
   control-plane/data-plane splits, cross-service provisioning with transactional rollback.
2. **Real-time & async systems** — Django Channels websockets, Celery pipelines, LiveKit,
   split Redis topologies, encrypted messaging.
3. **Database & legacy engineering** — 100M+ row query tuning, zero-downtime Java→Python
   migration on a live 25-year MSSQL schema, safe non-destructive schema-sync.

---

## 3. Global Skills Matrix

| Category | Technologies |
|---|---|
| **Languages** | Python, Java (21), SQL, Dart, C#, PowerShell, Bash |
| **Frameworks** | Django, Django REST Framework (DRF), FastAPI, Spring Boot, Quarkus, Hibernate ORM / Panache, Jakarta EE |
| **Databases** | PostgreSQL, MySQL/MariaDB, MSSQL (SQL Server), MongoDB, Neo4j, SQLite |
| **Async / Realtime** | Celery, Celery Beat, Redis, WebSockets, Django Channels, Daphne, ASGI |
| **DevOps / Infra** | Nginx, Gunicorn, Uvicorn, Docker, Docker Compose, PgBouncer, GitLab CI, GitHub Actions, systemd |
| **Cloud / Storage** | E2E Compute Nodes & EOS (primary), Azure Blob, MinIO/S3, GCP, AWS |
| **Integrations** | LiveKit, Google Maps, Mappls (MapMyIndia), OSRM, OnlyOffice, FFmpeg, Firebase FCM, Mailcow (IMAP/SMTP) |
| **Data / ETL** | pandas, openpyxl, xlsxwriter, numpy, mimesis |
| **Security** | JWT (HS256/RS256), PyJWT, AES-GCM, HMAC blind-index, Fernet/cryptography, SNMPv3 auth/privacy, NTLM |
| **Quality / Tooling** | Git, SonarQube, pre-commit, Black, Autoflake, Swagger/OpenAPI, Postman, DBML, WeasyPrint, Jinja2 |

**Roles demonstrated across the body of work:** Backend Engineer · Python Developer · Django
Developer · Solution Architect · Technical Lead · Database Engineer · Platform Engineer ·
Distributed Systems / Microservices Engineer · Realtime Systems Engineer · Research Analyst.

---

## 4. Professional Experience

### Aptagrim Limited — Backend Engineer → Technical Lead
*Hyderabad, India · Jul 2023 – Present*

Six production projects, spanning core backend development, technical leadership, solution
architecture, and database engineering. Ordered newest-first below.

---

### 4.1 — PsyHire v2 — Multi-Tenant ATS / Recruitment Platform
**Role:** Solution Architect & Core Backend Developer
**Timeline:** May 2026 – Present
**Hook metric:** Three Django microservices, a per-tenant database *and* per-company Redis
logical DB, resolved at runtime through a Redis-backed tenant registry.

**What it is:** A distributed, multi-tenant recruitment and hiring platform built as three
Django services — a control-plane/admin service, an auth/identity service, and a recruiting
application service — sharing a Git-submodule platform layer.

**What I built / architected:**
- Architected the platform as **three Django microservices** (admin/control-plane, auth,
  application), giving **each tenant its own PostgreSQL database** and **each company its own
  Redis logical DB**, resolved at runtime through a **Redis-backed tenant registry**
  (`tenant_company_details:<subdomain>`).
- Designed **cross-service tenant provisioning**: the admin service creates physical
  Postgres DBs and allocates Redis logical DBs, then orchestrates migrations on sibling
  services via `X-Service-Token` internal calls — with **transactional rollback across
  databases and Redis** when any sibling migration fails, so partial onboarding never leaves
  orphaned state.
- Built a **shared Git-submodule platform layer** reused by every service:
  **ContextVar-based tenant routing**, runtime registration of tenant DB aliases into
  `settings.DATABASES`, custom **PyJWT/HS256** authentication, internal service-token
  permissions with **constant-time comparison**, and a generic
  handler/serializer/view/model framework (`CoreGenericBaseHandler`, `CoreGenericModel`, etc.).
- Implemented `tenant_atomic()` to bind `transaction.atomic()` to the active tenant DB
  (Django atomic blocks don't follow database routers) and a `TenantDatabaseRouter` for
  read/write routing.
- Integrated **LiveKit** interviews with **Room Composite Egress** recording to MinIO/S3,
  signed webhook validation, and per-interview agent dispatch.
- Integrated **external AI APIs** for resume parsing/scoring, JD generation, and skill
  extraction — with an `MLApiLogModel` that logs endpoint/timing/payloads and sanitizes file
  uploads to metadata only.
- Built **Celery-driven hiring-pipeline automation**: resume screening, stage auto-advance,
  applicant/job expiry, and interview reminders — tenant-aware periodic tasks iterating all
  tenants, using `select_for_update(skip_locked=True)` batching.
- Built **Django Channels** websockets for live pipeline moves, notifications, and applicant
  chat, backed by a dedicated Redis channel layer.
- Built an operational admin dashboard (server-rendered Jinja/Django) with a guarded SQL
  editor (statement classification, destructive-query confirmation, 30s timeout, 10k row
  cap, audit logging), Redis explorer, and provisioning status polling.
- Integrated a **Mailcow** mailbox subsystem (IMAP over verified SSL, SMTP/STARTTLS,
  thread-aware replies, scheduled mail via Celery) with Fernet-encrypted credentials.

**skills:** Python 3.11+, Django 5, Django REST Framework, multi-tenant architecture,
control-plane/data-plane split, per-tenant PostgreSQL provisioning, per-company Redis logical
DB allocation, Redis-backed tenant registry, ContextVar tenant routing, runtime DB alias
registration, TenantDatabaseRouter, cross-service migration orchestration, X-Service-Token
internal APIs, transactional rollback across DB + Redis, Git submodule platform layer,
generic DRF handler/serializer/view framework, PyJWT HS256 auth, Redis-backed session/token
validation, constant-time token comparison, Celery, Celery Beat, tenant-aware periodic tasks,
select_for_update(skip_locked), Django Channels, ASGI websockets, LiveKit, Room Composite
Egress recording, signed webhook validation, MinIO/S3 object storage, external AI/ML API
integration, ML API logging, Mailcow, IMAP/SMTP, Fernet encryption, Docker Compose, pre-commit,
Black, python-decouple, bulk_create, JSONField modeling, database indexing.

---

### 4.2 — Ping (AptaOne Ping) — Enterprise Communication & Collaboration Platform
**Role:** Technical Lead & Core Backend Developer / Solution Architect
**Timeline:** Apr 2026 – May 2026
**Hook metric:** A self-hosted Slack + Zoom + Google-Calendar-in-one, with AES-GCM encrypted
messaging and **HMAC blind-index search over encrypted messages**.

**What it is:** A self-hosted enterprise communication platform — chat, files, calendar,
meetings, mail, and notifications — built as a modular monolith across domain-focused Django
services (auth-service + communication-service).

**What I built / architected:**
- **Led backend and solution architecture** for the whole platform (chat, files, calendar,
  meetings, mail, notifications) on Django, DRF, Channels and Celery.
- Designed a **split Redis topology**: one instance for the user/session store and encrypted
  mailbox credentials (6379), another for the Celery broker, Django Channels layer, pub/sub,
  watcher state, and distributed locks (6380) — isolating real-time fan-out from auth state
  under load. `request.user` is reconstructed from Redis payloads instead of hitting Postgres
  on every request.
- Built **secure real-time messaging**: **AES-GCM** encrypted message content,
  conversation-level shared-secret encryption of attachment URLs, and **HMAC blind-index
  search over encrypted messages** using inverted token indexes; plus replies, threads,
  forwards, reactions, mentions, read receipts, bookmarks, pins, scheduled messages, and
  edit/delete audit history with idempotent message creation.
- Integrated **OnlyOffice** document editing via **JWT-signed editor sessions** with callback
  validation, stale-save protection, permission-aware view/edit modes, and save-back into
  private object storage.
- Integrated **LiveKit** meetings/recordings: authenticated + guest tokens, room lifecycle,
  recording egress, webhook reconciliation, participant controls, and recordings posted back
  into chat as file messages.
- Built notification infrastructure over **Firebase FCM** + WebSockets: device registration
  across Android/iOS/Web/Windows/macOS/Linux, multicast push, invalid-token deactivation, and
  notification analytics.
- Integrated **Mailcow** self-hosted email — IMAP/SMTP, Redis-backed encrypted mailbox
  credentials, mailbox provisioning/deactivation, inbox/thread APIs, scheduled email delivery,
  and mail-event notification triggers.
- Built a **FFmpeg/Pillow Celery media-compression pipeline** guarded by Redis distributed
  locks (records original/compressed size, compression time, deletes originals after a safety
  window).
- Owned production-readiness: **PgBouncer** connection pooling, Redis capacity separation,
  performance telemetry (p50/p95/p99, throughput, DB query count/time), 60+ admin dashboard
  modules, Celery Beat scheduling, and pre-commit quality gates.

**skills:** Python, Django, Django REST Framework, Django Channels, Celery, Celery Beat,
modular monolith architecture, solution architecture, split Redis topology, Redis-backed auth
& session state, Redis pub/sub, Redis distributed locks, AES-GCM encryption, HMAC blind-index
search over encrypted data, inverted token index, conversation-level shared-secret encryption,
real-time messaging, read receipts / reactions / mentions / scheduled messages, message audit
history, idempotency keys, OnlyOffice integration, JWT-signed editor sessions, callback
validation, LiveKit, guest/participant tokens, recording egress, webhook reconciliation,
Firebase FCM push notifications, multi-platform device registration, Mailcow, IMAP/SMTP,
encrypted mailbox credentials, scheduled email, FFmpeg, Pillow, media compression pipeline,
private S3/MinIO storage, presigned URLs, PgBouncer connection pooling, performance telemetry,
admin dashboards, pre-commit.

---

### 4.3 — AIN — Network Monitoring & Discovery Platform (SolarWinds-class)
**Role:** Database Engineer & Research Analyst
**Timeline:** Jan 2026 – Apr 2026
**Hook metric:** Protocol-based device discovery across **SNMP v1/v2c/v3, WMI, SSH and ICMP**,
normalized into one protocol-agnostic inventory schema; time-series analytics over NetFlow,
IPFIX, syslog, and Windows Event Logs.

**What it is:** A network monitoring and discovery platform in the SolarWinds problem space —
built as Python/FastAPI microservices (discovery, credential backend, monitoring analytics)
plus Linux/Windows collector automation and a Windows installer.

**What I built / researched:**
- Engineered **multi-tenant async FastAPI + SQLAlchemy 2.x backends** with **runtime
  per-tenant database routing** (tenant connection strings resolved from a parent DB, cached
  async engines) and **safe, non-destructive schema-sync migrations** — adds tables/columns,
  never drops — with a change report.
- Built **time-series monitoring analytics** over PostgreSQL — CPU, RAM, virtual memory,
  disk, ICMP latency/loss, and interface bits/sec + utilization — using tuned **raw SQL**
  with **dynamic hourly/daily aggregation windows** (`DATE_TRUNC`, `build_datetime_range`,
  switching to daily buckets past 24h) shaped for chart workloads.
- Built **flow and log analytics**: **NetFlow / IPFIX** top-talkers, traffic distribution and
  protocol usage against `netflow_records` / `ipfix_records`; **syslog** facility/severity
  graphs; **Windows Event Log** level counts and time-series.
- Researched and implemented **protocol-based device discovery** across **SNMP v1/v2c**
  (`pysnmp`), **SNMPv3** (SHA auth + AES privacy via `UsmUserData`), **WMI** (`pywinrm`, NTLM),
  **SSH** hybrid enrichment (`paramiko`, Cisco CPU/memory CLI parsing), and **ICMP** ping —
  normalizing heterogeneous device facts (device, server, network, CPU/memory/disk/interface
  arrays) into a **single protocol-agnostic inventory schema**, with device classification.
- Designed a **repository/service/schema-layered** credential backend
  (`CredentialProfile` + protocol-specific child tables: WMI, SNMP-community, SNMPv3) with
  Pydantic v2 validation and eager loading via `selectinload`.
- Hardened access with **JWT/bearer auth middleware**, SNMPv3 auth/privacy, and tenant
  isolation; separated application/log/flow databases for workload isolation.
- Supported delivery with **Gunicorn/Uvicorn + Nginx**, plus collector automation:
  `softflowd` NetFlow/IPFIX exporters, `rsyslog` forwarding, `NXLog` Windows event forwarding,
  and a .NET 8 / WPF / WiX MSI / NSSM Windows installer.

**skills:** Python, async programming, FastAPI, Starlette, Uvicorn, Gunicorn, SQLAlchemy 2.x
(async), asyncpg, Pydantic v2, Alembic, PostgreSQL, multi-database routing, tenant-aware DB
resolution, cached async engines, non-destructive schema-sync migrations, raw SQL analytics,
time-window bucketing (hourly/daily), NetFlow, IPFIX, syslog, Windows Event Logs, SNMP v1/v2c,
SNMPv3 (SHA/AES), WMI, NTLM, SSH, paramiko, pysnmp, pywinrm, ICMP, CIDR/subnet expansion,
protocol normalization, device classification, repository/service/schema layering, JWT/bearer
middleware, CORS, tenant isolation, selectinload eager loading, contract/response-shape
testing (pytest, pytest-asyncio), softflowd, rsyslog, NXLog, systemd, Nginx, Certbot, Docker,
Docker Compose, Helm, Kubernetes, .NET 8, WPF, WiX MSI, NSSM, Serilog, PowerShell, Bash,
pre-commit.

---

### 4.4 — Sigma — BFSI Loan Recovery / Field-Operations Platform
**Role:** Technical Lead & Core Backend Developer
**Timeline:** Jul 2025 – Jan 2026
**Hook metric:** Real-time field-officer tracking and OSRM route optimization with WebSocket
live-location streaming, plus a rule-based auto-assignment engine over a geographic hierarchy.

**What it is:** A field-agent loan-recovery platform serving managers and field officers
across a region → zone → city → pincode → area hierarchy — case management, allocation/referral
file ETL, route planning, attendance, dashboards, and live video calling. Django modular
monolith deployed on E2E Compute Nodes + EOS.

**What I built / architected:**
- **Architected and deployed the full backend stack** on **E2E Compute Nodes + EOS object
  storage** for the field-officer loan-recovery domain.
- Built **real-time field-officer tracking and route optimization** using the **OSRM** routing
  engine (route geometry + driving distance) with **WebSocket live-location streaming** (JWT-
  authenticated per-FO channel groups) and route reconstruction from recorded GPS points,
  batched to manage API load.
- Built a **rule-based auto-assignment engine** enforcing **territory (pincode/area), product
  assignment, capacity (max cases/day), proximity, inactive-FO exclusion, and performance**
  constraints — a real policy engine, not a random picker.
- Integrated **Google Maps** geocoding + Address Validation, **Mappls (MapMyIndia) eLoc**
  (OAuth token caching in DB, entity/coordinate lookup), and **OSRM/OpenStreetMap** routing;
  case-address coordinate enrichment pipelines with API-limit handling.
- Integrated **LiveKit ASGI** video calling with room tokens, webhooks, and egress recording
  to Azure Blob.
- Delivered **bulk allocation/referral file ETL**: template-driven validation, header checks,
  loan-account reconciliation, incremental bulk writes, error-file generation, and **WebSocket
  progress reporting** — a multi-step pipeline with incremental persistence.
- Implemented **ORM-aggregated web and mobile dashboards** for collections, disposition, and
  field-officer productivity analytics — `Sum`/`Max`/`Coalesce`/`TruncDate`,
  `OuterRef`/`Subquery`, `.iterator()` streaming querysets, manager-hierarchy scoping, and
  zero-filled time series.
- Built a **custom auth stack**: custom user model (`login_id` username field), token
  blacklist, multi-login protection, OTP/password-reset, login analytics, a separate
  permission model, and geography/reports-to queryset scoping.
- Built attendance/leave, route planning (priority queue by billing expiry / revisit / risk /
  collectable amount), disposition-driven case updates (PTP/RTP/paid/NRPC/etc.), rewards and
  milestones.

**skills:** Python, Django 5.2, Django REST Framework, modular monolith, solution
architecture, E2E Compute Nodes, EOS object storage, OSRM / OpenStreetMap routing, route
geometry & distance, route optimization, priority-queue sequencing, WebSocket live-location
streaming, Django Channels, JWT-authenticated websockets, rule-based auto-assignment engine,
territory/capacity/proximity constraints, Google Maps Geocoding, Google Address Validation,
Mappls / MapMyIndia, eLoc, OAuth token caching, coordinate enrichment, LiveKit ASGI video,
egress recording, Azure Blob, MinIO/E2E storage, bulk file ETL, pandas, openpyxl,
template-driven validation, incremental bulk_update, error-file generation, websocket progress
reporting, ORM aggregation (Sum/Max/Coalesce/TruncDate), Subquery/OuterRef, streaming querysets
(.iterator), manager-hierarchy scoping, zero-filled time series, custom user model, token
blacklist, OTP, password reset, login analytics, permission scoping by geography, generic
handler/serializer/view framework, Redis, Celery, Docker Compose, DBML schema docs, pre-commit,
Swagger/drf-yasg.

---

### 4.5 — TruOperate — Factory / Industrial Operations Management System
**Role:** Backend Technical Lead & Core Backend Developer
**Timeline:** Feb 2024 – Jul 2025
**Hook metric:** **Zero-downtime modernization of a 25-year-old Java platform to Python/Django**
while preserving the live production **Microsoft SQL Server** schema — 100M+ row datasets.

**What it is:** An enterprise operations platform for industrial plant and field workflows —
accounts, security groups, equipment/tags, operator rounds, log books, instructions, alerts,
dashboards, reporting, and mobile/field service APIs — split into TruPlant, TruOperate,
TruEngine and TruTech domains. A legacy Java→Django redevelopment on the same MSSQL estate.

**What I built / led:**
- **Led zero-downtime modernization of a ~25-year-old Java platform** to Python/Django while
  preserving the production **Microsoft SQL Server** schema through **manual field-by-field
  model mapping** and **SQL-based migration bridges** (where standard Django migrations were
  insufficient), keeping compatibility with stored procedures, triggers, and a deeply nested
  enterprise data model.
- Designed **optimized complex SQL queries over 100M+ row MSSQL datasets** (equipment readings,
  shift logs, violations).
- Built **security-group role-based access control** and operator-round, log-book, equipment,
  instruction and alert APIs, using reusable **generic DRF abstractions** and
  **metadata-driven permission automation** — scanning code (URLs / views / serializers) to
  resolve and generate permission metadata automatically.
- Integrated **JasperReports through a Java-to-Python bridge** (`pyreportjasper`) for enterprise
  report generation — templates, subreports, PDF/HTML artifacts — plus **Excel export**
  workflows with user-specific table preferences.
- Built **CRON/Celery + Celery Beat** automation for shift reminders, round distribution, and
  real-time violation alerts over the legacy MSSQL estate (Redis broker/result backend).
- Delivered **TruTech service APIs** for mobile/field synchronization — round uploads, route
  sessions, device logs, GPS coordinates and image transfer — plus device authorization,
  app/version tracking, and access logging.
- Added audit-oriented data models and history tracking for operational accountability.

**skills:** Python, Django, Django REST Framework, legacy modernization, zero-downtime
migration, Java-to-Python migration, Microsoft SQL Server (MSSQL), pyodbc, manual schema
mapping, SQL-based migration bridges, stored procedures, triggers, 100M+ row query
optimization, complex SQL tuning, security-group RBAC, custom user model, generic DRF
abstractions, metadata-driven permission automation, code scanning (URLs/views/serializers),
JasperReports, pyreportjasper, Java-to-Python reporting bridge, PDF/HTML report generation,
Excel export, pandas, openpyxl, xlsxwriter, table-preference mapping, Celery, Celery Beat,
Redis, CRON automation, mobile/service sync APIs, device authorization & version tracking, GPS
& image transfer, audit trails, base64/binary media handling, django-filter, CORS, GitLab CI.

---

### 4.6 — PsyHire v1 — ATS / HR Tech Platform
**Role:** Core Backend Developer
**Timeline:** Aug 2023 – Feb 2024
**Hook metric:** Scalable DRF APIs with async Celery + Redis for bulk email, reminders, and ML
model inference; relational + NoSQL together; Azure→E2E/GCP media migration.

**What it is:** An applicant-tracking / HR-tech platform — the first-generation product behind
the later PsyHire v2 rebuild.

**What I built:**
- Designed **scalable DRF REST APIs** with asynchronous **Celery + Redis** processing for bulk
  email campaigns, reminder triggers, and **ML model inference**.
- Worked with **relational (PostgreSQL) and NoSQL (MongoDB)** stores together in one system.
- Managed **media-storage migration from Azure Blob to E2E/GCP** object storage.

**skills:** Python, Django, Django REST Framework, REST API design, Celery, Redis, asynchronous
task processing, bulk email campaigns, reminder scheduling, ML model inference integration,
PostgreSQL, MongoDB, polyglot persistence (relational + NoSQL), Azure Blob Storage, GCP object
storage, cloud storage migration.

---

## 5. Personal & Open-Source Projects

Five projects — two of them **published PyPI packages** with CI and documentation sites.

---

### 5.1 — django-data-seed — Open-Source PyPI Package
**Type:** Published PyPI package · MIT
**Links:** [PyPI](https://pypi.org/project/django-data-seed) · [GitHub](https://github.com/rohith-baggam/django-data-seed) · [Docs](https://rohith-baggam.github.io/django-data-seed/)
**Hook metric:** Fill an entire Django database with realistic, valid, reproducible test data
in **one command, zero config** — hundreds of thousands of rows in seconds.

**What it is:** A zero-config Django test-data generator. Point it at a project and it reads
the models, works out the correct fill order, generates believable values that respect every
field's rules, and bulk-inserts the lot.

**What it does / how it's built:**
- **Zero-config generation** — inspects models and fills every field type with a valid value;
  no factories, no fixtures.
- **Realistic values** — a `city` column gets a real city, `email` a real address, `price` a
  believable amount (via `mimesis`, locale-aware).
- **Relationship-aware & coherent** — seeds parents before children, **reuses** existing rows
  instead of creating a fresh parent per child, keeps a row internally consistent
  (`created ≤ updated ≤ shipped`, one identity behind name/email).
- **Reliable uniqueness** — respects `unique` and `unique_together` / `UniqueConstraint`
  without duplicate errors.
- **Pre-flight safety** — refuses to run against a stale schema; flags unapplied migrations or
  model drift before writing a row.
- **Bulk speed** — batched `bulk_create`; a hundred thousand rows in seconds.
- **Reproducible** — `--seed 42` produces identical data every run (for teammates, CI, bug
  reports).
- **CLI *and* Python API** — `python manage.py seeddata` or `from django_data_seed import seed`.
- **CI-verified across Python 3.10–3.14 × Django 4.0–6.0** on SQLite, PostgreSQL and MySQL via
  GitHub Actions.

**skills:** Python, Django, PyPI packaging, open-source maintenance, CLI tooling, Django
management commands, model introspection / metaprogramming, topological ordering of model
dependencies, foreign-key-aware seeding, coherence modeling, unique / unique_together constraint
handling, bulk_create batching, reproducible seeding (PRNG seeding), pre-flight schema
validation, mimesis, rich (terminal UI), optional-extras packaging, GitHub Actions CI matrix
(Python 3.10–3.14 × Django 4.0–6.0), multi-database support (SQLite/PostgreSQL/MySQL/SQL
Server), MkDocs Material documentation, semantic versioning, MIT licensing.

---

### 5.2 — django-perfy — Open-Source PyPI Package
**Type:** Published PyPI package · MIT
**Links:** [PyPI](https://pypi.org/project/django-perfy) · [GitHub](https://github.com/rohith-baggam/django-perfy) · [Docs](https://rohith-baggam.github.io/django-perfy/)
**Hook metric:** API + WebSocket + server performance monitoring for Django, with a built-in
dashboard, downloadable/emailable **PDF reports**, and optional telemetry-in-a-separate-DB.

**What it is:** A drop-in performance-monitoring app for Django — middleware + Channels mixin +
Celery snapshots + dashboard + PDF reports.

**What it does / how it's built:**
- **API monitoring** — middleware records endpoint, method, status, response time, DB query
  count and DB time, with configurable **sampling** and always-capture of slow requests.
- **WebSocket monitoring** — a Channels consumer mixin records connect/disconnect/send/receive
  events and timings.
- **Resource snapshots** — periodic CPU/memory/Redis/Postgres metrics via Celery tasks and a
  self-contained background timer (works even without Celery Beat).
- **Rollups** — minute/hour `PerformanceSummary` aggregates for fast dashboards.
- **Dashboard** — staff-only Jinja2 dashboard (overview, API, WebSocket, resources, DB
  queries, correlation, raw logs).
- **PDF reports** — latency, throughput, bottleneck and resource-utilization reports;
  previewable in-browser, downloadable, or emailable (WeasyPrint).
- **Secondary database** — route all telemetry to a separate DB alias via one setting; a
  bundled **DB router** handles reads, writes and migrations.
- **PII-safe by default** — header/body capture off by default with configurable redaction of
  headers and body fields; hashed user IDs.

**skills:** Python, Django, PyPI packaging, open-source maintenance, Django middleware,
Django Channels consumer mixins, WebSocket instrumentation, Celery, Celery Beat, background
timer scheduling, Redis, psutil, resource telemetry (CPU/memory/Redis/Postgres), performance
aggregation / rollups (p50/p95/p99, throughput), DB query counting, database router (secondary
DB), Jinja2 dashboards, WeasyPrint PDF generation, emailable reports, configurable sampling,
PII redaction, hashed user IDs, Django system-check framework, optional-extras packaging,
MkDocs Material documentation, MIT licensing.

---

### 5.3 — Rex — Real-Time Cross-Database Migration Framework
**Type:** Two-part open-source framework (sender + receiver) · MIT
**Links:** [Receiver GitHub](https://github.com/rohith-baggam/Rex-data-sync-receiver) · [Sender GitHub](https://github.com/rohith-baggam/Rex-data-sync-sender)
**Hook metric:** Encrypted, real-time Django-to-Django data sync across PostgreSQL / MySQL /
SQLite — with public/private-key security and transaction-scoped rollback.

**What it is:** A sender/receiver framework for live data synchronization between Django
projects across servers and database engines, with schema validation and real-time progress
over WebSockets.

**What it does / how it's built:**
- **Cross-database migration** — transfers data between Django projects even across different
  engines (e.g. PostgreSQL → MySQL), transforming into Django-native fields.
- **Public/private-key security with token exchange** — sender and receiver exchange a
  pre-shared token and validate matching `SECRET_KEY` before any transfer; only authorized
  projects can connect.
- **Schema validation** — the receiver validates the incoming schema (every model and field)
  against its own models before syncing, preventing inconsistencies.
- **Incremental encrypted transfer** — data is dumped to JSON, each instance encrypted, and
  streamed incrementally with live progress.
- **Transaction-scoped rollback** — the whole migration runs inside a transaction, rolling
  back on any error or connection loss.
- **Real-time progress** — WebSocket feedback from connection → schema verification → data
  sync completion, surfaced in a web interface.

**skills:** Python, Django, Django Channels, Daphne, WebSockets, real-time communication,
channels-redis, Redis, cryptography (public/private-key encryption), token-based authentication,
end-to-end encrypted data transfer, cross-database migration (PostgreSQL/MySQL/SQLite), schema
validation, incremental streaming transfer, transaction management / rollback, Django REST
Framework, django-data-seed (used for demo data), progress broadcasting, MIT licensing.

---

### 5.4 — Quarkus Blog Backend — Learning Project
**Type:** Personal learning project (Java / Quarkus)
**Links:** [GitHub](https://github.com/rohith-baggam/quarkus-blog-app)
**Hook metric:** A full Java 21 / Quarkus 3 REST backend built to map Django/DRF concepts onto
the JVM — RS256 JWT, Panache ORM, Flyway, layered architecture.

**What it is:** A backend-only blog application built to learn Quarkus deeply, as a hands-on
JVM counterpart to a Django/DRF backend.

**What it does / how it's built:**
- REST backend exploring the **full Quarkus feature set**: user registration/login, blog CRUD,
  pagination (limit/offset), dynamic filtering/search, and image relationships.
- **RS256 JWT auth** (asymmetric signing) with a **custom request filter** that loads the
  current user from the token; CDI scopes (`@ApplicationScoped`, `@RequestScoped`).
- **Hibernate ORM with Panache** repository patterns and entity relationships.
- **Flyway** migrations for schema evolution.
- Clean layered **Resource / Service / Repository** architecture with DTOs, a standardized API
  response wrapper, global exception mapping, Hibernate Validator, and OpenAPI/Swagger docs.
- MySQL for production, SQLite for local dev; Spotless for formatting.

**skills:** Java 21, Quarkus 3, Hibernate ORM, Panache, Jakarta EE, CDI (@ApplicationScoped /
@RequestScoped), SmallRye JWT, RS256 asymmetric JWT, custom JAX-RS request filters, REST API
design, Jackson, Flyway migrations, Hibernate Validator, layered architecture
(Resource/Service/Repository), DTO design, standardized response wrappers, global exception
mapping, pagination & filtering, OpenAPI/Swagger, MySQL, SQLite, Spotless, Django→Quarkus
concept mapping.

---

### 5.5 — Personal Learning Blog App (Quarkus Basics)
*Note: this is the same learning codebase described in 5.4, documented separately in the source
notes as the "Quarkus basics" overview. Treat 5.4 and 5.5 as one project in the portfolio —
list it once. Retained here only so no source material is lost.*

---

## 6. Education, Certifications & Soft Skills

### Education
- **B.E. Computer Science Engineering** — Chandigarh University, Punjab · *Jul 2019 – 2023* ·
  CGPA 7.0/10
- **Intermediate (MPC)** — Narayana Junior College, Visakhapatnam

### Certifications
- Google Cloud Platform — Production Deployment
- Neo4j Graph Data Science & Cypher
- Code Quality — SonarQube, pre-commit, PEP-8

### Professional / Soft Skills
- Team Collaboration & Leadership
- Product Management Mindset
- On-time Delivery & Ownership
- Knowledge Seeker

### Personal Traits
- Building Personal Projects
- Quick Learner of New Tech Stacks
- Passionate about Optimization Techniques

---

## 7. Design Direction (for the portfolio designer)

### Suggested information architecture
1. **Hero** — name, headline ("Technical Lead · Backend Engineer · Solution Architect"), the
   one-line pitch from §2, and primary links (GitHub, LinkedIn, PyPI, email). Consider three
   headline stat chips: **3 yrs experience · 6 production platforms · 2 published PyPI packages**.
2. **About** — the professional summary from §2 plus the "three things that define the work."
3. **Skills** — render §3 as grouped chip clusters, not a wall of text.
4. **Experience / Case studies** — the six professional projects (§4). Each as an expandable
   card: title, role, timeline, hook metric, 3–5 bullets, and a skills chip row.
5. **Open source & personal** — the five personal projects (§5), with badges for the two PyPI
   packages (PyPI version, CI, license) and prominent links.
6. **Education & certifications** (§6), compact footer-adjacent.
7. **Contact** — email + socials.

### Ordering & emphasis
- Lead professional experience **newest-first** (PsyHire v2 → Ping → AIN → Sigma → TruOperate
  → PsyHire v1). The three strongest signal projects are **PsyHire v2** (multi-tenant
  architecture), **Ping** (encrypted real-time), and **TruOperate** (legacy migration + 100M
  rows) — give them the most visual weight.
- For open source, lead with **django-data-seed** and **django-perfy** (published, CI-backed);
  Rex and Quarkus are strong supporting projects.

### Tone
- Senior, precise, evidence-driven. Prefer concrete nouns (per-tenant PostgreSQL, AES-GCM,
  OSRM, 100M-row MSSQL) over adjectives. Every claim here is backed by real implementation.

### Per-project hook metrics (pull-quotes / card headlines)
- **PsyHire v2:** "Three Django microservices · a database *and* a Redis DB per tenant ·
  resolved at runtime."
- **Ping:** "Self-hosted Slack + Zoom in one — AES-GCM messaging with blind-index search over
  encrypted data."
- **AIN:** "Device discovery across SNMP, WMI, SSH & ICMP → one inventory schema; time-series
  analytics over NetFlow/IPFIX/syslog/Windows events."
- **Sigma:** "Real-time field-officer tracking + OSRM route optimization + a rule-based
  auto-assignment engine."
- **TruOperate:** "Zero-downtime migration of a 25-year-old Java platform to Django — live
  MSSQL, 100M+ rows."
- **PsyHire v1:** "Scalable DRF APIs with async Celery/Redis and ML inference; relational +
  NoSQL together."
- **django-data-seed:** "Fill an entire Django DB with realistic, reproducible data in one
  command — zero config."
- **django-perfy:** "API + WebSocket + server performance monitoring for Django, with PDF
  reports."
- **Rex:** "Encrypted, real-time Django-to-Django data sync across PostgreSQL / MySQL / SQLite."
- **Quarkus Blog:** "A Java 21 / Quarkus REST backend — RS256 JWT, Panache, Flyway, layered
  architecture."

### Color / theme suggestion (optional)
- A dark, technical palette (deep slate/navy background, one accent — teal or electric blue)
  suits the backend/infra positioning. Keep it accessible in both light and dark. Use monospace
  for tech chips and code-like accents.

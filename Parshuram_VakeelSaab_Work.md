# My Work at VakeelSaab (Dec 2025 – Nov 2026)

**Name:** Parshuram Kumar
**Role:** Backend / full-stack developer
**Period covered:** 1 Dec 2025 to 22 Nov 2026
**Source:** my git history in 6 repos (about 600 non-merge commits) plus the code itself.

| Repo | What it is | My commits | Active |
|---|---|---|---|
| `lawyer-appointment` | Consultations, scheduling, cases, and the whole call center (Express 5, Sequelize, MySQL, Socket.IO) | 185 | Dec 2025 – Nov 2026 |
| `admin-panel` | Internal dashboard for agents, managers, admins (Next.js 15, React 19) | 192 | Mar 2026 – Nov 2026 |
| `user-service` | Users, auth, lawyer profiles, KYC, pricing, admin APIs (NestJS 10, TypeORM, MySQL) | 134 | Dec 2025 – Nov 2026 |
| `vs-gateway` | API gateway (Fastify 5, Redis) | 72 | Dec 2025 – Nov 2026 |
| `comm-service` | Push, SMS, email, WhatsApp (Express 4, Sequelize) | 9 | Feb 2026 – Nov 2026 |
| `doc-service` | PDF and calendar file generation (Python, FastAPI, WeasyPrint) | 9 | Jan 2026 – Nov 2026 |

## How to use this file

- **Part 0** is ready to copy into a resume.
- **Parts 1 to 6** are study material. Each feature has: why it was needed, what I built, how it works step by step, key files, packages, tricky parts, and interview questions with answers.
- **Part 7** is the list of all packages and tools.
- **Part 8** is the month-by-month timeline. **Part 9** is interview preparation: stories and topics to revise.
- Where something says **"not confirmed"**, do not claim it in an interview without checking first.
- No secrets, phone numbers, customer data or internal URLs are in this file. Only env variable *names* appear.

## Contents

- Part 0: Resume section
- Part 1: System architecture overview
- Part 2: Exotel telephony integration (deep dive)
- Part 3: Appointment / call-center service (`lawyer-appointment`), including Bonvoice
- Part 4: User service, communication service, document service
- Part 5: Admin panel (frontend)
- Part 6: API gateway (`vs-gateway`)
- Part 7: Tech stack and packages (all repos)
- Part 8: Month-by-month timeline
- Part 9: Interview preparation (stories, topics to revise, things to be careful about)

---

# Part 0: Resume Section

## Profile summary (pick one)

**Short:**
Backend developer with 1 year of experience building microservices in Node.js and TypeScript (NestJS, Express, Fastify) with MySQL and Redis. Built a production call-center system on top of Exotel cloud telephony, including inbound call routing, webhooks, real-time agent dashboards and call recording storage. Also worked on the React / Next.js admin panel and a Python FastAPI document service.

**Longer:**
Software developer at VakeelSaab, a legal-consultation platform that connects clients with lawyers. I work across 6 repositories: an API gateway, 4 backend services and an admin dashboard. My main work is the call center: telephony integration (Exotel, and a second provider, Bonvoice), fair and race-free agent assignment, call status tracking from webhooks, dispositions and follow-up tasks, and reports and exports for managers. I also built lawyer onboarding, KYC and verification APIs, admin audit logging, and PDF and calendar generation.

## Tech stack line

**Languages:** TypeScript, JavaScript, Python, SQL
**Backend:** Node.js, NestJS 10, Express 5, Fastify 5, FastAPI
**Frontend:** Next.js 15 (App Router), React 19, Tailwind CSS, Radix UI / shadcn, SWR, TanStack Table, Zustand, react-hook-form, zod
**Data:** MySQL, TypeORM, Sequelize, SQLAlchemy, Redis (ioredis)
**Real-time and jobs:** Socket.IO, node-cron, Jenkins-triggered cron routes
**Integrations:** Exotel (telephony, WebRTC SDK), Bonvoice (telephony), AWS S3 (presigned URLs), Firebase push notifications, Slack alerts, WhatsApp messaging
**Docs and tools:** Swagger / OpenAPI, Joi, class-validator, Git, Bitbucket Pipelines, Docker, Postman, New Relic logs

## Resume bullet points

Pick 6 to 8 that fit the job you apply for.

**Telephony and call center**
- Built the Exotel cloud-telephony integration for a call center: outbound click-to-call, inbound call routing, status webhooks, call logs, dual-channel recording and browser (WebRTC) calling.
- Designed race-free inbound routing: a Redis lock per call id, an atomic SQL "claim" (`UPDATE ... WHERE status='available'`) so two calls can never get the same agent, and sticky plus least-assigned agent ranking for fair load.
- Made webhook handling idempotent with a unique call id index and a duplicate-insert fallback, and wrote one status resolver that maps all provider events to internal call statuses.
- Built a stale-call reconcile job that checks calls stuck in "ringing" or "answered" against the provider's Call Details API and fixes them, with the rule "never guess, only write what the provider confirms".
- Moved call recordings from provider URLs to AWS S3 in the background, with presigned playback URLs, a bulk migration endpoint with dry run, and a marker for recordings that no longer exist.
- Integrated a second telephony provider (Bonvoice) behind a provider switch stored in the database, with per-agent provider choice, so the call center can change provider without a deploy. *(Tested on dev; say so if asked.)*
- Built DTMF priority routing, an inbound blocked-numbers list, AI-call intake with atomic claim over sockets, and automatic dispositions for missed, short and unanswered calls.

**Backend APIs and data**
- Rewrote call-log search in raw SQL with batch lookups to remove N+1 queries, and added a separate stats API so the dashboard no longer computes stats over 1000 rows in the browser.
- Built Work Queue V2 for agents: backend-driven tabs, priority, stats and follow-up tasks, with one disposition per call enforced by the API (409 on duplicates).
- Built lawyer onboarding and profile APIs in NestJS: career, education, case highlights, KYC, courts (atomic add-court inside a transaction), pricing and consultation preferences.
- Built an admin audit log using a custom decorator and interceptor that records who changed what, and an "auto un-verify" rule that removes a lawyer's verified status when key profile or KYC fields change.
- Added about 20 admin list filters (city, state, court, expertise, onboarding stage, verification), search relevance sorting, and Fisher-Yates random ordering of lawyers behind a cached switch.
- Built update email / phone for users with session revoke and sync to the finance service, and user status flags for marking non-genuine users.
- Split a 2,400-line gateway route file into one config file per controller (27 files) and added gateway routes, auth and validation for each new feature.
- Wrote an auto-generated Swagger / OpenAPI setup from the Express router tree, and Swagger docs for the NestJS user service.

**Notifications and documents**
- Changed call push notifications to go only to the user's latest device, and built device de-register and latest-device APIs.
- Built a calendar invite (`.ics`) API and an anonymous lawyer profile PDF in Python (FastAPI, Jinja2, WeasyPrint), with fingerprint-based reuse so an unchanged PDF is not generated twice.
- Set up Slack alerts for critical events and a daily cron that resets agent assignment counts.

**Frontend (admin panel)**
- Built call-center screens in Next.js 15 and React 19: manager call logs with filters and a detail modal, agent dashboard, work queue with clustering, infinite scroll and a hand-built calendar view.
- Built a CSV export that fetches all filtered rows in pages of 2000, three pages at a time, with progress, cancel and an option to mask phone numbers.
- Added permission-based UI (`resource:action` permissions from the JWT) so buttons and private details show only to allowed roles, with the API enforcing the same rule.
- Built about 20 Next.js proxy routes (backend-for-frontend) so the browser never calls the gateway directly.

## One-minute "tell me about your work" answer

"I work at VakeelSaab, a platform where clients consult lawyers. The backend is microservices behind an API gateway. I work on most of them, but my main area is the call center. I integrated Exotel, which is a cloud telephony provider. When a client calls our number, Exotel asks our backend which agent to ring. My code picks an agent fairly, locks that agent with one atomic SQL update so two calls cannot take the same person, and tells the agent's dashboard over a socket. Then Exotel sends webhooks as the call moves, and I map those to our own call statuses, save the recording to S3, and create a disposition if the call was missed. A hard part was calls getting stuck when the last webhook never arrived, so I built a reconcile job that asks Exotel's API for the truth. Besides that I built lawyer onboarding and KYC APIs in NestJS, an audit log, the admin panel screens in Next.js, and a small Python service for PDFs and calendar files."

---

# Part 1: System Architecture Overview

### 1. One-paragraph summary

VakeelSaab is a legal-consultation platform. Clients talk to lawyers by call, chat and meetings. The backend is a set of small services. All client traffic (mobile apps, admin panel) enters through one **API gateway**. The gateway checks the JWT, checks the request shape, and forwards the request to the right service. Each service owns its own MySQL database. Redis is used for cache, locks and pub/sub. Real-time features use Socket.IO.

### 2. The services

| Repo | What it is responsible for | Language / framework | Runtime | Database / ORM |
|---|---|---|---|---|
| `vs-gateway` | Single entry point. Routing, JWT check, header and query filtering, retries, proxy to services. No business logic. | TypeScript, Fastify 5 | Node 22 (Dockerfile, `engines`) | No SQL database. Redis (ioredis) only. |
| `user-service` | Users, login/OTP, JWT issue and refresh, roles and permissions, lawyer profiles, KYC, packages, pricing, call quota, addresses, careers/education/case highlights, court-city data, admin APIs, audit logs, invoices, translations. | TypeScript, NestJS 10 | Node 24 (Dockerfile) | MySQL with TypeORM (`synchronize: false`, migrations) |
| `lawyer-appointment` | Consultations, slots and availability, slot confirmations, chats, cases, documents, feedback, documentation-service tickets, recent activity. Also the whole **call center**: agents, call logs, dispositions, follow-up tasks, telephony webhooks, AI calls, transcription, meetings, reports. | TypeScript, Express 5 | Node 22 (Dockerfile) | MySQL with Sequelize 6 (sequelize-cli migrations) |
| `comm-service` | Sending notifications: push, SMS, email, WhatsApp. Device token registration. Notification logs and templates. WhatsApp retry job. Delivery webhooks. | JavaScript (CommonJS), Express 4 | Node 22 (Dockerfile) | MySQL with Sequelize 6 |
| `doc-service` | Document generation: HTML templates to PDF (invoices, lawyer invoice, lawyer profile PDF, supporting docs), PDF merge, calendar (`.ics`) files. Uploads results to S3. | Python, FastAPI | Python 3.11 (Dockerfile) | MySQL with SQLAlchemy 2 (async), Alembic listed in requirements |
| `admin-panel` | Internal dashboard for admins and call-center agents. Lawyers, clients, KYC, call center screens, live ops, reports. | TypeScript, Next.js 15 (App Router), React 19 | Node 22 (Dockerfile) | No own SQL DB. `mongodb` driver is in dependencies (usage not confirmed). |

A seventh service, **finance-service** (wallets, invoices, withdrawals, reconciliation), is registered in the gateway under `/g/fs`. Its repo is not in my folder, so its internals are not confirmed.

Important detail: the gateway has two entries that point at the same codebase. `lawyer-appointment-service` (`/g/la`) and `call-center-service` (`/g/cc`) are both the `lawyer-appointment` repo. The repo has two Dockerfiles (`Dockerfile` and `Dockerfile.cc`) so it can run as two deployments.

### 3. Architecture diagram

```
        Mobile apps (client app, lawyer app)          Admin panel (Next.js, browser)
                     |                                   |                 |
                     | HTTPS                             | /api/proxy/*    | Socket.IO
                     |                                   | (Next server)   | (direct to LA,
                     v                                   v                 |  NEXT_PUBLIC_SOCKET_URL)
        +---------------------------------------------------------+        |
        |                 vs-gateway  (Fastify)                   |        |
        |  CORS + Helmet -> request id -> logging -> JWT 'auth'   |        |
        |  -> header/query/body checks -> Axios proxy + retries   |        |
        |  Redis: list of deactivated users (pub/sub + 60s sync)  |        |
        +----+-----------+-----------+------------+----------+----+        |
             |           |           |            |          |             |
          /g/us       /g/la       /g/cc        /g/cs      /g/fs            |
             v           v           v            v          v             |
      +-----------+  +--------------------+  +----------+  +---------+     |
      |  user-    |  | lawyer-appointment |  |  comm-   |  | finance |     |
      |  service  |  | (REST + call       |<-+ service  |  | service |     |
      |  NestJS   |  |  center) Express   |  | Express  |  | (not in |     |
      |  TypeORM  |  |  Sequelize         |  | Sequelize|  |  folder)|     |
      +-----+-----+  |  Socket.IO server  |<-+----------+  +---------+     |
            |        |  /consult /chat    |                                |
            |        |  /call-center  <------------------------------------+
            |        +---------+----------+
            |                  |
            |   service-to-service HTTP (shared tokens / internal secret)
            +---------+--------+--------------------+
                      v                             v
               +-------------+               +--------------+
               | doc-service |               |  MySQL (one  |
               | FastAPI +   |               |  DB per      |
               | Celery +    |               |  service)    |
               | WeasyPrint  |               +--------------+
               +------+------+               +--------------+
                      |                      |    Redis     |
                      v                      +--------------+
                   AWS S3

  External: Exotel / Bonvoice (telephony), LiveKit (audio/video rooms), Firebase FCM + Apple APNs (push),
            SMS provider, Gmail API / SMTP (email), Interakt (WhatsApp), Slack (alerts),
            Gemini / OpenAI / Anthropic SDKs (AI), New Relic (monitoring), OpenReplay (admin-panel sessions)
```

### 4. How the gateway forwards requests

The gateway is **config driven**. There are no hand-written controllers. A route is a plain object in a config file. At startup the gateway reads the config and registers one Fastify route per object.

- **Service registry**: `config/services.ts` lists the services (name, enabled, config file, priority).
- **Path prefixes** (the `base_route` of each service):

| Prefix | Service | Upstream URL env name |
|---|---|---|
| `/g/us` | user-service | `USER_SERVICE_URL` |
| `/g/la` | lawyer-appointment-service | `LAWYER_APPOINTMENT_SERVICE_URL` |
| `/g/cc` | call-center-service (same repo as LA) | `CALL_CENTER_SERVICE_URL` |
| `/g/cs` | comm-service | `COMM_SERVICE_URL` |
| `/g/fs` | finance-service | `FINANCE_SERVICE_URL` |

- **Path mapping**: incoming URL = `base_route + route.path`. Upstream URL = `base_url + route.path`. So `/g/us/api/v1/admin/lawyers` goes to `<user-service>/api/v1/admin/lawyers`. Path params like `:id` are replaced by the real values.
- **Size**: about 470 route objects in total on this branch (user-service about 198, lawyer-appointment about 111, call-center about 98 + 13 meeting routes, finance about 44, comm about 6).
- **Proxying**: `src/core/proxy.ts` builds the upstream URL, copies headers, and calls the service with Axios (`src/utils/httpClient.ts`). Keep-alive HTTP agents are used.
- **Auth / JWT**: the `auth` middleware calls `request.jwtVerify()` from `@fastify/jwt`. Algorithm HS256, with issuer and audience checks (`JWT_SECRET`, `JWT_ISSUER`, `JWT_AUDIENCE`). The gateway does not create tokens. user-service creates them. Both use the same secret.
- **Deactivated users**: after the JWT check, the gateway looks up the user id in an in-memory set of deactivated users. user-service writes this list to a Redis sorted set and publishes changes on a Redis channel. The gateway subscribes to the channel and also re-syncs every 60 seconds (`src/utils/inactive-users.ts`). So an admin can block a user even while their JWT is still valid.
- **Permissions**: the gateway only checks "is the token valid". It has a role helper (`admin-only`) but fine-grained permissions (for example `lawyer:read`) are checked **upstream** in the services (NestJS `RolesPermissionsGuard` with `@Permissions(...)` in user-service; `requirePermission(...)` in lawyer-appointment). Roles and permissions travel inside the JWT payload.
- **Validation** (per route, all optional):
  - `headers.required`: reject if a header is missing.
  - `headers.filter.allow`: only these headers are forwarded. Everything else is dropped.
  - `query_params.allowed`: only these query params are kept. Others are silently removed. Simple type rules (`integer`, `number`, `string`, `boolean`, min/max, pattern) and defaults can be set.
  - `path_params`: type and pattern rules.
  - `body.content_types`: reject wrong `Content-Type`. `body.required`: reject empty body. Body schema and size checks are still TODO in the code.
- **Rate limiting**: a middleware named `rate-limit` exists and many routes list it, but in the current code it only logs (placeholder with a TODO). The `@fastify/rate-limit` package is installed but not registered. Real throttling is done in user-service with `@nestjs/throttler` and in lawyer-appointment with `express-rate-limit`.
- **Retries and timeouts**: each route has a `timeout` and optional `retries`. In this code `retries` means **total attempts**. 4xx answers are never retried. Other failures retry with exponential backoff (1s, 2s, 4s...). Upstream 4xx bodies pass through to the client as they are. Upstream 5xx becomes 502.
- **Language header**: the gateway adds `x-user-language` for services (from the client header, else from the JWT `preferredLanguage`, else `en`).
- **Raw-body routes**: a route with `body.raw: true` keeps the exact bytes. This is needed for webhooks that sign the body (LiveKit, WhatsApp provider).
- **Internal header**: every service config removes `X-Internal-Token` from incoming requests, so outside clients cannot fake an internal header.
- **Present but not wired**: a circuit breaker class (`src/core/circuitBreaker.ts`) and upstream health checking exist, but the proxy does not call the breaker and the health check body is a TODO. Swagger packages are installed but not registered.
- **Gateway own endpoints**: `/health`, `/ready`, `/status`, `/redis`, `/health/full`.
- **Hot reload**: `chokidar` watches the `config` folder in dev. Production runs compiled code from `dist`, so a config change needs a rebuild and redeploy.

### 5. How services call each other

Services talk over plain HTTP. There is no message queue between services.

- `lawyer-appointment` has client classes in `src/core/internalServices` for user-service, comm-service, document service and finance-service. Base URLs and tokens come from env (`USER_SERVICE_URL`, `COMM_SERVICE_URL`, `DOCUMENT_SERVICE_URL`, `FINANCE_SERVICE_URL`, and `*_ACCESS_TOKEN` names).
- `user-service` has `src/internalService` with clients for doc-service, finance-service, lawyer-appointment and OTP.
- **Internal secrets**:
  - user-service protects internal routes (for example `/cron/...`) with `InternalApiKeyGuard`, which checks the `x-api-key` header against `INTERNAL_API_SECRET`.
  - lawyer-appointment protects internal routes with `requireInternalSecret`, which checks the `X-Internal-Secret` header against `INTERNAL_API_SECRET`. A helper `internalSecretOr(jwtGuard)` allows a route to be used by both a human (JWT) and a scheduler (secret).
  - comm-service uses one shared static token in the `Authorization` header (`src/middlewares/auth.js`).

### 6. Cache, queue and messaging

| Thing | Where | Used for |
|---|---|---|
| Redis (ioredis) | gateway | Deactivated-user list (sorted set + pub/sub) |
| Redis (ioredis / redis) | user-service | Cache, OTP, the deactivated-user list writer |
| Redis (ioredis) | lawyer-appointment | Reader/writer clients, locks for cron jobs, alert de-duplication, call-center state |
| Celery + Redis | doc-service | Background document generation (queue `document_generation`), Flower for monitoring |
| Node event emitters | user-service (`@nestjs/event-emitter`), comm-service (`eventBus.js`) | In-process events only |

No Kafka or RabbitMQ was found.

### 7. Real-time (sockets)

`lawyer-appointment` runs a Socket.IO server (`src/socket.ts`). It can share the REST port or use its own (`SOCKET_PORT`). Three namespaces:

- `/consult` for consultation and call flow between client and lawyer.
- `/chat` for chat messages.
- `/call-center` for agent dashboards (incoming call, agent status, dispositions).

A "kill switch" middleware can block a namespace. The admin panel connects with `socket.io-client` (`lib/socket/*`). REST code can push socket events through singleton services (for example `CallCenterSocketService`).

### 8. Cron jobs and schedulers

- **lawyer-appointment** (`node-cron` and `setInterval`): call-center command-center jobs (fact builder, rollup, snapshot, daily close, AI attach), AI-call watchdog, Exotel daily job, meetings job (every minute), welcome WhatsApp jobs, report sweep (every minute), app-call transcription (every 5 minutes), chat contact-sharing scan (hourly), overdue task check (hourly). Many use a Redis lock so only one instance runs a job.
- **Jenkins**: `lawyer-appointment/Jenkinsfile` is a scheduled-call poller. Jenkins calls internal HTTP endpoints every minute (prep, fire, cleanup legs) using the internal secret.
- **user-service**: a `cron` module exposes HTTP endpoints (for example daily onboarding summary) protected by the internal API key. An outside scheduler triggers them.
- **comm-service**: `whatsappRetry.job.js` (`node-cron`) retries failed WhatsApp messages.
- **gateway**: no business cron. Only the 60-second deactivated-user sync and a health-check timer.

### 9. External integrations (names only)

- Telephony: Exotel (calls, webhooks, WebRTC SDK in admin panel), Bonvoice (second provider, on the `VS-647-bonvoice-integration` branch).
- Audio/video rooms: LiveKit.
- Push: Firebase Cloud Messaging (`firebase-admin`), Apple APNs (`@parse/node-apn`).
- SMS: SMSCountry provider in comm-service. user-service has `SMS_GATEWAY_*` env names.
- Email: Gmail API (`googleapis`) and Nodemailer.
- WhatsApp: Interakt.
- Storage: AWS S3 (AWS SDK v3 in lawyer-appointment, `boto3` in doc-service).
- Alerts: Slack (`@slack/web-api`) in user-service and lawyer-appointment.
- AI: Google Gemini (`@google/genai`), OpenAI SDK, Anthropic SDK. Used for transcription, call analysis and translation.
- Payments: handled by finance-service. Provider name not confirmed.
- Session replay: OpenReplay in admin panel.

### 10. Deployment and running (what is visible)

- Every repo has a `Dockerfile` and `docker-compose` files. The gateway image builds TypeScript and runs `node dist/src/server.js` as a non-root user. Its compose file starts Redis next to it with health checks.
- `user-service/bitbucket-pipelines.yml`: builds a Docker image with the AWS CLI image, pushes it, then a step named "Update Kubernetes Manifests" (GitOps style). So user-service runs on Kubernetes. For the other repos the same setup is likely but not confirmed in their own files.
- `lawyer-appointment/Jenkinsfile`: scheduler job (not a deploy pipeline).
- Env files: `.env`, `.env.example` (and `.env.production.template` in lawyer-appointment). Gateway also has `config/environments/{development,staging,production}.ts`.
- PM2: not found.
- Release branches are named `release-DDMMYY`.

### 11. Logging and monitoring

| Repo | Logging | Monitoring |
|---|---|---|
| vs-gateway | Pino (JSON). Every request gets a request id and correlation id. Sensitive headers are redacted in logs. | `/health`, `/ready`, `/status`, `/redis`. No APM agent found. |
| user-service | Winston through `nest-winston`, with New Relic log enricher | New Relic agent, Slack alerts (low / medium / high channels), Swagger docs |
| lawyer-appointment | Winston | New Relic agent, Slack alerts kernel (`src/core/alerts`), Swagger UI |
| comm-service | Winston | Notification logs table |
| doc-service | Python `logging` | Flower for Celery |
| admin-panel | Browser console | OpenReplay |

### 12. Life of a request (admin approves a lawyer on the dashboard)

Example: an admin clicks "Dashboard approve" for a lawyer.

1. **Browser**: a React component calls `authorizedFetch('/api/proxy/admin/dashboardApproveLawyer', { method: 'POST', body: { lawyerId, status } })`. The JWT is sent in the `Authorization` header.
2. **Next.js server** (`app/api/proxy/admin/[...path]/route.ts`): checks the `Authorization` header is present. Builds the gateway URL `/g/us/api/v1/admin/dashboardApproveLawyer`. Forwards method, body and token with `fetch`.
3. **Gateway, global hooks**: creates request id and correlation id, logs the request, adds security headers.
4. **Gateway, route match**: Fastify matches `POST /g/us/api/v1/admin/dashboardApproveLawyer`. This is route id `dashboard-approve-lawyer` in `config/services/user-service/admin-controller.ts`.
5. **Gateway, `auth` middleware**: verifies the JWT (HS256, issuer, audience). Rejects with 401 if missing, expired or invalid. Then checks the user is not in the deactivated-user set.
6. **Gateway, validation**: required headers `Authorization` and `Content-Type` must be present. Body must exist and be `application/json`. Header filter keeps only `Authorization`, `Content-Type`, `Accept`.
7. **Gateway, proxy**: adds `x-forwarded-*`, `x-request-id`, `x-user-language`. Calls user-service with Axios, timeout from the route.
8. **user-service, guards**: `JwtAuthGuard` verifies the same JWT again with Passport and puts `userId`, `roles`, `permissions` from the token on `request.user`. The handler is marked `@Permissions('lawyer:update')` and uses `JwtAuthGuard` + `RolesGuard`. (Most other admin routes use `RolesPermissionsGuard`, which compares the needed permission with the permissions inside the token. `RolesGuard` as I read it checks roles only, so how the permission is enforced on this exact route is not confirmed.) The global `ValidationPipe` checks the DTO (`whitelist`, `forbidNonWhitelisted`, `transform`).
9. **user-service, service**: `AdminService.dashboardApproveLawyer` loads the user with TypeORM. If approving, it first runs readiness checks (for example basic details and professional details must be complete). Then it saves `isDashboardApproved`, who did it, and when. An `@AuditAction` decorator records an admin audit log.
10. **MySQL**: one `SELECT` and one `UPDATE` on the users table.
11. **Way back**: user-service returns JSON. The gateway copies status and body, adds `x-gateway` and `x-response-time` headers, logs "Request completed". The Next.js route returns the same status and JSON to the browser. The UI shows a toast.

If user-service answers 400/403/404, the gateway passes that status and message through. If user-service is down or answers 5xx, the gateway retries (up to the route's attempt count) and finally returns 502 or a timeout error.

---


---

# Part 2: Exotel Telephony Integration

## Exotel Telephony Integration (Deep Dive)

Source of truth: the code in `lawyer-appointment` (also called vs-main), read on branch `VS-677-fix-call-landing-bug`, plus `vs-gateway` and `admin-panel`. Everything below was checked in code unless it says "not confirmed".

### 1. What Exotel is and why we use it

- Exotel is a cloud telephony provider. It owns the phone lines and the virtual number. Our backend only tells it what to do over HTTP and listens to its webhooks.
- VakeelSaab has a call center. Agents talk to clients and lawyers. Clients also call a virtual number.
- What the code uses Exotel for:
  - **Inbound calls.** A client dials the virtual number. Exotel asks our backend "who should I ring?". We reply with one agent.
  - **Outbound calls (click to call).** An agent clicks a button in the admin panel. Exotel rings the agent first, then the client, then joins both.
  - **Consultation calls.** The same outbound API joins a lawyer and a client. The call is tagged with the consultation id.
  - **Lawyer confirmation calls.** An agent calls a lawyer to confirm a booking.
  - **Number masking.** Every call goes out with `virtual_number` (env `EXOTEL_CALLER_ID`). Inbound routing replies with `outgoing_phone_number` = the virtual number. So the parties see the company number, not each other's number. (The masking itself is done by Exotel. Our code only passes the virtual number.)
  - **Recording.** Every call asks for dual channel recording.
  - **Browser calling (WebRTC/SIP).** Agents with VoIP enabled take calls in the browser using Exotel's web SDK.
- One backend codebase runs as two deployables: an HTTP service and a Socket.IO service. Both use the same MySQL database.

### 2. File map

All paths are inside `lawyer-appointment/` unless another repo is named.

| File | What it does |
|---|---|
| `src/features/call-center/exotel/exotel-api.service.ts` | `ExotelApiClient`. The axios client. Builds requests for Exotel v2 and v3 APIs. |
| `src/features/call-center/exotel/exotel-integration.service.ts` | `ExotelIntegrationService`. Business layer for outbound calls, lawyer confirmation, end call, get recording. |
| `src/features/call-center/exotel/exotel-webhook.service.ts` | `ExotelWebhookService` (singleton, about 4,180 lines). Inbound routing, all webhook events, status mapping, auto disposition, stale call cleanup. |
| `src/features/call-center/exotel/exotel.controller.ts` + `exotel.routes.ts` | HTTP endpoints under `/api/v1/call-center/exotel/...`. |
| `src/features/call-center/exotel/exotel-webhook.controller.ts` + `exotel-webhook.routes.ts` | Older webhook endpoints under `/api/v1/call-center/webhooks/...` (mostly legacy). |
| `src/features/call-center/exotel/ccm-routing-lock.ts` | Redis lock and response cache per `CallSid`. |
| `src/features/call-center/exotel/abandoned-call.ts` | Pure rules for "caller already hung up". |
| `src/features/call-center/exotel/dtmf-priority.ts` | Pure rules for digit based priority routing. |
| `src/features/call-center/exotel/blocked-number.service.ts` | Denylist lookup for inbound callers. |
| `src/features/call-center/exotel/recording-migration.service.ts` | Copies a recording from Exotel to S3. |
| `src/features/call-center/call/call.service.ts` | `CallService`. Creates and updates `call_logs` rows. Atomic agent claim. |
| `src/features/call-center/call/call.controller.ts` + `call.routes.ts` | Call log APIs, bulk recording migration, the "refresh status" button. |
| `src/features/call-center/call/dto/initiate-call.dto.ts` | `CallStatus`, `CallType`, `CallDirection` enums and `TERMINAL_STATUSES`. |
| `src/core/socket/call-center/call-center.ts` + `call-center-socket.service.ts` | Socket.IO namespace `/call-center` and emit helpers. |
| `src/database/models/call-log.model.ts` | `call_logs` table. |
| `src/database/models/call-center-agent.model.ts` | `call_center_agents` table, with VoIP/SIP columns. |
| `src/features/call-center/ai/gemini-transcription.service.ts` | Transcription and analysis of a recording. |
| `vs-gateway/config/services/call-center.ts` | Gateway routes under `/g/cc`. |
| `admin-panel/lib/voip/exotel-webrtc.service.ts`, `admin-panel/hooks/use-call-center-socket.ts` | Browser phone and socket listeners. |

Routes are mounted in `src/features/call-center/index.ts` (`/exotel`, `/webhooks`) and in `src/app.ts` (`/api/v1/call-center`). `app.ts` also calls `ExotelWebhookService.getInstance()` on boot so its cron is registered.

### 3. The Exotel API client

`ExotelApiClient` (in `exotel-api.service.ts`) creates three axios instances in its constructor:

| Client | Base URL shape | Used for |
|---|---|---|
| `client` | `https://<ccm domain>/v2/accounts/<sid>` | `makeCall()` - outbound call (v2) |
| `clientV3` | `https://<ccm domain>/v3/accounts/<sid>` | `makeCallV3()` and `getCallDetails()` |
| `contactClient` | `https://<contact domain>/v2/accounts/<sid>` | `validateOrCreateContact()` |

- Auth is HTTP Basic: `base64(apiKey:apiToken)` in the `Authorization` header.
- Timeout is 30 seconds on each client.
- `isDummyConfig()`: if the env variables are missing, the client returns a fake success response. This lets the app run locally without Exotel.
- The client logs a curl version of each request for debugging. The Authorization header is printed as `<redacted>` because logs are shipped to New Relic.
- DND handling: if Exotel replies 403 with error code 10717, or the text mentions NDNC/TRAI, the client throws `DndError`. The UI can then show "this number is on the Do Not Disturb list".
- `makeCallV3` filters `status_callback` events. v3 accepts only `terminal`, `answered`, `ringing`. v2 style `completed` and `cancelled` would cause a 400, so they are removed. It also converts `max_time_limit` to a number, because v3 rejects a string.

### 4. Outbound call flow (step by step)

**Who triggers it**

- Agent clicks "call" in the admin panel -> `POST /g/cc/api/v1/call-center/calls/connect-agent-client` -> `CallController.connectAgentWithClient` -> `CallService.connectAgentWithClient`.
- Consultation call -> `POST /api/v1/call-center/consultation/initiate-call` -> `ConsultationIntegrationService.initiateCallFromConsultation`.
- Lawyer confirmation -> `ExotelIntegrationService.callLawyerForConfirmation`.
- All of them end in `ExotelIntegrationService.initiateOutboundCall` (or the confirmation variant).

**Steps inside `CallService.connectAgentWithClient`**

1. Load the agent by user id. The agent must be `available` or `after_call_work`.
2. Normalize the client number with `normalizePhoneNumber` (E.164 style).
3. Pick the route: SIP if `voip_enabled` and `sip_id` are set, otherwise PSTN with the agent's phone.
4. Call `initiateOutboundCall`.
5. If the agent is calling back a missed call (`callLogId` given, status `initiated`, no agent), write `handledBy` into the old row's `call_metadata`.
6. If a follow up task id is given, stamp `call_log_id` on the task and set it to `in_progress`.
7. Set the agent to `on_call`, set `current_call_id`, and write two `AgentActivityLog` rows.

**Steps inside `ExotelIntegrationService.initiateOutboundCall`**

1. PSTN path only: call `validateOrCreateContact` so the "from" number exists as a contact in Exotel.
2. Build the request body:
   - `from`: `{ user_contact_uri: <sip id> }` for SIP, or `{ contact_uri: <agent or lawyer phone> }` for PSTN.
   - `to`: `{ customer_contact_uri: <client phone> }`.
   - `virtual_number`: env `EXOTEL_CALLER_ID`.
   - `custom_field`: `consultation_<id>` for a consultation call, else `call_center_outbound`. Lawyer confirmation uses `call_center_lawyer_confirmation`.
   - `recording: true`, `recording_channels: 'dual'`.
   - `max_time_limit` (optional). Used to cap paid consultation calls.
   - `status_callback`: four entries (`terminal`, `answered`, `completed`, `cancelled`). All point to `<EXOTEL_WEBHOOK_BASE_URL>/api/v1/call-center/exotel/webhook/call-details`.
3. Choose the API version:
   - v3 (`makeCallV3`) if `EXOTEL_PLAYBACK_ENABLED=true`, a playback URL exists, and the call is PSTN. v3 can play an audio file to one or both sides before joining them.
   - Otherwise v2 (`makeCall`) -> `POST /calls`.
4. Read the call id from the reply: `response.data.call_sid` (v2) or `response.call_details.sid` (v3).
5. Create the call log with `CallService.initiateCall(...)`. It inserts a `call_logs` row with status `initiated`, type `outbound`, direction `outgoing`, the agent id, the consultation id and metadata. It refuses a duplicate `exotel_call_sid` with `ConflictError`.
6. Set the agent to `on_call`.

**How the legs work**

- Leg 1 ("from" leg): Exotel rings the agent (phone or browser).
- Leg 2 ("to" leg): after the agent picks up, Exotel rings the client.
- Then Exotel bridges both legs. The client sees the virtual number.

**How the row is updated later**

Exotel posts to `/webhook/call-details`. The body has `event_details.event_type` and `call_details`. `processEnhancedCallEvent` handles it (see section 6).

```
Agent clicks call (admin-panel)
   -> Next.js proxy -> vs-gateway /g/cc -> CallController.connectAgentWithClient
   -> CallService.connectAgentWithClient (checks agent, picks SIP or PSTN)
   -> ExotelIntegrationService.initiateOutboundCall
        -> (PSTN) validateOrCreateContact
        -> ExotelApiClient.makeCall (v2)  or  makeCallV3 (playback)
        <- call_sid
        -> CallService.initiateCall  => call_logs row (initiated)
   -> agent status = on_call

Exotel rings agent -> rings client -> bridges
   -> POST /webhook/call-details (answered)  => status answered, socket "answered" + autoOpenModal
   -> POST /webhook/call-details (terminal)  => final status, duration, recording URL
        -> agent = after_call_work (talk > 0) or available (talk = 0)
        -> V3 call details check (async)
        -> recording copied to S3 (async) -> transcription (async)
```

### 5. Inbound call flow (step by step)

Exotel feature used: **CCM Programmable Connect**. The call flow on the Exotel side calls our URL and expects a JSON answer that says where to connect the caller.

**Endpoint**: `GET` (or `POST`) `/api/v1/call-center/exotel/webhook/ccm-programmable-connect` -> `ExotelController.handleCCMProgrammableConnect` -> `ExotelWebhookService.handleIncomingCallRouting`.

**Params read from the request**: `CallSid`, `From`, `To`, `Direction`, `CallStartTime`, `Created`, `CurrentTime`, `CallStatus`, `CallType`, `DialWhomNumber`, `DialCallStatus`, `numbers` (JSON of dial legs), `digits` (DTMF key pressed; the code strips the quote marks Exotel puts around it).

**Important behaviour of Exotel**: we reply with `fetch_after_attempt: true`. This means Exotel calls the same URL again after each attempt. So one call can hit this endpoint many times, sometimes at the same moment. Most of the hard problems come from this.

**Steps in `handleIncomingCallRouting`**

1. **Denylist check** (`blockIfDenylisted`). If the caller is in `cc_blocked_numbers`, write a call log that is already final (`failed`, `ended_at` set, `call_metadata.blocked = true`) and reply with no destination and `fetch_after_attempt: false`. Nobody rings.
2. **Single flight lock** (only if `CCM_SINGLE_FLIGHT=true`). `CcmRoutingLock.acquire(callSid)` does a Redis SET-if-not-exists with a 15 second TTL.
   - Winner: runs routing, caches the reply for 300 seconds, releases the lock.
   - Loser: waits up to 4 x 200 ms for the cached reply and returns the same reply. If none, it returns the "queued" reply. It never picks a second agent.
   - Redis down: continue without the lock and send a high alert (deduped).
3. `routeCall` picks `routeCallAtomic` (if `CCM_ATOMIC_CLAIM=true`) or `routeCallLegacy`.

**Steps in `routeCallAtomic`**

1. Find an existing `call_logs` row by `exotel_call_sid`.
2. **Abandoned call guard** (`handleAbandonedPoll`). If the poll already says `CallStatus=completed` and no agent holds the call, the caller hung up during the greeting. Close the row as `failed`, do not claim an agent, do not show a banner.
3. **Retry detection**. It is a retry if `CallStatus` is `in-progress` or `completed`, or `CallType` is `incomplete`, or a leg in `numbers` has an empty `end_time` (still ringing). If the row already has an agent, **replay** the same destination from `call_metadata.routing_destination` (`rebuildResponseFromCallLog`). No new selection.
4. **Failed dial leg**. If `DialCallStatus=failed` and nothing is ringing, release that agent (`releaseAgentClaim`), add the agent to `attempted_agent_ids`, clear `agent_id`, delete the cached reply, and choose another agent.
5. **DTMF priority**. Exotel sends `digits` only on the first poll. So the digit is saved as `call_metadata.dtmf_priority` and reused on later polls.
6. `CallService.createOrUpdateIncomingCall(callSid, ...)` makes sure a row exists. `agent_id` is not set yet.
7. Load available agents: `AgentService.searchAgents({ status: AVAILABLE, limit: 50, internal: true })`. Remove agents already tried.
8. No agent -> `markQueuedAndRespond` (status `queued` when `CALL_STATUS_V2=true`, socket update with `agentId: 'unassigned'`), reply "queued".
9. Priority narrowing. Keep only agents whose `metadata.priorities` contains the digit. If none, keep the caller waiting for `spilloverAttempts` polls (default 2), then fall back to any agent.
10. **Rank** with `rankCandidates`: sticky agent first (the agent who last completed a call with this customer), then the others by lowest assignment count, and the agent who got the last call goes to the end.
11. **Claim loop**. For each candidate: `resolveAgentDestination` (SIP trunk if VoIP, else phone number), then `CallService.claimAgentForCall`. First successful claim wins.
12. **Guarded write** of `agent_id` and routing metadata into the call log. If the write affects 0 rows, release the claim and replay the winner's reply.
13. `incrementAssignmentCount` (Redis hash `ccm:assign_counts`, 24 h TTL, in-memory fallback).
14. Socket: `emitIncomingCall(agentId, {...})` with `callSid`, `customerNumber`, `routingMethod`, `isSticky`, `dtmfPriority`, `dtmfPriorityMatched`.
15. Reply to Exotel with `buildAgentRoutingResponse`.

**Reply fields we send to Exotel** (`buildAgentRoutingResponse`): `fetch_after_attempt: true`, `destination` (`{ numbers: [...] }` or `{ trunk: ... }`), `state_management: true`, `outgoing_phone_number`, `sticky_agent`, `record: true`, `recording_channels: 'dual'`, `max_ringing_duration: 30`, `max_conversation_duration: 1800`, `music_on_hold`, `start_call_playback` (a short text greeting; "Welcome back" for sticky), `sip_headers` (SIP only), and `dial_passthru_event_url` = `<base>/api/v1/call-center/exotel/webhook/call-events`.

**How the dashboard is told (sockets)**

- Socket.IO namespace `/call-center`. The client sends a JWT. Agents join room `agent_<agentId>`. Managers join room `managers`.
- `emitIncomingCall` -> event `notification` with `type: 'incoming_call'` to the agent room, and `type: 'incoming_call_monitor'` to managers (a different type, so managers do not get the agent popup).
- `emitCallUpdate` -> event `call_status_updated` to the agent room and to managers. Managers always get `autoOpenModal: false`.
- `emitAgentStatusChange` -> `agent_status_updated` (managers) and `status_updated` (agent).

**What happens after routing** (events on `/webhook/call-events`, handled by `processCallEvent`)

| Event | What we do |
|---|---|
| `Dial` | Call log -> `ringing`. If status is `busy`, mark agent as ringing and save `actively_ringing_agent_id`. No second popup. |
| `Ringing` | Agent -> on call (ringing), call log -> `ringing`. |
| `Answered` | Agent -> `on_call`, socket `answered`, call log -> `answered` (skipped if already final). |
| `Terminal` / `Completed` / `Cancelled` | If `Status=free`: agent -> `available`, fix wrong agent if needed, socket `completed`. If the row is still not final, write the final status. Then create the auto disposition. |
| No `EventType` but has `DialCallStatus` or `RecordingUrl` or `DialCallDuration` | This is the final "call completed" payload. Parse legs, set final status, duration, recording, agent -> `after_call_work`. |

```
Client dials virtual number
  -> Exotel flow (greeting, optional digit)
  -> GET /webhook/ccm-programmable-connect   (repeats while fetch_after_attempt = true)
       -> blocked?            -> failed row, no destination, stop
       -> Redis lock per CallSid (winner routes, losers replay cached reply)
       -> caller already gone? -> close row as failed, no agent
       -> retry of assigned call? -> replay same destination
       -> pick agent: sticky -> least assigned -> last assigned at the end
       -> atomic claim (available -> on_call) -> guarded write on call_logs
       -> socket: incoming_call (agent) + incoming_call_monitor (managers)
       <- JSON { destination, record, dial_passthru_event_url, ... }
  -> Exotel rings the agent (phone or browser SIP)
  -> GET /webhook/call-events  Dial -> Ringing -> Answered -> Terminal
  -> final payload (legs, duration, RecordingUrl)
       -> final status + auto disposition + S3 copy + transcription
No agent free -> row = queued -> reply with empty destination -> Exotel holds caller and polls again
```

### 6. Webhook handling

**Endpoints**

| Path (under `/api/v1/call-center`) | Method | Handler | Purpose |
|---|---|---|---|
| `/exotel/webhook/ccm-programmable-connect` | GET, POST | `handleCCMProgrammableConnect` | Inbound routing decision. |
| `/exotel/webhook/call-events` | GET, POST | `handleWebhookEvents` -> `processCallEvent` | Inbound passthru events (flat fields). |
| `/exotel/webhook/call-details` | GET, POST | `handleWebhookCallDetails` -> `processEnhancedCallEvent` | Outbound status callbacks (nested JSON). |
| `/exotel/webhook/internal/cleanup-stale-calls` | POST | `cleanupStaleCalls` | Cron entry. |
| `/exotel/webhook/internal/backfill-stuck-call-dispositions` | POST | `backfillStuckCallDispositions` | Cron entry. |
| `/webhooks/incoming-call`, `/call-status`, `/recording`, `/dtmf` | mixed | `ExotelWebhookController` | Older style. Only `/recording` does real work (saves URL, starts S3 copy). |

**Payload fields used**

- Flat format (`call-events`): `CallSid`, `From`, `DialWhomNumber`, `Status`, `EventType`, `DialCallStatus`, `DialCallDuration`, `RecordingUrl`, `RecordingAvailableBy`, `CurrentTime`, `EndTime`, `StartTime`, `OutgoingPhoneNumber`, `flow_id`, `tenant_id`, and leg fields `Legs[i][Number]`, `[Type]`, `[OnCallDuration]`, `[CallerId]`, `[CauseCode]`, `[Cause]`, `[DisconnectedBy]`.
- Nested format (`call-details`): `event_details.event_type`; `call_details.call_sid` (v2) or `call_details.sid` (v3); `call_details.call_status` / `status`; `total_talk_time`; `end_time`; `updated_time`; `recordings[0].url`; `custom_field`.
- Legs: the code loops over `Legs[0]`, `Legs[1]`, ... The leg with the biggest `OnCallDuration` is taken as the agent who really talked, and that value is the talk time.

**Status mapping** - one function, `resolveCallStatus`, is the single source of truth (when `CALL_STATUS_V2=true`). It looks at the Exotel word, the talk time, the direction and the current DB status.

| Exotel status | Internal status |
|---|---|
| empty or `free`, talk > 0 | `short_call` (<= 30 s) or `completed` |
| empty, row was `ringing` | `missed` |
| empty, row was `initiated`/`queued` | `failed` |
| row was `queued` and no talk | `failed` (no agent phone was ever dialed) |
| `from_leg_unanswered`, outbound | `no_answer` (agent did not pick their own phone) |
| `from_leg_unanswered`, inbound | `missed` if row reached `ringing`, else `failed` |
| `agent_unanswered` | `no_answer` |
| `no-answer`, `customer_unanswered`, `to_leg_unanswered` | `missed` |
| `canceled`, `cancelled`, `agent_canceled`, `from_leg_cancelled`, `to_leg_cancelled` | `cancelled`, or `completed`/`short_call` if there was talk time |
| `failed`, `customer_no_dial`, `agent_no_dial`, `to_leg_no_dial`, `from_leg_no_dial` | `failed` |
| `busy`, `customer_busy` | `busy` |
| `to_leg_answered` | `completed`/`short_call` by talk time; `busy` if zero talk |
| `from_leg_answered` | by talk time; else `cancelled` if row was `answered`, else `missed` |
| `completed`, `in-progress` | `missed` (0 s), `short_call` (<= 30 s), `completed` |
| unknown word | `failed` plus a low Slack alert, deduped per word for 24 h |

Older simple maps still exist for the flag-off path: `mapExotelStatusToCallStatus` and `mapEnhancedCallStatus`.

**Idempotency and duplicate handling**

- `call_logs.exotel_call_sid` has a unique index. A second insert for the same call fails.
- `createOrUpdateIncomingCall` catches `UniqueConstraintError`, reads the row again and goes to the update path.
- Late `answered` events are ignored if the row is already in `TERMINAL_STATUSES`.
- `Terminal` writes the final status only if the row is not final yet.
- Rows with `call_metadata.blocked` are frozen: both event processors return early.
- Auto disposition checks `hasExistingDisposition(callLogId)` first.
- Every event is appended to `call_metadata.callEvents` as an audit trail.

**Retries**

- The two main webhook handlers always answer HTTP 200, even when processing throws. The comment says this stops Exotel from retrying.
- The routing endpoint returns a safe fallback JSON on error instead of a 5xx, so the caller is not dropped.
- A code comment says Exotel sends the final webhook at most three times and then stops. The stale call cron covers that gap.
- The gateway also has its own upstream retry setting for the service. How it applies to these routes: not confirmed.

**Security and validation (honest picture)**

- `validateWebhookSignature` exists: HMAC-SHA1 of `url + payload` with the API token, compared to header `x-exotel-signature`. It is only called in the two legacy handlers (`handleIncomingCall`, `handleCallStatusUpdate`).
- The three live webhooks (`ccm-programmable-connect`, `call-events`, `call-details`) have no signature check and no auth. In the gateway their `middleware` list is empty.
- The routing handler validates input by hand: `CallSid`, `From`, `To` must exist.
- The legacy routes use `class-validator` DTOs (`ExotelIncomingCallDto`, `ExotelRecordingDto`, `ExotelDtmfDto`).
- Whether an IP allowlist exists at the network level: not confirmed.
- `cleanup-stale-calls` has no auth "by decision": it is idempotent and only writes what Exotel's own API reports.
- `migrate-recordings` accepts a user JWT or the `X-Internal-Secret` header (`internalSecretOr` middleware).

### 7. Call logs data model

**Table `call_logs`** (`CallLog` model, Sequelize, MySQL)

| Column | Notes |
|---|---|
| `id` | UUID primary key. |
| `exotel_call_sid` | Unique. The join key for every webhook. |
| `call_type` | `inbound`, `outbound`, `transfer`. |
| `agent_type` | `human` or `ai`. |
| `call_direction` | `incoming`, `outgoing`. |
| `customer_number`, `customer_user_id` | Client phone and resolved user id. |
| `lawyer_number`, `lawyer_user_id` | Set on lawyer calls. |
| `agent_id` | FK to `call_center_agents`. Nullable (queued or missed calls have none). |
| `consultation_id` | Link to a consultation. |
| `call_status` | MySQL ENUM, see below. |
| `initiated_at`, `answered_at`, `connected_at`, `ended_at` | Timeline. |
| `duration_seconds` | Talk time. |
| `recording_url`, `recording_duration` | Exotel URL. |
| `call_metadata` | JSON. Holds almost all extra state. |
| `exotel_response` | JSON. |

**Statuses** (`CallStatus` enum)

- Live: `initiated`, `queued`, `ringing`, `answered`.
- Final success: `completed`, `short_call` (talk time 30 s or less).
- Final failure: `missed`, `busy`, `cancelled`, `failed`, `no_answer`.

**Useful keys inside `call_metadata`**: `callEvents`, `legs`, `routing_method`, `routing_destination`, `destination`, `is_sticky_agent`, `assignment_attempts`, `attempted_agent_ids`, `no_agent_reason`, `dtmf_priority`, `blocked`, `abandoned_before_connect`, `pre_terminal_db_status`, `api_verified_at`, `api_raw_status`, `api_resolved_status`, `status_reconciliation`, `internal_recording_url` (the S3 key), `recording_migration_attempts`, `recording_migration_failed`, `callback_claim`, `handledBy`, `agent_corrected_at_terminal`.

**Relations**

- Agent: `call_center_agents` has `status` (`available`, `on_call`, `after_call_work`, `break`, `offline`) and `current_call_id`. It also holds VoIP fields: `voip_enabled`, `sip_username`, `sip_id`, `sip_domain`, `exotel_trunk_name`, `exotel_app_id`, `exotel_app_user_id`, and `metadata.priorities`.
- Disposition: `call_dispositions.call_log_id`. One disposition per call is the rule.
- Consultation: the `consultation_<id>` custom field drives it. On `answered` the consultation moves to `ONGOING`. On terminal it runs `postWorkAfterConsultationCall` (billing work) and moves to `COMPLETED`. The call log gets `consultation_id` through `assignToConsultation`.
- Transcription: `call_transcriptions.call_log_id`.

### 8. Auto disposition, missed calls and crons

**Auto disposition** (`shouldCreateAutoDisposition`, `createAutoDisposition`)

- Created when an inbound call ends, so the agent must fill in what happened.
- Skipped for outbound calls, consultation calls, and calls that already have a disposition.
- Category `system_generated`, `resolution_status: pending`, `is_manual_entry: false`.
- Sub category by outcome:
  - no agent -> `incoming_call_missed`
  - agent but zero talk -> `incoming_call_no_answer`
  - talk under 30 s -> `incoming_call_short`
  - else -> `incoming_call_answered`
- After it is created, a socket update with status `disposition_required` goes to the agent.
- `updateSystemGeneratedDisposition` runs after the final payload and corrects the sub category with the real talk time.

**When Exotel retries the call to another agent ("call hopping")**

- Problem: Exotel polled the routing URL again while an agent's phone was still ringing. The old code picked a new agent and overwrote `agent_id`. Then the disposition and the call log pointed to the wrong agent.
- First fix (commit "handled the auto disposition creation when the exotel is trying the calls again"): track retry attempts and skip disposition creation when the Terminal event's agent did not match the call log.
- Current code does it differently:
  - Guards 1 and 2 stop the overwrite at routing time (CCM status signals and the still-ringing leg).
  - Guard 3 in `createOrUpdateIncomingCall` refuses to change `agent_id` when the row is `ringing` or `answered`.
  - **Terminal agent correction** in `handleTerminalEvent`: `DialWhomNumber` in the Terminal event is treated as the truth. If it does not match `agent_id`, the call log is corrected, the auto disposition (`is_manual_entry: false`) is moved to the right agent, and a high alert is sent.
- Each guard sends a Slack alert with a dedupe key per `CallSid`.

**Stale call cleanup** (`cleanupStaleCalls` + `reconcileCallFromExotel`)

- Triggered by a Jenkins job every 5 minutes through the internal route. Not an in-process cron. Switch: `STALE_CALL_CLEANUP_ENABLED`.
- Picks rows in `answered`, `ringing`, `queued`, `initiated` that are older than 20 minutes and inside a lookback window (default 24 hours).
- For each row it calls Exotel V3 Call Details (`getCallDetails`).
  - Exotel says still active -> write nothing.
  - Exotel says over -> run `resolveCallStatus`, write status, duration, recording, `ended_at`, the `api_*` keys and a `status_reconciliation` marker (who, when, why).
  - API error -> write nothing, try next tick.
- Then it frees the agent with `releaseAgentClaim` and creates the missing auto disposition.
- The same function powers the "refresh" button: `POST /calls/:callId/reconcile-status` (`triggeredBy: 'manual'`, and it stores who pressed it).

**Stuck call disposition backfill** (`backfillStuckCallDispositions`)

- Inbound calls with an agent and no disposition, 16 minutes to 24 hours old.
- Creates only the disposition. It never changes `call_status`.

**Other crons**

- `node-cron` job `0 6 * * *` (Asia/Kolkata by default) resets the per-agent assignment counts. It is registered in the `ExotelWebhookService` constructor.
- After each terminal webhook, `verifyCallStatusViaApi` runs via `setImmediate`. It asks the V3 API once and overwrites status/duration/recording if the API knows better. It sets `api_verified_at` so it runs only once.

### 9. Recordings and transcription

**Where the URL comes from**

- Final inbound payload: `RecordingUrl`.
- Outbound terminal callback: `call_details.recordings[0].url`.
- V3 Call Details API (verification or reconcile).
- Legacy `/webhooks/recording` route.

**Storage flow**

1. The Exotel URL is saved in `call_logs.recording_url`.
2. `CallService.updateCallStatus` sees a new `recording_url` and calls `RecordingMigrationService.schedule`. It uses `setImmediate`, so the webhook reply is not delayed.
3. `migrateToS3`:
   - Download the MP3 into memory with axios (Basic auth, 60 s timeout, TLS verification off for Exotel's recording host).
   - Read the row again. If another run already set the S3 key, stop (race guard).
   - Upload with `s3Service.uploadBuffer` to key `<env>/recordings/<callSid>.mp3` in the bucket from `S3_CALL_RECORDINGS_BUCKET`.
   - Save the key in `call_metadata.internal_recording_url`. No new column.
4. On failure `recordFailure` counts attempts. After 3 failures it sets `recording_migration_failed = true`.

**Serving a recording**

- `CallController.withPresignedRecordingUrl` swaps `recording_url` in the API response for a short lived presigned S3 URL (default 15 minutes). The database value is not changed. If S3 fails, the Exotel URL is returned as a fallback.

**Bulk migration** - `POST /api/v1/call-center/calls/migrate-recordings`

- `{ call_log_ids: [...] }` (max 100) or `{ all: true, limit }` (default 100, max 500 per run).
- `dry_run: true` only counts.
- Default window is 6 months, because Exotel deletes older recordings. `max_age_months` and `force` change this.
- Runs in bounded batches with `Promise.allSettled`. Returns per item results and `remaining_unmigrated`.
- Auth: user JWT or `X-Internal-Secret`, so Jenkins can run it.

**Transcription**

- When the S3 copy is done and `AI_NOTES_ENABLED=true`, `GeminiTranscriptionService.processRecording(callLogId)` runs.
- It uses Google Gemini (`@google/genai`). It picks a prompt by call type: `agent_client`, `agent_lawyer`, or `client_lawyer`.
- Output is saved in `call_transcriptions`: transcript with speakers, summary, sentiment, agent score, lead and agreement info, contact sharing detection.
- Manual trigger: `POST /call-center/transcriptions/trigger`. It rejects the request if a run is already pending. If only the Exotel URL exists, it starts the S3 copy first.

### 10. Gateway and admin panel

**vs-gateway** (`config/services/call-center.ts`, base route `/g/cc`, upstream env `CALL_CENTER_SERVICE_URL`)

| Route id | Path | Middleware |
|---|---|---|
| `exotel-ccm-events` | `/exotel/webhook/ccm-programmable-connect` (GET) | none |
| `exotel-call-events` | `/exotel/webhook/call-events` (GET) | none |
| `exotel-call-details-events` | `/exotel/webhook/call-details` (POST) | none |
| `cleanup-stale-calls` | `/exotel/webhook/internal/cleanup-stale-calls` (POST) | none, 1 retry |
| `backfill-stuck-call-dispositions` | `/exotel/webhook/internal/backfill-stuck-call-dispositions` (POST) | `auth` |
| `call-connect-agent-client` | `/calls/connect-agent-client` | auth |
| `call-reconcile-status` | `/calls/:callId/reconcile-status` (POST) | `auth`, `rate-limit`, 20 s timeout |
| `voip-get-credentials`, `voip-get-status` | `/voip/credentials/:agentId`, `/voip/status/:agentId` | auth |

Note: the gateway allows only listed query params per route. A new param must be added in the gateway too.

**admin-panel** (Next.js)

- `lib/socket/call-center-socket.ts`: connects to `<url>/call-center` with the JWT. Listens to `notification` and `call_status_updated`.
- `hooks/use-call-center-socket.ts`: shows the incoming call banner, updates the store, opens the client modal when an outbound call is answered (`autoOpenModal`), handles `session_conflict` and `session_superseded` (one tab per agent).
- `lib/voip/exotel-webrtc.service.ts`: wraps the Exotel web phone SDK (`@exotel-npm-dev/exotel-ip-calling-crm-websdk`). Loads SIP credentials from `/api/proxy/call-center/voip/credentials/:agentId`. Methods: `initialize`, `makeCall`, `acceptCall`, `hangup`, mute and hold.
- `lib/voip/voip-context.tsx`, `hooks/use-voip.ts`, `components/call-center/VoIPCallController.tsx`: React state and UI for the browser phone.
- `scripts/apply-exotel-sdk-patch.mjs`: a postinstall script. It rewrites the SDK's `require` of sound files to public paths so the Next.js build works.
- `components/call-center/CallStatusBadge.tsx`, `call-log-details-modal.tsx`, `CallLogsSection.tsx`, `app/dashboard/call-center/manager/call-logs/page.tsx`: call log screens.
- `app/api/call-center/calls/[callLogId]/reconcile-status/route.ts` and `lib/api/services/call-center/call-logs-service.ts`: the refresh button.

### 11. Config and env variable names (names only)

| Name | Use |
|---|---|
| `EXOTEL_ACCOUNT_SID`, `EXOTEL_API_KEY`, `EXOTEL_API_TOKEN`, `EXOTEL_SUBDOMAIN` | Account and Basic auth. |
| `EXOTEL_CCM_API_DOMAIN`, `EXOTEL_CONTACT_API_DOMAIN` | API hosts. |
| `EXOTEL_CALLER_ID` | The virtual number. |
| `EXOTEL_WEBHOOK_BASE_URL` | Public base URL used to build callback URLs. |
| `EXOTEL_PLAYBACK_ENABLED`, `EXOTEL_PLAYBACK_AUDIO_URL`, `EXOTEL_PLAYBACK_TO` | v3 audio before connect. |
| `CCM_SINGLE_FLIGHT`, `CCM_ATOMIC_CLAIM` | Race fix flags. Default off. |
| `CALL_STATUS_V2` | Turns on `resolveCallStatus`, `queued` status and API verification. |
| `STALE_CALL_CLEANUP_ENABLED` | Switch for the stale call cron. |
| `S3_CALL_RECORDINGS_BUCKET`, `S3_PRESIGNED_URL_EXPIRY` | Recording storage. |
| `AI_NOTES_ENABLED`, `GEMINI_API_KEY` | Transcription. |
| `INTERNAL_API_SECRET` | Value for the `X-Internal-Secret` header. |
| `CRON_TIMEZONE`, `JWT_SECRET`, `NODE_ENV` | General. |
| `HOLD_MUSIC_URL`, `DEFAULT_AGENT_NUMBER`, `SALES_DEPARTMENT_NUMBER`, `SUPPORT_DEPARTMENT_NUMBER` | Legacy handlers only. |

Runtime config in the `service_configs` table (cached in Redis, changeable without a deploy): `dtmf_priority_routing` (`isEnabled`, `spilloverAttempts`) and `ccm_abandoned_poll_guard` (`isEnabled`).

### 12. Packages and what each does here

| Package | Use in this integration |
|---|---|
| `axios` | All calls to Exotel, and the recording download. |
| `express` (v5) | Webhook and API routes. |
| `sequelize` + `mysql2` | ORM for `call_logs`, agents, dispositions. Conditional `UPDATE ... WHERE` for atomic claims. |
| `ioredis` (via `RedisService`) | Per-call lock, cached routing reply, assignment counts, config cache. |
| `socket.io` | Namespace `/call-center`, rooms per agent and for managers. |
| `node-cron` | Daily reset of assignment counts. |
| `class-validator`, `class-transformer` | DTO validation. |
| `jsonwebtoken` | Socket and API auth. |
| `@aws-sdk/client-s3`, `@aws-sdk/s3-request-presigner` | Upload recordings, presigned play URLs. |
| `@google/genai` | Transcription and call analysis. |
| `winston`, `newrelic` | Logs and monitoring. |
| Node `crypto`, `https` | HMAC signature check; HTTPS agent for the recording download. |
| `@exotel-npm-dev/exotel-ip-calling-crm-websdk` (admin-panel) | Browser SIP phone. |
| `socket.io-client` (admin-panel) | Dashboard events. |

No queue library is used. Background work uses `setImmediate` and fire-and-forget promises. Heavy scheduled jobs are run by Jenkins calling internal HTTP routes.

Tests are plain `ts-node` scripts: `npm run test:ccm-race` and `npm run test:cc-abandoned-call`.

### 13. Tricky problems and design decisions

1. **Two calls claim the same agent.** Old flow was read "available agents" then write. Two calls could pick the same agent. Fix: `claimAgentForCall` is one SQL statement, `UPDATE call_center_agents SET status='on_call', current_call_id=? WHERE id=? AND status='available'`. MySQL row locking lets only one caller get `affectedRows = 1`. The loser just tries the next candidate.
2. **Safe release.** `releaseAgentClaim` includes `current_call_id = <this call>` in the WHERE. So an old call can never free an agent who is already on a newer call.
3. **Same CallSid arrives twice at once.** Exotel polls the routing URL. Fix: Redis lock per `CallSid` plus a cached reply. The cached reply is read from the Redis master, because replica lag would break a sub-second race.
4. **Fail open on Redis.** If Redis is down, routing continues without the lock. The DB claim still protects agents. One deduped alert is sent instead of hundreds.
5. **Concurrent insert of the call log.** The unique index on `exotel_call_sid` plus catching `UniqueConstraintError` turns a crash into a normal update.
6. **Never return a destination the DB does not record.** If the guarded write fails, the claim is released and a queued reply is sent.
7. **Call hopping and wrong agent on disposition.** Three guards at routing time, plus correction at the Terminal event using `DialWhomNumber`.
8. **"Dashboard shows a call but the phone never rings."** The caller hung up during the greeting, but the poll still came with `CallStatus=completed`. Old code claimed an agent and showed a banner. Fix: the abandoned poll guard closes the row as `failed`. The rule is a pure function (`decideAbandonedPoll`) with its own test, and the switch is in the DB with default off.
9. **Do not blame the agent.** `from_leg_unanswered` on an inbound call is `missed` only if the row reached `ringing`. If nobody was dialed, it is `failed`.
10. **Lost final webhook.** Rows stayed `ringing` or `answered` forever and agents stayed `on_call`. Fix: reconcile from the V3 Call Details API. Rule: never guess; write only when Exotel says the call is over.
11. **Webhook data can be wrong.** After each terminal webhook the V3 API is asked once and treated as the authority.
12. **Timezone bug.** Exotel sends naive IST strings. `new Date()` read them as UTC, 5 h 30 m off. `parseExotelTimestamp` adds `+05:30` when no offset is present.
13. **Wrong final status.** The old check for the final payload was `DialCallStatus === 'completed'`. Calls ending in no-answer or busy fell to the legacy path and were mapped back to `answered`. Now any payload without `EventType` but with `DialCallStatus`, `RecordingUrl` or `DialCallDuration` is treated as final.
14. **DTMF digit vanishes on retries.** Exotel sends `digits` only on the first poll. The digit is stored in call metadata and reused.
15. **Blocked callers.** The row is written already final and flagged, so no cron and no late event can reopen it. `failed` was reused to avoid an ALTER on the busiest table.
16. **Fair load.** Assignment counts moved to a Redis hash so they survive restarts and work across replicas. Counting happens after a successful claim, not at selection.
17. **Double popup for managers.** `Dial` no longer re-sends the incoming call event, and managers get a separate `incoming_call_monitor` type.
18. **Recording copy race.** Two triggers could both upload and start two transcriptions. A re-read of the row after download stops the second one.
19. **Dead recordings.** Exotel deletes recordings after about 6 months. Bulk migration has an age window and a "permanently unavailable" marker after 3 failed tries.
20. **Staged rollout.** Risky changes sit behind flags (`CCM_ATOMIC_CLAIM`, `CCM_SINGLE_FLIGHT`, `CALL_STATUS_V2`), and the legacy path is kept for rollback.
21. **Secrets in logs.** The debug curl line never prints the real Authorization header.

### 14. Interview questions and answers

**Q1. What is Exotel and what did you build with it?**
Exotel is a cloud telephony provider. I built the full call center integration: outbound click-to-call, inbound routing to agents, webhook processing, call logs, auto dispositions, recording storage in S3, and cleanup jobs for stuck calls.

**Q2. How does an outbound call work?**
The agent clicks call. Our backend sends a POST to Exotel's calls API with `from` (agent), `to` (client), the virtual number, recording on, a custom field and status callback URLs. Exotel returns a call SID. We save a `call_logs` row with status `initiated`. Exotel rings the agent first, then the client, and joins them. Status callbacks update the row.

**Q3. How does an inbound call reach an agent?**
Exotel calls our routing webhook with the CallSid and caller number. We pick an available agent, lock that agent in the database, save the assignment, push a socket event to the agent's dashboard, and return JSON with the destination. Exotel then dials that agent.

**Q4. How do you choose the agent?**
First the "sticky" agent, the one who last completed a call with this customer. Then the agent with the fewest assignments today. The agent who got the last call is moved to the end. If the caller pressed a digit, we first try agents who serve that digit, and after a set number of retries we fall back to any agent.

**Q5. Two calls arrive at the same time. How do you stop both going to one agent?**
The claim is one conditional SQL update: set status to `on_call` only where status is `available`. The database lets only one request change the row. The other gets zero affected rows and tries the next agent. I did not need a distributed lock for this part.

**Q6. Exotel calls your routing URL many times for one call. How do you handle it?**
With a Redis lock per CallSid. The first request does the routing and caches the reply for five minutes. Other requests return the same cached reply. Also, if the call already has an agent and Exotel signals a retry, we replay the stored destination and do not choose again.

**Q7. What if Redis is down?**
Routing still works. The lock is skipped, an alert is sent, and the database level claim still stops double assignment. The system fails open so calls are never blocked by a cache outage.

**Q8. How do you handle duplicate or out of order webhooks?**
The call SID is unique in the table. Inserts that collide fall back to an update. A late `answered` event is ignored if the call is already final. The final status is written only if the row is not final yet. Dispositions are created only if none exists. We always answer 200 so Exotel does not keep retrying.

**Q9. What if the final webhook never arrives?**
A scheduled job runs every five minutes. It finds calls still live after 20 minutes and asks Exotel's call details API. If Exotel says the call ended, we write the real status, duration and recording, free the agent, and create the missing disposition. If Exotel says it is still active, or the API fails, we write nothing.

**Q10. How do you map Exotel statuses to your own?**
One function, `resolveCallStatus`. It uses the Exotel status word, the talk time, the call direction and the current database status. For example, `completed` with zero talk time is `missed`, and with 30 seconds or less it is `short_call`. Unknown words become `failed` and send one alert.

**Q11. What is auto disposition and what went wrong with retries?**
When an inbound call ends, we create a pending disposition so the agent must record the outcome. When Exotel retried a call to another agent, the agent id on the call could be overwritten, so the disposition went to the wrong person. I added guards to stop the overwrite, and at the Terminal event I trust the number Exotel actually dialed and correct both the call log and the disposition.

**Q12. How are recordings stored?**
We save Exotel's URL first. Then, in the background, we download the MP3 into memory and upload it to S3 under a key built from the call SID. The key is saved in the call's JSON metadata. The API returns a short lived presigned URL. There is a bulk endpoint with dry run for old recordings, because Exotel deletes them after about six months.

**Q13. How does the dashboard update in real time?**
Socket.IO, namespace `/call-center`, JWT auth. Each agent joins a room named by agent id and managers join a managers room. The backend emits `notification` for incoming calls and `call_status_updated` for status changes.

**Q14. How are the webhooks secured?**
Honest answer: there is an HMAC-SHA1 signature function, but it is only used in the older handlers. The live webhooks are public and rely on input checks, idempotent writes, and matching by CallSid. If I improved it, I would add signature or IP checks at the gateway.

**Q15. Why feature flags, and why keep the old routing code?**
Call routing is live and risky. The new atomic path and the lock are behind env flags, so they can be turned on per environment and turned off quickly. The old path stays until the new one has run safely in production. Small thresholds live in a database config table so they change without a deploy.

**Q16. What was a hard bug you found?**
Agents saw an incoming call on screen but their phone never rang. I found that the caller had already hung up, but Exotel still sent the routing request with status `completed`. We were claiming an agent for a dead call. I added a guard that closes such calls as `failed` without touching any agent, wrote it as a pure function with tests, and put it behind a database switch.

### 15. Things not confirmed

- Whether `CCM_ATOMIC_CLAIM`, `CCM_SINGLE_FLIGHT`, `CALL_STATUS_V2` and `STALE_CALL_CLEANUP_ENABLED` are on in production. Repo notes say the two CCM flags default to off.
- The abandoned call guard (`abandoned-call.ts` usage, its config service and its test) is in the working tree of branch `VS-677-fix-call-landing-bug`. Parts are not committed yet, so it may not be live.
- Whether `call_dispositions.call_log_id` has a unique index in the real database. One migration removed it, a later SQL note says it exists. The code also checks before insert.
- Any network level protection (IP allowlist) for the public webhooks.
- The exact Exotel dashboard setup (flow/applet that calls the routing URL and collects the digit). It is not in the repos.
- Jenkins job definitions and their real schedules. Only code comments describe them.
- Call transfer (`transferCall`) builds XML but does not send it to Exotel in the code read; treat transfer as not finished.

---

# Part 3: Appointment and Call-Center Service

## Appointment / Call-Center Service (lawyer-appointment)

**What this repo is.** One Node.js + TypeScript codebase (package name `lawyer-appointment-system`). It is built into two running services that share one MySQL database:

- the HTTP API service (consultations, scheduling, chat, cases, documents, call-center REST APIs)
- the call-center socket service (Socket.IO, live dashboard for agents)

**My work here.** About 185 non-merge commits from Dec 2025 to Nov 2026. Most of it is the call-center: who gets a call, what status a call ends in, what the agent must do after the call (dispositions and follow-up tasks), and the reports and exports around it. I also built scheduling and case APIs for the mobile app, and a second telephony provider (Bonvoice).

**How to read this file.** Each feature has: why it was needed, what I built, how it works step by step, key files, DB tables, packages, tricky parts, and interview questions with short answers.

**Branch note (important for honesty in interviews).**
- Most features are on the current working branch and the release branches. Some newer ones (Aug to Oct 2026) are in `release-171126` but not yet in `main` in this local clone.
- The Bonvoice work lives on `VS-647-bonvoice-integration` (also in two dev branches). It is tested on the dev environment. It is not merged to `main` and not live in production.
- One feature (reconciliation of zero-duration calls, June 2026) exists only on side branches. It is marked below.

---

### Quick map of features

| # | Feature | When | Main area |
|---|---|---|---|
| 1 | Fair agent assignment: daily reset of counts, singleton, cron, Slack alert | Dec 2025, Mar 2026 | call-center/exotel |
| 2 | Slack critical-alert module (first version) | Dec 2025 | core/utils |
| 3 | Auto-generated Swagger / OpenAPI docs | Dec 2025 | core/swagger |
| 4 | Restricted chat config + chat notes privacy fix | Dec 2025 to Jan 2026 | core chat |
| 5 | Scheduling: slot confirmation rules, schedule call list, ICS | Jan to Feb 2026 | core slotConfirmation |
| 6 | Recent activity + performance APIs (mobile home screen) | Jan to Feb 2026 | features/recent-activity |
| 7 | Documentation-service tickets (CRUD) | Feb 2026 | features/documentation-service |
| 8 | Case management: case confirmation, participants, proceedings, case-transfer list | Jan to Feb 2026 | features/case-management |
| 9 | Document upload type check, owner scoping, safe access grant | Feb and Aug 2026 | features/document-management |
| 10 | Agent search shows only socket-connected agents | Mar 2026 | call-center/agent |
| 11 | Call log search rewritten in raw SQL + filters + export data | Mar to Sep 2026 | call-center/call |
| 12 | Call-log stats API | Mar to Aug 2026 | call-center/call |
| 13 | Disposition fixes: one per call, right agent, wrong-agent guard | Mar to Apr 2026 | call-center/disposition, exotel |
| 14 | DND error + 10-minute call limit fix | Apr 2026 | call-center/exotel |
| 15 | AI-call intake: receive, broadcast, claim | May to Jun 2026 | call-center/ai-call |
| 16 | Call status rework (v2 and v3), stale-call cleanup, cron routes, refresh button | May to Oct 2026 | call-center/exotel, call |
| 17 | Stuck-call disposition backfill | Aug 2026 | call-center/exotel |
| 18 | Callback claim route + claim dispositions | Aug and Oct 2026 | call-center/call |
| 19 | Work Queue V2 (my dispositions, my tasks, stats, priority) | Jul 2026 | call-center/disposition |
| 20 | Follow-up rules, new categories, dispositions without a call | Aug to Oct 2026 | call-center/disposition |
| 21 | Blocked numbers (inbound deny list) | Sep 2026 | call-center/exotel |
| 22 | DTMF priority routing with spillover | Aug to Oct 2026 | call-center/exotel |
| 23 | Call notes: edit/delete only by author within 24 hours | Oct 2026 | call-center/call-notes |
| 24 | Small config and data features | Aug to Oct 2026 | mixed |
| 25 | Bonvoice: second telephony provider | Oct to Nov 2026 | call-center/bonvoice, telephony |

---

### 1. Fair agent assignment: reset of assignment counts, singleton, cron, Slack alert

**When:** Dec 2025, then Mar 2026.

**Problem.** Inbound calls go to the agent with the fewest calls so far (least-assigned). The counts live in memory inside `ExotelWebhookService`. Two problems:
1. The counts never reset. An agent who worked yesterday looked "full" today.
2. The code did `new ExotelWebhookService()` in several places. Each object had its own counts map, so counting was not shared.

**What I built.**
- A reset function and an API route to trigger it by hand.
- Made the service a **singleton** (`private constructor` + `static getInstance()`), so all controllers share one object and one counts map.
- A **node-cron** job inside the constructor: every day at 6:00 AM (timezone from `CRON_TIMEZONE`, default `Asia/Kolkata`) it clears the counts.
- A Slack "low" alert when the cron starts and after each reset. The alert shows counts before and after, so we can see it really ran.

**How it works.**
1. `app.ts` calls `ExotelWebhookService.getInstance()` at startup. This creates the one object and schedules the cron.
2. At 6 AM the cron calls `resetAgentAssignmentCounts()`. It copies the map (for the alert), calls `.clear()`, logs, and sends a Slack alert.
3. Manual path: `POST /api/v1/call-center/exotel/reset-agent-counts` -> `ExotelController.resetAgentCounts` -> `resetAssignmentCountsAPI()` -> `{ success, message }`. The route is behind `authenticateToken` in the current code.

**Key files.** `src/features/call-center/exotel/exotel-webhook.service.ts` (`getInstance`, `resetAgentAssignmentCounts`, `resetAssignmentCountsAPI`), `exotel.controller.ts`, `exotel.routes.ts`, `exotel-webhook.controller.ts`, `src/app.ts`.

**DB.** None. Counts are in memory.

**Packages.** `node-cron` (schedule), Slack alert helper.

**Tricky parts / decisions.**
- In-memory state only works if there is one object. The singleton fixed that.
- In-memory state also only works with one running copy of the service. With more copies each has its own counts, and a cron runs once per copy. The repo notes now say cron jobs should take a Redis lock. My job did not have that lock. This is a good "what I would improve" answer.

**Interview questions.**
- *What is a singleton and why did you need it?* One shared object for the whole app. The counts map is in memory, so every caller must use the same object.
- *What happens to in-memory counts on restart or with two pods?* They are lost on restart and not shared between pods. For many pods I would move them to Redis.
- *How does node-cron work?* You give a cron pattern like `0 6 * * *` and a function. It runs inside the Node process on that schedule. A timezone option controls what "6 AM" means.
- *Why send an alert when nothing went wrong?* A silent cron is hard to trust. A low-priority message proves it ran and shows the numbers.

---

### 2. Slack critical-alert module (first version)

**When:** Dec 2025.

**Problem.** Errors only went to log files. The team wanted important problems in Slack, with a severity level and an optional file attached (for example a CSV of bad rows).

**What I built.** `src/core/utils/critical-alert.ts` (about 1000 lines) with a small public API: `alert.high()`, `alert.medium()`, `alert.low()`.

**How it works.**
1. A `ConfigStore` singleton holds the Slack client (`WebClient` from `@slack/web-api`), default channel, and a temp folder.
2. `alert.high/medium/low(error, options)` does not call Slack directly. It **emits an event** on a Node `EventEmitter`. The caller returns at once and is never blocked.
3. A listener picks the event and calls `SlackNotifier.sendToSlack()`. It builds Slack "blocks": header with emoji and colour per severity, the error message (cut to 1000 characters), the stack trace, and an optional comment.
4. If `fileData` is passed, a `FileCreator` writes a temp file as txt, json or csv (it can also convert between formats), uploads it, then deletes it. Old temp files are cleaned up after 24 hours.

**Packages.** `@slack/web-api`; Node `events`, `fs`, `path`, `crypto` (random file names).

**Decisions and an honest lesson.**
- Event-based so that an alert failure can never break the business code.
- This first version was later replaced by a team-wide alert kernel in `src/core/alerts/` (with dedupe keys and channel routing). My later code uses that newer API (`alerts.low(..., { channelKey, dedupeKey })`).
- Lesson: the first version read its Slack settings from code instead of environment variables. Secrets must only live in env / secret stores. This was fixed later in the repo. Be ready to say this plainly if asked.

**Interview questions.**
- *Why EventEmitter and not a direct call?* To decouple. The caller fires and forgets. Slack being slow or down cannot slow the request.
- *How do you stop alert spam?* The first version did not. The newer kernel uses a dedupe key and a time window, for example one alert per hour per topic.
- *Where should a Slack token live?* In an environment variable or a secret manager. Never in source code or git history.

---

### 3. Auto-generated Swagger / OpenAPI documentation

**When:** Dec 2025.

**Problem.** The service had many routes and no API docs. Writing Swagger by hand for each route would go stale fast.

**What I built.** A generator that walks the live Express router tree and builds an OpenAPI spec at request time. Served at `/api-docs` (Swagger UI) and `/api-docs.json` (raw spec).

**How it works.**
1. In `app.ts` and `core/routes/index.ts` I replaced `app.use(path, router)` with a small `mount(path, router)` helper. It sets `router.__swaggerMountPath = path` and then mounts as normal. Reason: in Express 5 it is hard to read the mount path back from the internal regex.
2. The validation middlewares were changed to **tag themselves**. `validateDto(DtoClass, 'body'|'query'|'params')` attaches `__swagger = { kind, dtoClass }`. `validateRequest(joiSchema, ...)` attaches `__swagger = { kind, joiDescribe: schema.describe() }`.
3. `generateOpenApiSpec(app)` in `src/core/swagger/openapi.ts` walks every layer, joins mount paths, converts `:id` to `{id}`, and collects the handlers of each route.
4. For each route it reads the tags on the handlers:
   - class-validator DTOs become JSON schema using `validationMetadatasToSchemas` from `class-validator-jsonschema`
   - Joi schemas are converted by my own function `joiDescribeToOpenApiSchema`
5. It detects auth by handler name (`authenticateToken`) and roles (`requireLawyer`, `requireClient`, `authorizeCallCenter`) and adds a bearer security scheme.
6. Tags (groups in the UI) come from the path. Call-center routes are grouped by sub-module.

**Key files.** `src/core/swagger/openapi.ts`, `src/core/swagger/swagger.ts`, `src/core/middleware/validation.middleware.ts`, `src/app.ts`, `src/core/routes/index.ts`, `src/features/call-center/index.ts`.

**Packages.** `swagger-ui-express`, `class-validator-jsonschema`, `class-validator`, `joi`.

**Same commit.** I also made New Relic start-up safe: it loads only if `NEW_RELIC_LICENSE_KEY` is set and `NEW_RELIC_ENABLED` is not `false`, inside a try/catch, so local runs do not crash.

**Tricky parts.** Two validation libraries (Joi and class-validator) in one codebase. Express 5 internals. The docs can never drift, because they are built from the same validators that run on each request.

**Interview questions.**
- *Why generate docs from code?* Hand-written docs go out of date. Here the validator is the single source of truth.
- *How do you get the request body schema?* The validation middleware carries its DTO class or Joi description as a property. The generator reads it.
- *What is OpenAPI?* A standard JSON/YAML format that describes endpoints, parameters, bodies and auth. Swagger UI renders it.
- *Downside of this approach?* Response shapes are not known from validators, so responses are generic. Also the spec is built on every request; it could be cached.

---

### 4. Restricted chat config and chat notes privacy fix

**When:** Dec 2025 to Jan 2026.

**Restricted chat config.**
- Problem: in a restricted chat the app shows a row of action cards. The app should not hard-code them.
- Built: `GET /api/v1/chats/restricted-chat-config` -> `chatController.getRestrictedChatConfig` -> `chatService.getRestrictedChatConfig()`. It returns a list of cards. Each card has `id`, `type` (`uploadDocument`, `scheduleCall`, `text`), `icon` URL, `title`, `backgroundColor`, `textColor`. The four cards: Upload Documents, Schedule Call, Request For Quotation, What's Next.
- This is "server-driven UI": change the backend list and the app changes without an app release. (Later the team added language support to it.)
- Files: `src/core/routes/chat.routes.ts`, `src/core/controllers/chat.controller.ts`, `src/core/services/chat.service.ts`.

**Chat notes bug.**
- Problem: chat notes are private notes a user writes inside a chat. The list API filtered by a `created_by` value sent by the client. So one user could see another user's notes.
- Fix: `getAll` now takes `userId` from the JWT (`req.userId`) and always sets `where.created_by = userId`. The chat list query in `chat.service.ts` also got `created_by: userId`.
- I first added the same owner check to get-one, update and delete, then (same day) kept it only on list and create, as the final commits show. The exact product reason for that rollback is not confirmed.
- Files: `src/core/controllers/chatNotes.controller.ts`, `src/core/services/chatNotes.service.ts`. Table: `chat_notes` (soft delete with `is_active`).

**Small related fix (Jan 2026).** The consultation screen footer showed the amount in paise. Fixed to rupees: `Math.round((amount + amount * taxRate) / 100)` in `src/core/services/consultation.service.ts`.

**Interview questions.**
- *What was the security bug?* Trusting a user id from the request. The fix is to always take identity from the verified token.
- *What is this bug class called?* IDOR / broken access control.
- *What is soft delete?* Set a flag like `is_active = false` instead of removing the row. Data stays for audit.
- *Why store money in paise?* Integers avoid floating-point rounding errors. Convert only when showing.

---

### 5. Scheduling: slot confirmation rules, schedule call list, ICS

**When:** Jan to Feb 2026.

**Background.** A lawyer and a client agree on a call time through a "slot confirmation". One row in `slot_confirmations` holds the whole negotiation. `data` is a JSON timeline (propose, agree, cancel, reschedule). `finalSlot` is the agreed slot. `status` is one of `none`, `lawyer_agree`, `client_agree`, `both_agree`, `scheduled`, `lawyer_cancel`, `client_cancel`, `update`.

**What I built.**

1. **Slot validation (Joi).** In `src/core/validation/slotConfirmation.validation.ts`:
   - `checkSlotOverlap(a, b)`: two slots overlap when `start1 < end2 && start2 < end1`. Two slots with the same start time also conflict.
   - `validateNoSlotConflicts` is a Joi `.custom()` rule on the `slots` array. It checks every pair and also checks that each slot ends after it starts. It returns a clear message like "Slot 1 overlaps with Slot 2".
2. **Service-side checks** in `newSlotConfirmationCreation` (`src/core/services/slotConfirmation.service.ts`):
   - all proposed slots must be in the future (else an error with the list of `pastSlots`)
   - slots already booked are returned as `conflictingSlots`
   - slots that clash with another in-progress negotiation are returned with what they conflict with
3. **Grouped list.** `getAll` now returns `{ cancelled, pending, scheduled }`. Scheduled items are kept only if the end time is today or later, and are sorted by time. Each item gets `lawyerDisplayName` (`VSL<id>`) and `clientDisplayName` (`VSCL<id>`), so real names stay hidden.
4. **Schedule call list API.** `GET /api/v1/slot-confirmations/schedule-call-list?date=` (JWT + Joi query check). `getScheduleCallList(userId, date)`:
   - loads all slot confirmations where the user is the lawyer or the client
   - `scheduledCalls`: status `scheduled` and `finalSlot` falls on that date
   - `pendingConfirmationCalls`: other open statuses where any proposed slot in the timeline falls on that date
   - `cancelledCalls`: cancel statuses where any slot falls on that date
   - helpers `extractSlotsFromData` and `isSlotOnDate` accept different field names (`startDateTime`, `start_date`, `startDate`)
5. **Reschedule / cancel flow fixes.** On reschedule or cancel, the old chat message is updated (`updatePreviousSlotConfirmationMessage`), and the new request is created on behalf of the other party with the right user type. Message texts were fixed so the proposer sees "You have proposed a call schedule".
6. **ICS call.** When a slot becomes agreed/scheduled, the service calls the document service `GET /download-ics` through `DocumentService.downloadICS()` with the slot start and end. It is inside try/catch, so a failure never blocks scheduling. Honest note: organizer and attendee values are placeholders in this code, so this is a first integration step, not a finished calendar invite.
7. **Consultation validation.** `createConsultationSchema` now requires `lawyer_id` and `client_id` as trimmed UUIDs with clear messages.

**Key files.** `src/core/services/slotConfirmation.service.ts`, `src/core/validation/slotConfirmation.validation.ts`, `src/core/controllers/slotConfirmation.controller.ts`, `src/core/routes/slotConfirmation.routes.ts`, `src/core/internalServices/documentService.ts`, `src/core/validation/consultation.validation.ts`.

**Packages.** `joi`, `moment-timezone`, Sequelize `Op.or`.

**Tricky parts.**
- Filtering by a date inside a JSON column is done in JavaScript after loading rows. Simple, but not good for very large data.
- The user lookup runs once per row (N+1 pattern). In later work (call-log search) I fixed the same pattern with batch lookups.

**Interview questions.**
- *How do you check that two time ranges overlap?* `startA < endB && startB < endA`.
- *Why validate both in Joi and in the service?* Joi checks the shape and rules inside one request. The service checks against the database (booked slots, other negotiations).
- *Why return grouped data?* The app has three tabs. Grouping on the server keeps the app simple and all clients consistent.
- *What is an N+1 query problem?* One query for the list, then one more per row. Fix with a single batched query (`IN (...)`).

---

### 6. Recent activity and performance APIs

**When:** Jan to Feb 2026.

**Problem.** The app home screen needs "what happened recently" and a small performance card for lawyers.

**What I built.** A feature module `src/features/recent-activity/` (routes, controller, service). All routes need a JWT.

- `GET .../schedule-requests`: reads `slot_confirmations` where the user is lawyer or client, newest first. Each item becomes a display card: `title`, `description`, `icon`, `backgroundColor`, `borderColor`, `chat_id`, `time`, `consultationId`. Colours depend on status (green accepted, red declined, yellow waiting).
- `GET .../case-acceptance-requests`: same idea on `case_confirmations` (only `isActive`). The description is read from the last item of the JSON timeline, from the part of the message written for this user.
- `GET .../performance?type=today|currentWeek|lastWeek|currentMonth`: builds the date range with `moment-timezone` (default `Asia/Kolkata`). It then:
  1. calls the finance service `getLawyerEarningsByDateRange(userId, start, end)` (failure gives 0, not an error)
  2. counts consultations in stage `COMPLETED` in that range
  3. returns two cards (earning, consultations), the date range, and the list of filter options

**Key files.** `src/features/recent-activity/*`, `src/core/internalServices/financeService.ts` (I added the earnings call).

**DB.** Read only: `slot_confirmations`, `case_confirmations`, `consultations`.

**Notes.** I first wrote it under `core/` and then moved it to its own feature folder. Translations were added later by the team.

**Interview questions.**
- *Why does the backend send colours and icons?* Server-driven UI. The look can change without an app release.
- *How do you compute "this week" for an Indian user on a UTC server?* Use a timezone library and compute start/end in the user's zone, then convert to real instants.
- *What if the finance service is down?* The call is in try/catch. The screen still loads and shows 0 earnings.

---

### 7. Documentation-service tickets

**When:** Feb 2026.

**Problem.** Clients can request paid document work (drafting and similar). Ops needs a ticket per request.

**What I built.** A new table/model and three APIs in `src/features/documentation-service/`.

- Model `documentation_services` (`src/database/models/documentation-service.model.ts`): `ticketID` (auto number), `clientID`, `agentID`, `lawyerID`, `serviceID`, `status` (enum), `description`, `metaData` (JSON), `priority` (enum), `updated_by`, timestamps.
- `POST /` create: client comes from the JWT; checks the user exists in user-service and the service exists; default status OPEN.
- `GET /` list: optional filters (agent, client, lawyer, ticket, service, status, priority, created/updated since). Joins the `Service` name. Newest first.
- `PATCH /:ticketID` update: checks ticket, updater, and any new agent/lawyer/service ids; only changes fields that were sent; records `updated_by`.
- DTOs with class-validator in `dto/`.

**Decision.** First version also fetched user details for every row. I removed that from the list API to keep it fast.

**Interview questions.**
- *How do you do a partial update safely?* Only apply fields that are not `undefined`, and validate every foreign id before saving.
- *Why validate ids against another service?* There is no DB foreign key across services, so the code must check.

---

### 8. Case management: case confirmation, participants, proceedings, case-transfer list

**When:** Jan to Feb 2026. File: `src/features/case-management/case/case.service.ts` (+ controller, routes, DTOs).

**a) Case confirmation logic.**
- Problem: one chat could end up with many active `case_details` rows.
- On a new case confirmation: load all case details of the chat ordered by `updated_at DESC`, keep the newest, and archive the rest (`is_active = false`, `case_status = ARCHIVED`).
- On decline (by lawyer or client): archive all case details of that chat, and reset the `lawyerAgree` / `clientAgree` flag so the state is clean.

**b) Add participants from chat.**
- Before: participant type was guessed from text in the chat user type.
- Now: `addParticipantsFromChat(caseId, chatId, userId, authorization)` calls user-service for each participant and stores real name, type and phone in `case_participants`. If the call fails it falls back to the chat data. Types are mapped to two values: lawyer/advocate -> `lawyer`, everything else -> `client`.

**c) Update proceedings.** `PATCH /api/v1/cases/proceedings` with `UpdateProceedingDto` (`id`, `nextHearingDate`, `nextHearingPurpose` max 500 chars, `metadata` object).
- Flow: find the proceeding (404 if missing) -> next hearing date must be in the future -> **merge** new metadata into the old (`{ ...old, ...new }`) -> update only the sent fields.
- Model change: enabled the `metadata` JSON field on `case_proceedings`.

**d) Case-transfer list.** `GET /api/v1/cases/case-transfer-list`. Loads active case confirmations where the user is lawyer or client. Groups into `accepted` (`case_confirm`), `cancelled`, `pending` (all other). Each item gets display names (`VSL...`, `VSCL...`) and the case title, description and metadata.

**Interview questions.**
- *Why merge metadata instead of replacing it?* A partial update must not delete keys the client did not send.
- *How did you make sure only one case is active per chat?* Sort by latest update, keep the first, archive the rest in one `UPDATE ... WHERE case_id IN (...)`.
- *Why fall back to chat data if user-service fails?* Creating the case is more important than perfect participant data.

---

### 9. Documents: file-type check, owner scoping, safe access grant

**When:** Feb 2026 and Aug 2026. File: `src/features/document-management/document/document.service.ts`.

- **Allowed file types (Feb).** The allow-list is now PDF, DOC/DOCX, JPG/JPEG, PNG, XLS/XLSX, CSV. I removed plain text and GIF, added CSV, and added common alias MIME types that some phones send. The error message lists the allowed types. Max size stays 100 MB.
- **Owner scoping (Aug).** Problem: listing documents without a `caseId` returned the whole table. Fix: admins (or users with the document-view permission) see all; a normal user with a `caseId` must be a participant of that case; a normal user without a `caseId` sees only `owner_id = userId`. Owner lookups skip ids that are not UUIDs and skip the call if the list is empty.
- **Idempotent access grant (Aug).** When an upload is confirmed, the other party of the case gets VIEW access. Old code did "find, then create". Two fast requests could both create. New code uses `DocumentAccess.findOrCreate` and relies on the unique index on (`document_id`, `user_id`). Any error is logged and ignored so the upload confirmation never fails.

**Packages.** `mime-types`, `uuid` (`validate`), `lodash`.

**Interview questions.**
- *Why an allow-list and not a block-list?* Unknown types are rejected by default. Safer.
- *Is checking the MIME type enough?* No. The client can lie. Stronger checks read the file's first bytes or scan the file.
- *What is a race condition in find-then-create?* Two requests both see "not found" and both insert. A unique index plus `findOrCreate` (and catching the duplicate error) fixes it.

---

### 10. Agent search shows only socket-connected agents

**When:** Mar 2026. File: `src/features/call-center/agent/agent.service.ts`.

**Problem.** An agent could be `available` in the database but have closed the browser. Calls sent to that agent were lost ("ghost agent").

**What I built.** In agent search each agent now gets `is_socket_connected` from `CallCenterSocketServiceCore.isAgentConnected(agent.id)`. The result is filtered to only connected agents. I also added a separate result type for deleted agents.

**Decision.** I first applied the same check inside `findAvailableAgent()` (the routing path), then reverted that the same day. So the socket filter stays in search only. The exact reason is not recorded in the commit.

**Interview questions.**
- *Why can DB status and real status differ?* The DB changes only when the app tells it. A browser crash sends nothing. The socket connection is the live truth.
- *How do you know if a user is connected in Socket.IO?* Each agent joins a room. If the room has at least one socket, the agent is connected.

---

### 11. Call-log search: raw SQL, filters, AI pool, export data

**When:** Mar to Sep 2026. Files: `src/features/call-center/call/call.service.ts` (`searchCalls`, `resolveUsersByMobile`), `dto/search-calls.dto.ts`, `src/core/internalServices/userService.ts`.

**Problem.** The call-logs page was slow. Sequelize `findAndCountAll` with includes made heavy queries, and extra lookups ran one after another.

**How it works now (step by step).**
1. **Build WHERE safely.** A list of SQL pieces plus a `replacements` object (named parameters like `:agent_id`). No user value is glued into the SQL string.
2. **Sort safety.** `sort_by` must be in an allow-list of column names; `sort_order` can only be ASC or DESC. (Column names cannot be bound as parameters, so an allow-list is the safe way.)
3. **Two queries in parallel** with `Promise.all`:
   - `SELECT COUNT(DISTINCT cl.id)` with the smallest joins possible
   - `SELECT cl.*, agent columns, consultation columns ... LEFT JOIN ... ORDER BY ... LIMIT :limit OFFSET :offset` with `nest: true`, so `'agent.id'` becomes `agent: { id }`
4. **Clean-up of raw rows.** Raw SQL returns JSON columns as strings, so I parse `call_metadata`. Empty joined objects become `null`. Date strings become `Date` objects.
5. **Batch extras, never per row:**
   - dispositions: one `IN (callIds)` query, only when asked (`include_disposition`) or when a disposition filter is used
   - caller city/state (`include_location`): one query on `cc_call_facts`
   - notes (`include_notes`): one query on `call_notes` by phone number; many notes for one number are joined into one cell
   - user names: `resolveUsersByMobile` sends numbers to user-service in chunks of 500, 4 chunks at a time, using a "minimal" mode that returns only id/name/email/number. A failed chunk is skipped; the list still loads.
6. **Filters added over time:**
   - date range defaults to today in Indian time
   - `include_missed` (also show calls with no agent)
   - disposition category / sub-category through `EXISTS (SELECT 1 FROM call_dispositions ...)`
   - `call_status` can be a comma list
   - virtual call type `consultation` (any call linked to a consultation)
   - search also matches an exact call id
7. **AI lead pool merged into the same API** (May 2026): `only_unclaimed_ai` returns only unclaimed AI calls. `include_unclaimed_ai` returns `(normal filters) OR (AI call, completed, unclaimed or claimed by me)`. So the frontend calls one API instead of two.
8. **Extra row fields:** `outcome` (plain-words explanation, see feature 16), `participants` (client only / lawyer only / both), `customer_user_id`, `lawyer_user_id`.

**Related model change (Aug 2026).** `call_logs` got `customer_user_id` and `lawyer_user_id`. Sequelize hooks `beforeCreate` / `beforeUpdate` (`resolveParticipantUserIds`) look up the numeric user id from the phone number once, when the row is written. It is best effort and never blocks the write. The list then reads the stored columns instead of calling user-service again and again.

**Export.** The admin panel's CSV export pages through this same API (limit raised to 2000 per page). The flags above add disposition, notes and location columns.

**Tricky parts.**
- Raw SQL is faster, but you lose model features (JSON parsing, dates, nesting), so you must rebuild them.
- A real bug: the name lookup sent thousands of numbers in one request to an endpoint that also loaded roles and permissions. It took most of a minute. Chunking plus the minimal mode fixed it.
- Filtering through `EXISTS` avoids duplicate rows that a JOIN would create.

**Interview questions.**
- *When do you choose raw SQL over an ORM?* For hot list/report queries where you need exact joins and counts. Keep the ORM for simple CRUD.
- *How do you prevent SQL injection in raw queries?* Named replacements for values. Allow-lists for things that cannot be parameters (column names, sort order).
- *Why split count and data queries?* The count does not need the wide joins. Running both in parallel cuts the wait.
- *What is the N+1 problem and how did you avoid it?* One query per row. I collect ids and do one `IN (...)` query per extra.
- *Why chunk external calls?* Small requests answer fast and do not hit timeouts. Limited concurrency avoids overloading the other service.

---

### 12. Call-log stats API

**When:** Mar to Aug 2026. `GET /api/v1/call-center/call-logs/stats` -> `CallController.getCallLogStats` -> `CallService.getCallLogStats`. DTO: `dto/call-stats.dto.ts`.

**Problem.** The dashboard header needs totals per status for the current filters. The numbers must match the list under it.

**How it works.**
1. Same filters as the list (agent, customer, type, status list, search, disposition filter via `EXISTS`, date range default today in Indian time).
2. Three queries in parallel:
   - `GROUP BY call_status` with `COUNT(id)`: one query gives every status count
   - `AVG` and `SUM` of `duration_seconds` only where duration > 0
   - unique and new phone numbers under the same filters
3. Every known status starts at 0, so the response shape is always complete.
4. Response: `total_calls`, one field per status, `average_duration`, `total_duration`, number counts, `status_breakdown`, and the applied `date_range` and `filters`.

**Decisions.**
- Earlier "missed" also included failed and busy. I split them so each number can be checked against the list.
- One grouped query instead of many `COUNT` queries.
- Deleted agents' calls are still counted (SQL condition fix in Apr 2026).

**Interview questions.**
- *How do you get counts for 10 statuses in one query?* `GROUP BY status` with `COUNT(*)`.
- *Why zero-fill?* `GROUP BY` returns no row for a status with zero calls. The frontend should not handle missing keys.
- *Why average only where duration > 0?* Missed calls have 0 seconds and would pull the average down.

---

### 13. Disposition fixes: one per call, right agent

**When:** Mar to Apr 2026.

**What a disposition is.** After each call the agent records the outcome: a category, a sub-category and notes. The system also creates one automatically when a call ends.

**a) One disposition per call (Mar).**
- Problem: creating a second disposition for the same call hit the DB unique index and returned a generic 500.
- Fix in `DispositionService.create`: check first and throw `ConflictError` (HTTP 409) with a clear message. Also catch Sequelize `UniqueConstraintError` and turn it into 409, for the case where two requests race. The controller now maps every `ApiError` to its own status code.

**b) Wrong agent on auto-dispositions (Apr).**
- Problem: when the first agent does not answer, the telephony provider tries the next agent ("call hopping"). An "ended" event comes for each attempt. The auto-disposition was created for an agent who never handled the call.
- Fixes in `exotel-webhook.service.ts`:
  1. **Guard.** If the event's agent is not the agent on the call log, skip the disposition. Record the skip in `call_metadata.skipped_dispositions` and send a medium alert.
  2. **Retry tracking.** Each attempt is appended to `call_metadata.retry_attempts` (agent, outcome, time).
  3. **Correction.** If the call log has the wrong agent at the end, fix `call_logs.agent_id` and also fix the auto-created disposition (`is_manual_entry = false` only). The disposition metadata keeps the original agent, the corrected agent and the reason. A high alert is sent because earlier guards missed it.

**Tables.** `call_dispositions` (unique index on `call_log_id`), `call_logs.call_metadata` (JSON).

**Interview questions.**
- *Why both a pre-check and a catch of the unique error?* The pre-check gives a clean message in the normal case. The DB constraint is the real guarantee when two requests race.
- *Why 409?* The request is valid but conflicts with the current state.
- *How did you make this debuggable?* Every skip and correction is written into the row's metadata and sent to Slack.

---

### 14. DND error and the 10-minute call limit fix

**When:** Apr 2026.

- **DND.** In India a number can be on the Do-Not-Disturb registry. The provider then refuses the call with HTTP 403 and a specific error code. Before, the agent saw a generic failure. I added `DndError` (403, code `DND_BLOCKED`) in `src/core/utils/ApiError.ts`. The provider client detects that case (by code or by words in the description) and throws it. The call controller returns `{ success: false, message, errorCode: 'DND_BLOCKED' }`, so the UI can show the real reason.
- **Standard error body.** Same commit added `InsufficientBalanceError` (402, carries wallet details) and `formatErrorBody()`, so all error paths return the same shape: `{ success: false, message, error: { code }, data? }`.
- **10-minute limit.** Paid consultations have a max call length. The commit is named "fixed 10 minute calling". The code change is that the limit field type went from number to string in three files, to match what the provider API expects (the exact failure seen before the fix is not confirmed from the diff). Files: (`exotel-api.service.ts`, `exotel-integration.service.ts`, `consultation-integration.service.ts`).

**Interview questions.**
- *Why custom error classes?* One place decides status code and machine-readable code. Controllers stay simple and the frontend can branch on `code`.
- *What was the 10-minute fix?* The time-limit field had the wrong type for the provider API. I changed it to a string in the three places that pass it along. A type mismatch like this can fail silently, so always compare what you send with the provider's documentation.

---

### 15. AI-call intake: receive, broadcast, claim

**When:** May to Jun 2026. Folder: `src/features/call-center/ai-call/`.

**Problem.** A separate AI voice system answers some calls and collects details (case type, city, urgency, summary). Human agents must see these leads at once, and exactly one agent should take each lead.

**APIs (all under `/api/v1/call-center/ai-calls`, JWT).**
- `POST /completed`: the AI system posts a finished call.
- `POST /:callLogId/claim`: an agent claims the lead.
- `GET /unclaimed`: paginated pool of unclaimed leads.

**How `POST /completed` works.**
1. `validateDto(AiCallCompletedDto)`: `call_sid`, `customer_number`, `call_status` must be `completed`, ISO dates, `duration >= 0`, optional `recording_url`, and a nested `ai_intake` object checked with `@ValidateNested` + `@Type` (case type, caller name, summary, lead status, lead priority, optional city/state/language/tags/urgency).
2. **Idempotency:** if a call log with this `call_sid` already exists -> 409.
3. Create a `call_logs` row: `agent_type = 'ai'`, `agent_id = NULL`, status `completed`, and `call_metadata.ai_intake` holding the AI data.
4. **Broadcast** over Socket.IO: event `ai_call_available` to every available agent, plus a manager notice. Returns how many agents were notified.

**How claim works (the key part).**
1. Check: call exists, is an AI call, agent exists and is `available`. If the same agent claims again, return success (idempotent).
2. **Atomic claim:** `UPDATE call_logs SET agent_id = :me, call_metadata = JSON_SET(...claimed_at, claimed_by...) WHERE id = :id AND agent_id IS NULL AND agent_type = 'ai'`.
3. If affected rows = 0, someone else won -> 409.
4. Broadcast `ai_call_claimed` so the card disappears for everyone.
5. Since Oct 2026: also create a pending disposition owned by the claimer (see feature 18).

**DB.** New column `call_logs.agent_type ENUM('human','ai')`, default `human`.

**Docs.** I wrote an integration guide and an implementation plan under `docs/`. I later relaxed some DTO fields so the AI team's real payloads pass.

**Interview questions.**
- *Two agents click claim at the same time. How does only one win?* A conditional UPDATE with `WHERE agent_id IS NULL`. The database runs it atomically. I check affected rows.
- *Why not SELECT then UPDATE?* Both could read "free" before either writes. That is a race.
- *What is idempotency here?* The same `call_sid` posted twice does not create two rows. The same agent claiming twice gets success.
- *How do you validate nested JSON with class-validator?* `@ValidateNested()` plus `@Type(() => ChildDto)` from class-transformer.
- *What if no agent is online?* Nothing is lost. The lead stays in the unclaimed pool and shows in the list API.

---

### 16. Call status rework (v2, v3), stale-call cleanup, cron routes, refresh button

**When:** May to Oct 2026. This is my largest piece of work in the call-center. (The telephony webhooks themselves are covered in the Exotel section by another document. Here I only explain status logic and clean-up.)

**Problem.**
- Old statuses were too few. "Completed" was used for almost everything. A call with 0 seconds could show as completed.
- The provider uses many different words for how a call ended (three API generations).
- Sometimes the final webhook never arrives. The row stays "ringing" or "answered" forever and the agent stays stuck on that call.
- Ops kept asking: "what does cancelled mean on this call?"

**Step 1: new statuses (May 2026).**
- Migration `migrations/20260527120000-extend-call-status-enum.js`: adds `queued`, `missed`, `short_call` to the `call_logs.call_status` ENUM and moves old `no_answer` rows to `missed`. It has a full `down` path.
- `dto/initiate-call.dto.ts`: the `CallStatus` enum plus shared groups: `TERMINAL_STATUSES`, `CALL_COMPLETED_STATUSES`, `CALL_MISSED_STATUSES`, `CALL_FAILED_STATUSES`. All services now use these groups instead of hand-written lists.
- Everything was behind an env flag `CALL_STATUS_V2`, so it could be turned on safely.

**Step 2: one resolver function.** `resolveCallStatus({ exotelStatus, duration, callDirection, agentId, currentDbStatus })` in `exotel-webhook.service.ts` is the single place that maps provider words to our status. The rules:
- talk time > 30 s -> `completed`; 1 to 30 s -> `short_call`; "completed" with 0 s -> `missed`
- empty word: use duration if there is any; a ringing call that ended -> `missed`; otherwise `failed`
- a `queued` call that ends with no talk -> `failed` (no agent phone ever rang, so "missed" would blame an agent wrongly)
- "from leg unanswered" depends on direction: outbound means the agent did not pick up (`no_answer`); inbound means the caller left
- cancel words: if there was talk time, treat as completed/short; else `cancelled`
- no-dial / failed words -> `failed`; busy words -> `busy`
- unknown word -> `failed` plus a low Slack alert, deduped per word for a day

Another rule: the per-attempt "terminal" event must not write the final status any more. Only the single final event does.

**Step 3: verify with the provider's API.** `verifyCallStatusViaApi()` runs in the background after the final webhook (`setImmediate`, never throws). It fetches the call details from the provider API, runs the same resolver, and corrects status / duration / recording if the API knows better. It stores `api_verified_at`, `api_raw_status`, `api_initial_status`, `api_resolved_status` in `call_metadata`.

**Step 4: stale-call cleanup ("v3", Oct 2026).**
- First version (May/Jun): a 5-minute in-process cron that guessed (queued > 20 min -> missed, initiated > 20 min -> failed).
- Then I moved the trigger out of the process to an HTTP route called by a Jenkins job: `POST /api/v1/call-center/exotel/webhook/internal/cleanup-stale-calls`. An external scheduler runs once, however many service copies exist.
- Final version: **never guess, ask the provider.**
  - `cleanupStaleCalls({ statuses, lookbackHours, triggeredBy })` selects rows still live (`answered`, `ringing`, `queued`, `initiated`), older than 20 minutes, inside a look-back window (default 24 h).
  - For each row it calls `reconcileCallFromExotel(callLog, { triggeredBy, actor })`, which returns one of:
    - `still_ongoing`: the provider says the call is still active -> write nothing
    - `api_error` / `no_call_sid`: write nothing, try next time
    - `unchanged`: call ended but nothing new learned -> write nothing
    - `resolved`: run `resolveCallStatus`, write status, duration, recording and end time, add a `status_reconciliation` marker (who, when, from, to, why) and a `callEvents` entry
  - After `resolved`: release the agent if still tied to this call (`releaseAgentClaim`), and create the auto-disposition that the lost webhook would have created.
  - One Slack summary per run, deduped per hour.
- **Refresh button:** `POST /api/v1/call-center/calls/:callId/reconcile-status` runs the same function for one call with `triggeredBy: 'manual'` and the acting agent's code. The response includes the agent's name so the UI row can be replaced in place.

**Step 5: plain-words explanation.** `src/features/call-center/call/derive/call-outcome.ts` exports `deriveCallOutcome(row)`. It is a pure function (no I/O, never throws). It returns:
- `status_title` and `status_meaning`: what the status means in general
- `reason_code` and `reason_text`: what happened on this call, with real roles ("Agent hung up before the client picked up", "The agent's phone rang for about 32 seconds and nobody answered")
- `caller_role`, `receiver_role`, `decided_by` (webhook, API check, cron, manual, blocked, pending), `can_refresh`

The search API attaches this as `outcome` on each row, so every screen shows the same text.

**Other small fixes in this area.**
- recording URL validator accepts null/empty and any http(s) URL (the old `isUrl` check rejected empty strings)
- linking a consultation to a call no longer forces the call status to `completed` (Jul 2026)
- provider timestamps without a zone are parsed as Indian time (`parseExotelTimestamp`)

**Key files.** `exotel-webhook.service.ts` (`resolveCallStatus`, `verifyCallStatusViaApi`, `reconcileCallFromExotel`, `cleanupStaleCalls`), `exotel.controller.ts`, `exotel.routes.ts`, `call/derive/call-outcome.ts`, `call/call.controller.ts` (`reconcileCallStatus`), `call/dto/initiate-call.dto.ts`, `database/models/call-log.model.ts`, plan docs in `docs/call-status-rework*/`.

**Side-branch work (not in main).** Jun 2026: `call-sync-reconciliation.service.ts`, a service to backfill calls saved with 0 duration (fetch real duration and recording, fix the disposition and its follow-up tasks) with a route `POST .../exotel/sync/reconcile`. It exists only on side branches. Its idea was later covered by the API verification and cleanup above.

**Tricky parts and decisions.**
- **Never guess.** The first cron guessed and sometimes closed real long calls. The final one writes only what the provider confirms.
- **Idempotent and safe to re-run.** Running the job twice changes nothing the second time.
- **Audit trail.** Every automatic change records its source, so anyone can see "the cron closed this, not a webhook".
- **External cron vs in-process cron.** In-process crons run once per service copy.
- **Feature flag** for a risky change to a core field.
- **Words matter.** `queued` + no talk became `failed`, not `missed`, because "missed" counts against agents.

**Interview questions.**
- *What do you do when a webhook never arrives?* Do not trust webhooks alone. Run a periodic reconcile job that asks the provider's API for the truth, and give users a manual refresh that uses the same code.
- *Why keep the status mapping in one function?* Webhook, API check, cron and button must all agree. One function means one set of rules.
- *How do you change an ENUM column safely in MySQL?* `ALTER TABLE ... MODIFY COLUMN` with old values plus new ones (adding at the end is cheap), migrate data, keep a `down`. Hide the new behaviour behind a flag.
- *Why is `deriveCallOutcome` a pure function?* Easy to test, cannot break the API, and every screen gets the same answer.
- *What does idempotent mean for a cron job?* Running it again gives the same end state. Here: only live rows are picked, and only confirmed facts are written.
- *Why release the agent in the cleanup?* A lost webhook also leaves the agent marked "on call". They would get no new calls until someone fixes it.

---

### 17. Stuck-call disposition backfill

**When:** Aug 2026 (updated Oct 2026). `backfillStuckCallDispositions()` in `exotel-webhook.service.ts`. Route: `POST /api/v1/call-center/exotel/webhook/internal/backfill-stuck-call-dispositions` (called by Jenkins).

**Problem.** If the final event is lost, the auto-disposition is never created. The agent then has nothing in the work queue for that caller, and the lead is forgotten.

**How it works.**
1. Select inbound calls that have an agent, have **no** disposition (`NOT EXISTS` sub-query), are older than 16 minutes and newer than 24 hours, and are in `initiated`, `ringing` or any terminal status.
2. For each: reuse the normal guard `shouldCreateAutoDisposition` and the normal creator `createAutoDisposition` with duration 0. The result is a "no answer" disposition owned by the assigned agent.
3. It never changes `call_status`. That is the cleanup job's work.
4. Idempotent: the unique index on `call_log_id` and the guard mean one disposition per call, however many times it runs.
5. Calls without an agent are left out on purpose. They get a disposition when someone claims them (feature 18).

**Interview question.** *Why 16 minutes?* To be sure the call is not still ringing for real. A fixed and safe age is better than touching live calls.

---

### 18. Callback claim route and claim dispositions

**When:** Aug 2026, extended Oct 2026.

**Problem.** Some inbound calls end with no agent (all busy). They appear as "needs callback". Two agents would call the same client back because nobody knew who had picked it.

**What I built.** `POST /api/v1/call-center/calls/:callId/claim` -> `CallController.claimCallback` -> `CallService.claimCallbackCall(callLogId, agentId, agentCode)`.

**How it works.**
1. Find the agent row for the logged-in user (403 `NOT_AN_AGENT` if none).
2. If the call already has an agent -> 409. If this agent already holds the claim -> success. If another agent holds it -> 409 with that agent's code.
3. **Atomic claim:** `UPDATE call_logs SET call_metadata = JSON_SET(COALESCE(call_metadata,'{}'), '$.callback_claim', CAST('<json>' AS JSON)) WHERE id = ? AND agent_id IS NULL AND JSON_EXTRACT(call_metadata, '$.callback_claim') IS NULL`.
4. 0 rows affected -> re-read and report who won (409).
5. Then `createClaimDisposition(callLog, agentId, 'callback')` creates a pending disposition owned by the claimer, so the call appears in their work queue. Category `callback_claimed`; the sub-category depends on the call status at claim time (claimed missed / queued / cancelled / failed / initiated call). AI claims use `ai_call_claimed`.

**Decisions.**
- The claim does **not** set `agent_id`. It is only a marker in metadata. So stats for "unassigned calls" stay true.
- `createClaimDisposition` never throws. A claim must not fail because the disposition could not be written. An existing disposition (409) is treated as fine.
- Route param is named `:callId` to match the sibling route. The API gateway's router rejects two different param names at the same path position. I renamed it in a follow-up commit the same day.

**Key files.** `call/call.service.ts`, `call/call.controller.ts`, `call/call.routes.ts`, `disposition/services/claim-disposition.service.ts`, `disposition/config/disposition-categories.config.ts`.

**Interview questions.**
- *How do you update one key inside a JSON column atomically in MySQL?* `JSON_SET` in the UPDATE and `JSON_EXTRACT(...) IS NULL` in the WHERE. Check and write happen in one statement.
- *Why a metadata marker and not `agent_id`?* Assignment has meaning for stats and routing. I did not want to change that meaning for a UI need.

---

### 19. Work Queue V2 (backend-driven)

**When:** Jul 2026. Files: `src/features/call-center/disposition/services/work-queue-v2.service.ts`, `work-queue-v2.helpers.ts`, `work-queue-v2.controller.ts`, `dto/work-queue-v2-query.dto.ts`, `dto/work-queue-stats-v2-query.dto.ts`, `dto/update-disposition-priority.dto.ts`, plan in `docs/work-queue-backend-driven/PLAN.md`.

**Problem.** The agent's work queue (pending dispositions and follow-up tasks) loaded everything and filtered in the browser. It was slow, and tab badges did not match the lists.

**APIs (under `/api/v1/call-center/dispositions`).**
- `GET /tasks/v2/my-dispositions`: pending dispositions of the agent
- `GET /tasks/v2/my-tasks?bucket=pending|overdue|in_progress|completed|all`
- `GET /tasks/v2/stats`: all tab counts and summary cards
- `PATCH /:id/priority`: set or clear a manual priority on a disposition

All query strings are checked with class-validator DTOs through `validateDto(..., 'query')`.

**How it works.**
1. **Pagination:** `normalisePagination` (default 20, max 100; `offset` wins over `page`). The response has `total`, `page`, `totalPages`, `hasMore`.
2. **Buckets are defined once.** `bucketWhere()` returns the Sequelize condition. `bucketSql()` returns the same rule as SQL text for the stats. Rules: pending = status pending and due date not passed; overdue = pending/overdue and due date passed; in progress = all in-progress tasks; completed = completed.
3. **Filters:** priority, task type, category, customer number, date range, and text search. Task search also matches the phone numbers on the linked call log (customer and lawyer).
4. **Phone filter is task-type aware:** a lawyer follow-up can be found by the lawyer's or the client's number; other tasks only by the customer's number.
5. **Ordering:** group by due **day in Indian time** (`DATE(CONVERT_TZ(col,'+00:00','+05:30'))`), then priority weight (a `CASE` expression), then due time, then `id` as a stable tie-breaker.
6. **Stats in 2 queries, not 15.** One query per table with conditional sums: `SUM(CASE WHEN <bucket rule> THEN 1 ELSE 0 END)` for each tab and card. The two queries run with `Promise.allSettled`, so if one fails the other half still shows.
7. **Priority on dispositions.** Migration `migrations/20260723120000-add-priority-to-call-dispositions.js` adds `priority TINYINT` (0 none, 1 low ... 4 urgent) and a composite index (`agent_id`, `resolution_status`, `priority`, `created_at`). A number sorts naturally. The API converts to and from words.

**Tricky bugs I fixed.**
- **Date filter replaced the bucket rule.** Both set `due_date`. With object spread the last key wins, so not-yet-due tasks showed in the Overdue tab. Fix: put the date filter inside an `Op.and` array so both conditions apply.
- **Timezone skew.** The DB stores UTC. "Today" must be the Indian day. Using server-local `setHours()` moved the window by 5.5 hours. `istDayBoundaries()` and `dateWhere()` now compute the Indian day edges as exact UTC instants. A plain `yyyy-MM-dd` means a full Indian day; a full ISO time is used exactly.
- **Counts not matching lists.** Fixed by building list and stats from the same helper functions.
- **Pagination with joins.** `distinct: true` and `subQuery: false`, so counts and limits are right when filtering on joined tables.
- **Lawyer number search** (three commits): the number lives on the call log, not on the disposition, so the search had to go through the nested join.

**Also (Aug 2026).** The agent-performance tasks API got a `dateField` option (`created_at` or `due_date`, allow-listed) so the calendar view can fetch tasks by due date.

**Interview questions.**
- *How do you get many counts in one query?* Conditional aggregation: `SUM(CASE WHEN cond THEN 1 ELSE 0 END)` once per count over one scan.
- *`Promise.all` vs `Promise.allSettled`?* `all` fails if any one fails. `allSettled` waits for all and tells you which failed, so you can return partial data.
- *Why add `id` to ORDER BY?* If many rows share the same sort value, their order can change between pages. A unique tie-breaker keeps pages stable.
- *How do you handle "today" for Indian users with UTC storage?* Work out the Indian calendar day, convert its start and end to UTC instants, and filter on those.
- *Why store priority as a number?* Sorting and indexing are simple. Words are mapped at the API edge.
- *Why move filtering to the backend?* Less data over the network, the DB can use indexes, and every client gets the same numbers.

---

### 20. Follow-up rules, new categories, dispositions without a call

**When:** Apr to Oct 2026. Files: `disposition/config/disposition-categories.config.ts`, `disposition/services/disposition-workflow.service.ts`, `disposition/services/disposition.service.ts`, `disposition/disposition.controller.ts`, `database/models/constants/*.ts`.

**How follow-ups work in general.** Creating a disposition publishes an event on an internal event bus. `DispositionWorkflowService` listens and creates `follow_up_tasks` rows by category rules (for example a task due in 48 hours after a consultation is created).

**What I changed.**
- **Filters (Apr):** disposition category and sub-category filters for call search and stats.
- **New categories:** "Call From Lawyer" sub-category (May), a test category (Jul), `callback_claimed` and `ai_call_claimed` (Oct).
- **Optional lawyer follow-up (Aug).** Before, a "consultation created" disposition always created two tasks (client and lawyer). Now the lawyer task is created only if the agent ticks it. The flag `create_lawyer_follow_up` is not a DB column, so I store it in the disposition `metadata` to carry it through the event to the workflow.
- **Sub-category opt-out (Aug).** A sub-category can set `auto_followup_enabled: false` and override its parent. Used for "customer disconnected" (no auto task).
- **Dispositions without a call log (Oct).** Lawyer-onboarding calls and international clients are handled outside the dialler, so there is no call row.
  - New categories `lawyer_onboarding` and `international_client` with their sub-categories.
  - `CALL_LOG_OPTIONAL_CATEGORIES`: only these may skip `call_log_id`. When a call is given, the old ownership checks still run.
  - New column `call_dispositions.linkage_type` (`with_call_log` / `without_call_log`). It is derived on the server by `deriveLinkageType(call_log_id)` and is never accepted from the client.
  - SQL migration `src/database/migrations/20261014-add-linkage-type-to-call-dispositions.sql`: makes `call_log_id` nullable where needed, adds the column and an index, backfills. Every step first checks `information_schema`, so it is safe to run twice.
- **Agent-scheduled follow-up (Oct).** For these two categories a task is created only if the agent ticked follow-up and picked a date. It is due on that date and carries the agent's notes. New task types `lawyer_onboarding` and `international_client` were added to the `follow_up_tasks.task_type` ENUM (`20261016-add-onboarding-international-task-types.sql`, values appended at the end so old rows are untouched).

**Interview questions.**
- *Why derive `linkage_type` on the server?* A client must never set a value that can disagree with the real data. One function decides it.
- *How do you write a migration that is safe to run twice?* Check `information_schema` for the column or index first and only then run the `ALTER`.
- *A unique index on a nullable column: can many rows be NULL?* Yes. MySQL InnoDB allows many NULLs in a unique index, so "one disposition per call" still holds.
- *Why pass the flag through metadata?* The event carries the saved record only. Metadata is saved, so the listener can read it. No schema change needed.

---

### 21. Blocked numbers (inbound deny list)

**When:** Sep 2026. Files: `src/features/call-center/exotel/blocked-number.service.ts`, `src/database/models/cc-blocked-number.model.ts`, `migrations/20260930120000-create-cc-blocked-numbers.js`, `blockIfDenylisted()` and `buildBlockedResponse()` in `exotel-webhook.service.ts`, guard in `notifications/welcome-whatsapp.service.ts`.

**Problem.** Spam callers rang agents again and again and wasted their time.

**How it works.**
1. Table `cc_blocked_numbers`: `phone_number` (unique), `reason`, `is_active`, `blocked_by`, `hit_count`, `last_hit_at`. Rows are added by hand in SQL.
2. At the very top of inbound routing, before any lock or agent work, `isNumberBlocked(number)` runs. It builds several forms of the number (raw, normalised with country code, digits only, last 10 digits) and does one indexed lookup.
3. If blocked:
   - if a call log for this call id already exists, just answer (the provider can call the webhook more than once per call)
   - else create a `call_logs` row already finished: status `failed`, no agent, duration 0, and `call_metadata.blocked = true` with the reason and the matched rule
   - `recordBlockHit()` runs without waiting: `hit_count = hit_count + 1` as SQL (not read-then-write) and `last_hit_at`
   - reply to the provider with "no destination, do not retry, do not record"
4. Blocked callers also do not get the welcome WhatsApp message.

**Decisions.**
- **Fail open on lookup.** If the lookup itself fails (DB down), the call is allowed. One spam call is better than blocking every real caller.
- **Fail closed after a match.** If writing the log row fails, the caller is still blocked.
- **Not cached** on purpose. A cache would make a new block take effect late. One indexed lookup is cheap.
- **Still logged**, so the team can see the number tried to call. A metadata flag is used instead of a new status value, so reports are not disturbed.
- **Count per call, not per webhook hit,** so the number is useful.
- A concurrent duplicate insert is caught through `UniqueConstraintError` on the call id.

**Interview questions.**
- *Fail open or fail closed?* It depends on which mistake costs more. Here blocking real clients costs more, so the lookup fails open.
- *How do you increment a counter safely under concurrency?* Let the DB do it: `SET hit_count = hit_count + 1`. Reading and then writing in code can lose updates.
- *Why not cache the list?* Fresh data matters more than saving one fast query.
- *How do you handle the same webhook arriving twice?* Use the call id as an idempotency key. Check before insert and rely on the unique index.

---

### 22. DTMF priority routing with spillover

**When:** Aug to Oct 2026. Files: `src/features/call-center/exotel/dtmf-priority.ts`, `src/core/services/dtmfPriorityConfig.service.ts`, routing code in `exotel-webhook.service.ts`, `exotel.controller.ts`, SQL `src/database/migrations/20261007-add-dtmf-priority-routing.sql`. (Pointer only for telephony details: see the Exotel section.)

**Problem.** Some callers should reach a specific group of agents. The caller presses a digit (1 to 9) after dialling.

**How it works.**
1. The controller reads the pressed digit from the webhook (the provider wraps it in quotes, so I strip them) and passes it as `dtmfCode`.
2. Agents have codes in `call_center_agents.metadata` (new JSON column): `{ "priorities": [1,3] }`, or the older `{ "priority": 3 }`. `getAgentPriorities()` reads both shapes and ignores bad values.
3. The digit comes **only on the first poll**. So I save it in `call_metadata.dtmf_priority` and reuse it on later polls for the same call.
4. Routing: narrow the free-agent pool to agents who serve that digit. If none is free, keep the caller queued for up to `spilloverAttempts` polls. After that "spill over" to any free agent.
5. The incoming-call banner (socket event) carries `dtmfPriority` and `dtmfPriorityMatched`, so the agent knows if the call was meant for them or spilled over.
6. Settings (`isEnabled`, `spilloverAttempts`) live in the `service_configs` table under key `dtmf_priority_routing`. They are read through a typed wrapper that never throws. A missing or broken value falls back to the old behaviour (enabled, 2 attempts).

**Interview questions.**
- *Why keep the setting in the DB?* To change it without a deploy. It is cached for a short time.
- *What should a config default be?* The behaviour that existed before the config. A missing row must not change anything.
- *Why keep `dtmf-priority.ts` free of DB code?* Pure functions are easy to unit test.

---

### 23. Call notes: edit or delete only by the author within 24 hours

**When:** Oct 2026. Files: `src/features/call-center/call-notes/call-notes.service.ts`, `call-notes.controller.ts`, `dto/call-notes.dto.ts`.

**Problem.** Notes about a phone number are a record of what was said. Anyone could edit or delete any note at any time. The frontend only hid the buttons.

**What I built.**
- `assertCanModify(note, userId)`: the user must be the agent who wrote the note **and** the note must be younger than 24 hours (`NOTE_EDIT_WINDOW_MS`). Otherwise `AuthorizationError`. No exception for managers or admins.
- The list API returns `can_edit` and `is_edited` per note, so the UI does not need its own rule.
- Pagination: `limit` (1 to 100), `offset`, `total`, `has_more`. Ordered by `created_at DESC, id DESC`, so pages never overlap.
- Agent names for a page are loaded in one batch instead of one call per note. The notes query and the "who am I" lookup run in parallel.

**Interview questions.**
- *Why enforce the rule in the backend?* The frontend can be bypassed with a direct API call. The server is the only place a rule is real.
- *Why return `can_edit` from the server?* One rule, one place. The UI just reads it.
- *Why sort by `created_at` and `id`?* Two notes can share the same timestamp. `id` makes the order unique.

---

### 24. Smaller config and data features

- **Lawyer list order flag (Oct 2026).** `src/core/services/lawyerListOrderConfig.service.ts` + `GET /api/v1/internal/config/lawyer-list-order`. User-service calls it to know if the lawyer list on the consultation screen should be shuffled, so the same lawyers are not always on top. Key `lawyer_list_order` in `service_configs`. Opt-in: only a real JSON `true` enables it. Never throws.
- **Lawyer consultation stats (Oct 2026).** `POST /api/v1/call-center/calls/lawyer-consultation-stats` with a list of lawyer ids. One SQL query returns, per lawyer, the number of connected consultations and distinct clients in the last 7 days. Inner query: group calls by consultation and keep those with talk time > 0. Outer query: join `consultations` and group by lawyer. Lawyers with no rows are returned as zeros.
- **Agent provider flag (Jul 2026).** One-line change in the outbound call request (`state_management` on the caller leg) in `exotel-integration.service.ts`. See the Exotel section.
- **New Relic config** tweaks in `newrelic.js` and `app.ts` (Mar 2026).
- **Plan documents.** I wrote plans before large changes (call status v1/v2/v3, AI calling, work queue, follow-up options, Bonvoice). Some were removed after the work was done.

---

### 25. Bonvoice: second telephony provider

**When:** late Oct to Nov 2026. Branch `VS-647-bonvoice-integration`. Tested on dev. Not in main, not in production.

**Problem.** All calling depended on one provider. The company wanted a second one (Bonvoice) for cost and backup, without rewriting the call-center and without risk to the existing flow.

**Design idea.** Put a small "switch" in front of the provider. Existing code asks the switch for "the provider" and does not care which one answers.

#### Part A: the provider switch (`src/features/call-center/telephony/`)

- `outbound-provider.interface.ts`: the contract both providers follow (`initiateOutboundCall`, `callLawyerForConfirmation`, `endCall`, `transferCall`). Same names and return shape the old service already had.
- `provider-switch.ts`: the choice is stored in `service_configs` under key `telephony_provider` as `{ outbound, inbound, agentChoice }`.
  - `getTelephonyProviderConfig()` never throws. No row or a DB error means the old provider.
  - `setTelephonyProvider()` runs in a transaction: deactivate the current row, insert version + 1. The same value again adds no row (idempotent). The table itself is the history of flips.
  - `getCallProvider(callLog)` reads the provider from the **row**, not from the switch. So a flip in the middle of a call cannot confuse later steps.
- `call-provider.ts`: `getOutboundProvider()` returns the right service object. The three call sites changed one line: `new ExotelIntegrationService()` became `await getOutboundProvider()`.
- `provider-readiness.ts`: refuses to switch to Bonvoice if required settings are missing.
- `telephony.controller.ts` + `telephony.routes.ts`: `GET/PUT /api/v1/call-center/telephony/provider` and `GET/PUT .../bonvoice-settings`. A Slack alert is sent on every flip.
- `telephony.permissions.ts`: one permission string, `call_center:telephony:update`, read from the JWT.

#### Part B: outbound calls (`src/features/call-center/bonvoice/`)

How a call starts (`BonvoiceOutboundService.placeCall`):
1. Make our own event id (`VS` + random characters) with `makeEventId()`.
2. Convert both numbers to the 10-digit form Bonvoice accepts (`toBonvoiceNumber`; non-Indian numbers are rejected).
3. **Create the `call_logs` row first** (same `CallService.initiateCall` as before). Store the event id as the call id, and `call_metadata.provider = 'bonvoice'` plus `bonvoice: { eventID, kind, legA, legB }`.
4. Call the Bonvoice click-to-call API through `BonvoiceApiClient` (axios, token header, 15 s timeout).
5. On failure: mark the row `failed`, release the agent, alert, and rethrow.
6. Return the same shape the callers already expect.

Three kinds of call: agent to client, agent to lawyer (confirmation), and lawyer to client (paid consultation).

#### Part C: events (webhook)

- `bonvoice-webhook.controller.ts`: always answers HTTP 200 and logs the raw body.
- `dto/bonvoice-webhook.dto.ts` `normalizeWebhookPayload`: the provider sends JSON or form data, the text `"None"`, and numbers as strings. This turns it all into one clean shape and never throws.
- `bonvoice-webhook.logic.ts`: **pure decision functions.** `planProgress` (ringing / answered) never moves a row backwards and never out of a final state. `planHangup` computes the final status, end time, duration and recording. A duplicate hang-up is skipped.
- `bonvoice-status.mapper.ts` `mapBonvoiceOutcome`: looks at the agent leg first. If the agent never answered, the result is `no_answer` or `failed`. Only if the agent answered does the customer leg decide: `missed`, `busy`, `short_call` (30 s or less) or `completed`. An unknown word becomes `failed` plus an alert. Same meanings as the existing resolver, so the dashboards read both providers the same way.
- `bonvoice-webhook.service.ts`: finds the row by event id, applies the plan, and appends each event to `call_metadata.callEvents`.
- `bonvoice-after-call.ts`: side effects. Socket events to the dashboard, agent status changes, and for consultations: mark ongoing, mark completed, post-call work and tickets.

#### Part D: missing events and many environments

- `bonvoice-reconcile.ts`: for a row that never got a hang-up, ask Bonvoice's call-log API. No data yet means still ongoing (write nothing). A hang-up record goes through the **same** `applyHangup` path as the webhook. Used by the dashboard refresh button and by a cleanup route for a scheduled job.
- The old provider's cleanup job skips Bonvoice rows, so the two never touch each other's calls.
- `bonvoice-event-relay.ts`: Bonvoice allows one webhook URL per account, but we have several environments. Each call carries its environment name. If an event belongs to another environment, the receiver forwards it there and still answers 200.
- `bonvoice-settings.ts`: runtime settings (caller id, channel ids, relay URLs, number-to-environment map) in `service_configs` with a 30-second cache and env values as fallback. Only the secret token stays in env.

#### Part E: inbound calls (phase 2)

- Bonvoice asks our URL "who should take this call?" and waits for one phone number.
- `BonvoiceInboundService.route()` is an **adapter**. It converts the request to the shape the existing inbound router expects and calls the same `handleIncomingCallRouting`. So block list, free-agent pool, atomic claim, sticky agent, round-robin and DTMF priority are all reused. No routing rule is written twice.
- Differences handled in the adapter:
  - no call id in the request, so a placeholder id is used and replaced by the real id on the first event
  - SIP (web phone) agents are rung on their mobile; an agent with no usable mobile is released and an alert is sent
  - "no agent free" returns the provider's "no destination" reply, and the provider's own fail-over takes the call
  - any error still returns HTTP 200 with "no destination"
- Events are matched by call id, else by caller + number in a time window. If the call was answered by the fail-over number and not our agent, the agent is released at once.

#### Part F: hardening and per-agent choice (phase 3, Nov 2026)

- New column `call_logs.provider VARCHAR(16) DEFAULT 'exotel'` + index (`provider`, `initiated_at`). SQL `src/database/migrations/20261030-add-provider-to-call-logs.sql`, guarded by `information_schema`, with a backfill from the metadata marker. VARCHAR and not ENUM, so a third provider needs no `ALTER`.
- Recording download and the call explainer were made provider-aware.
- **Per-agent provider.** `resolveOutboundProvider({ agentId, kind, callerPermissions })`:
  - consultations always follow the company switch
  - an agent's own choice (`call_center_agents.metadata.preferred_provider`) counts only if the manager turned on "agents may choose", or the caller is a manager
  - if the chosen provider is not ready, fall back to the switch and record why
  - each call row stores how its provider was chosen (`switch`, `agent`, `fallback`)
  - `describeAgentTelephony()` explains the same decision in one line for the dashboard
- **Consultation time limit.** `toMaxTalktime()` converts our limit to the provider's max-talk-time field: rounds up, caps at 7200 s, and always sends a value (the provider's default would otherwise be shorter).
- Docs I kept: outbound plan, phase 2 plan, working notes, code map, parity audit against the first provider, mixed-mode analysis, master checklist, production checklist.

**Tests.** No Jest in this repo. Tests are ts-node scripts wired to npm scripts: `test:telephony`, `test:bonvoice`, `test:bonvoice-webhook`, `test:bonvoice-inbound`, `test:call-outcome`, plus a manual API probe `bonvoice:api`. The webhook tests use real payloads captured from test calls.

**Known gaps (from my own checklist, still open).** No end-call or transfer API on the new provider; no "try next agent" after a no-answer on inbound; webhook authenticity (secret header) not yet enforced; the scheduled cleanup job for Bonvoice rows not yet added by devops; production numbers and account not set up.

**Tricky parts and decisions.**
- **Row first, API second.** If the event comes before our API call returns, the row already exists. If the API fails, we still have a record.
- **Pure logic split from I/O.** The mapper and plan functions take data in and give data out, so they can be tested with real payloads and no DB.
- **Same write path for webhook and reconcile,** so both give the same result.
- **Everything defaults to the old provider.** No row, bad JSON or a DB error cannot break calling.
- **The row remembers its provider.** Never read the switch for an existing call.
- **Reuse the router through an adapter** instead of copying rules.
- **No migration for the agent preference** (existing JSON column), to avoid deploy-order problems.

**Interview questions.**
- *How did you add a second provider without rewriting the system?* An interface with the same methods, a factory that reads a DB switch, and a one-line change at each call site. This is the strategy / adapter pattern.
- *Why store the switch in the DB and not in env?* To flip it at runtime without a deploy, keep history (versioned rows), and show who changed it.
- *Why create the DB row before calling the provider?* Events can arrive very fast. And if the API call fails, we still have a row to mark as failed and an agent to release.
- *How do you handle duplicate or out-of-order events?* A small state machine: never move backwards, never leave a final state, ignore a repeated hang-up.
- *Why always return 200 to the provider's webhook?* A non-200 may trigger retries or disable the hook. I log and alert inside, and reconcile later if needed.
- *One webhook URL, many environments. How?* Tag each call with its environment and relay events to the right one.
- *How do you test an integration with a third party?* Pure functions tested with captured real payloads, a manual probe script against the real API, then end-to-end calls on dev with a checklist.

---

### Tech stack and packages in lawyer-appointment

Read from `package.json` and checked against imports in `src/`.

**Runtime and framework**
- **Node.js + TypeScript 5** — whole codebase. `ts-node` + `nodemon` for dev, `tsc` + `tsc-alias` for build, `tsconfig-paths` for the `@core/*`, `@features/*`, `@database/*` path aliases.
- **express 5** — HTTP framework. Routers per feature, mounted in `src/app.ts`.
- **reflect-metadata** — needed by class-validator / class-transformer decorators.
- **dotenv** — loads `.env` at start.

**Database**
- **sequelize 6** — ORM for MySQL. Models in `src/database/models`. I used it for CRUD, `findAndCountAll`, hooks (`beforeCreate` / `beforeUpdate`), transactions, `Op` operators, `sequelize.literal`, and raw `sequelize.query` with replacements.
- **mysql2** — the MySQL driver under Sequelize.
- **sequelize-cli** (dev) — runs the JS migrations in `migrations/`. Some of my migrations are hand-run SQL files in `src/database/migrations/`.
- **ioredis** — Redis client (`src/database/redis.ts`). Used for the config cache behind `serviceConfigService` and for alert dedupe. My DTMF and lawyer-list config services read through that cache.

**Validation**
- **class-validator** + **class-transformer** — DTO classes with decorators, used through `validateDto` (AI call DTO, work queue queries, proceedings, documentation service).
- **joi** — schema validation in older core routes, used through `validateRequest` (slot confirmation rules, consultation rules).
- **class-validator-jsonschema** — turns DTO classes into JSON schema for my Swagger generator.

**Real-time and scheduling**
- **socket.io** — live dashboard events (incoming call banner, AI call available / claimed, call updates). My code emits through the call-center socket service.
- **node-cron** — in-process schedules (daily reset of assignment counts; the first stale-call job).
- **Queues** — there is no message queue library in this repo. Background work is done with cron jobs, externally scheduled HTTP routes (Jenkins), an in-process event bus, and `setImmediate`.

**HTTP clients and other services**
- **axios** — outbound HTTP: telephony provider APIs (including my Bonvoice client and DND detection) and `src/core/utils/apiCaller.ts`, which the internal service clients use (user, finance, document services).
- **lodash** — small helpers (`_.get`, `_.pick`) in internal service clients and document code.

**Auth and security**
- **jsonwebtoken** — JWT check in `authenticateToken`; my routes read `req.userId` and `req.permissions`.
- **helmet**, **cors**, **express-rate-limit** — security headers, CORS and rate limits in `src/core/config/security.ts` (used by the app; not written by me).
- **bcryptjs** — password hashing in the auth feature (not my code).

**Dates and phones**
- **moment-timezone** — date ranges in Indian time (recent activity, schedule call list, slot checks).
- **libphonenumber-js** — behind `normalizePhoneNumber` in `src/core/utils/phoneUtils.ts`; I use it in blocked numbers, call search and routing.
- **rrule** — recurring availability rules (existing code, not mine).

**Files and cloud**
- **@aws-sdk/client-s3**, **@aws-sdk/s3-request-presigner** — S3 uploads and signed URLs (`src/core/services/s3.service.ts`); used by documents and call recordings.
- **mime-types** — file type checks in document upload.
- **uuid** — ids, and `validate` to check owner ids are UUIDs.

**Logging, monitoring, docs**
- **winston** — app logger (`src/core/utils/logger.ts`), used everywhere.
- **newrelic** — APM agent; I made its start-up safe for local runs.
- **@slack/web-api** — Slack alerts. I built the first alert module on it; the current alert kernel also uses it.
- **swagger-ui-express** — serves the Swagger UI at `/api-docs` for my generated spec.

**AI and media (present in the repo, not my work)**
- **@google/genai**, **openai**, **@anthropic-ai/sdk** — call transcription and daily report features.
- **livekit-server-sdk** — in-app audio/video calls.

**Testing and code quality**
- No Jest. Tests are plain **ts-node** scripts under `__tests__/`, each with an npm script (`test:*`). I wrote the Bonvoice, telephony and call-outcome tests this way.
- **eslint**, **prettier** (dev) — lint and format. The repo notes say the lint setup is currently broken, so type checking with `tsc` is the real check.

---

### Patterns I used again and again (good summary for interviews)

1. **Atomic conditional UPDATE** for "only one winner" (AI claim, callback claim).
2. **Idempotency** through unique keys and "check then rely on the constraint" (dispositions, blocked calls, access grants, provider switch).
3. **Never guess; reconcile with the source of truth** (stale calls, missing webhooks).
4. **One function owns one rule** (status resolver, bucket rules, linkage type), so screens never disagree.
5. **Batch, don't loop** (dispositions, notes, names in chunks) to remove N+1 queries.
6. **Config in the database with safe defaults**, so behaviour changes without a deploy and a missing row changes nothing.
7. **Side effects never break the main flow** (alerts, access grants, ICS, name lookups are all try/catch and best effort).
8. **Pure functions for decisions, thin code for I/O**, so logic can be tested without a database.
9. **Feature flags for risky changes** to core fields.
10. **Audit trail in JSON metadata**: who or what changed a row, when and why.

---

# Part 4: User Service, Communication Service, Document Service

## User Service (user-service)

### What this service does

- It owns users: clients, lawyers, admins and call-center agents.
- It handles login (OTP), JWT tokens, roles and permissions.
- It stores the lawyer profile: basic details, professional details, KYC, pricing, courts, languages.
- It gives the admin dashboard its APIs: list/filter lawyers and clients, verify lawyers, edit data on their behalf, audit logs.
- It serves app config to the mobile apps (minimum build number, banners, help numbers).
- It calls other services over HTTP: comm-service (push, WhatsApp, deep links), finance-service (wallet), doc-service (PDF), lawyer-appointment service (expertise list, config).

### Tech stack and packages

| Package | What it is | Where it is used here |
|---|---|---|
| `@nestjs/common`, `@nestjs/core`, `@nestjs/platform-express` | NestJS framework (TypeScript, on top of Express) | Every module: controllers, services, guards, interceptors, pipes |
| `typeorm`, `@nestjs/typeorm`, `mysql2` | ORM and MySQL driver | Entities in `src/database/entities`, migrations in `src/database/migrations`, QueryBuilder filters in `src/admin/admin.service.ts` |
| `class-validator`, `class-transformer` | DTO validation and value transforms | All DTO files, e.g. `src/admin/dto/admin.dto.ts`, custom decorator `src/common/validators/date.decorator.ts` |
| `@nestjs/swagger` | Swagger / OpenAPI docs | `@ApiTags`, `@ApiOperation`, `@ApiProperty` on controllers and DTOs; set up in `src/main.ts` |
| `@nestjs/jwt`, `@nestjs/passport`, `passport`, `passport-jwt` | JWT login and guards | `src/auth/strategies/jwt.strategy.ts`, `JwtAuthGuard`, `RolesPermissionsGuard` |
| `@nestjs/config`, `joi`, `dotenv` | Config from env + env validation | `src/config/configuration.ts` (e.g. `MAX_COURT_ASSIGNMENTS`, `INTERNAL_API_KEY`, `APP_OPEN_URL`) |
| `ioredis` / `redis` | Redis client | `src/redis/redis.service.ts`: lawyer online status, config cache for random ordering, Slack alert de-dup |
| `@slack/web-api` | Slack API client | `src/common/slack-alerts/*`: onboarding and verification alerts, CSV upload |
| `moment-timezone` | Dates and time zones | `src/call-quota/call-quota.service.ts` (today's date), Slack alert time in `lawyer.service.ts` |
| `rxjs` | Reactive streams (Nest uses it for interceptors) | `tap` / `catchError` in `src/admin/interceptors/admin-audit.interceptor.ts` |
| `lodash` | Utility functions | `isEmpty` checks in `src/lawyer/lawyer.service.ts` |
| `uuid` | UUID generation | Permission ids in `src/database/seeds/seed.ts` |
| `helmet`, `compression`, `@nestjs/throttler` | Security headers, gzip, rate limit | `src/main.ts`, `src/app.module.ts` (existing setup, not my work) |
| `winston`, `nest-winston`, `newrelic` | Logging and monitoring | Logger in services; New Relic agent loaded on start (existing setup) |
| `jest`, `ts-jest`, `@nestjs/testing` | Unit tests | `src/admin/admin.service.updateUserContact.spec.ts`, `admin.service.getLawyerProfilePdf.spec.ts`, `admin.service.setUserStatusFlags.spec.ts`, `src/lawyer/utils/shuffle.spec.ts` |
| Node `fetch` + `AbortSignal.timeout` | Built-in HTTP client | Calls to comm-service, finance-service, doc-service, LA service in `src/internalService/*` |

Global validation: `ValidationPipe` with `whitelist: true` and `forbidNonWhitelisted: true` in `src/main.ts`. This means unknown fields in a request body cause a 400 error.

---

### 1. Lawyer profile sections: career, case highlights, education (Dec 2025, auth added Feb 2026)

**Problem.** The lawyer profile has three list sections. Only create/update/delete existed. The app could not read them back.

**What was built.** For each of the three modules: get all, get by id, get by user id.

**How it works.**
1. Request: `GET /career`, `GET /career/by-id?id=`, `GET /career/by-user?userId=` (same for `/case-highlight` and `/education` controllers).
2. Controller checks the query param. If missing it throws `BadRequestException`.
3. Service calls the TypeORM repository: `find` / `findOne` with `isDeleted: false`, `relations: ['user']`, newest first.
4. If one record is not found it throws `NotFoundException` (404).
5. In Feb 2026 `JwtAuthGuard` + `@ApiBearerAuth()` were added to all read routes.

**Key files.** `src/career/career.controller.ts`, `src/career/career.service.ts`, `src/casehighlight/*`, `src/education/*`.

**DB.** Entities `Career`, `CaseHighlight`, `Education`. The `Career` column `active` was renamed to `isActive` to match the others.

**Bugs / decisions.**
- First version had two `@Get()` handlers on the same path (get all and get by id). Nest only ever reaches the first one. Fixed by moving get-by-id to `/by-id`.
- Case highlight `create` had its arguments swapped (`create(user, dto)` vs `create(dto, user)`), so the user was not linked. Fixed.
- Soft delete: rows are never removed, only `isDeleted = true`.
- Later (Apr 2026) the admin lawyer list joined these three tables so the dashboard shows them.

**Date rule (Jan 2026).** Start and end dates in career and education must be 1980 or later. Built as a reusable custom decorator `@IsAfterYear(1980)` in `src/common/validators/date.decorator.ts` using `registerDecorator` and a `ValidatorConstraint` class. The first version was hard-coded to 1980; it was rewritten so the year is a parameter.

**Interview questions.**
- Q: Why did two `@Get()` routes on the same path not work? A: Nest matches routes in order. The first handler always wins. Each route needs its own path.
- Q: What is soft delete and why use it? A: We set a flag instead of deleting the row. We keep history and can undo. Every read must filter `isDeleted = false`.
- Q: How do you write a custom validator in class-validator? A: Make a class that implements `ValidatorConstraintInterface` with `validate()` and `defaultMessage()`, then wrap it with `registerDecorator`. Pass options through `constraints`.

---

### 2. Swagger documentation for the whole service (Dec 2025 - Jan 2026)

**Problem.** Frontend and mobile developers had no clear API contract.

**What was built.** Swagger decorators across about 31 files: `@ApiTags`, `@ApiOperation`, `@ApiResponse`, `@ApiQuery`, `@ApiBearerAuth` on controllers and `@ApiProperty` / `@ApiPropertyOptional` on DTOs. In Jan 2026 detailed `@ApiBody` schemas with examples were added for 5 lawyer APIs (about 180 lines in `src/lawyer/lawyer.controller.ts`), including the multi-section "create lawyer profile" body.

**How it works.** `src/main.ts` builds the document with `DocumentBuilder` and serves it at `<api prefix>/docs`. Decorators add metadata; Nest reads it at start.

**Interview questions.**
- Q: How does Swagger know the request body shape? A: From the DTO class and its `@ApiProperty` decorators. For free-form bodies we give an `@ApiBody` schema by hand.
- Q: Why document status codes? A: The client knows which errors to handle (400, 401, 403, 404, 409).

---

### 3. Call quota APIs and bug fixes (Jan 2026)

**Problem.** The system needs to track how many calls a user has per day (total and free) and how many are used.

**What was built.** A new `CallQuotaModule` with four APIs: create, update, get all, get for user.

**How it works.**
1. `POST /call-quota` (JWT). Body: `data` with four counters `TTC` (total today calls), `TFC` (total free calls), `UTTC` (used total), `UTFC` (used free). Each is validated as an integer >= 0 with `@ValidateNested` + `@Type`.
2. Service finds the user, builds today's date with `moment().format('YYYY-MM-DD')`.
3. It looks for an existing row for this user and date. If found it updates `data` (upsert). If not, it creates one.
4. `PATCH /call-quota` updates by quota `id` or by `userId` (+ optional date). If neither is given: 400.
5. `GET /call-quota/all` returns all rows but removes the full user object and returns only `userId`.
6. `GET /call-quota` returns today's quota for a user.

**Key files.** `src/call-quota/call-quota.controller.ts`, `call-quota.service.ts`, `dto/call-quota.dto.ts`. Entity `CallQuota` (`src/database/entities/call-quota.entity.ts`, already existed) registered in `data-source.ts`.

**Bugs fixed.**
- Typo in a counter name (`UTTTC` -> `UTTC`), which made the API contract wrong.
- Create was overwriting `isActive` / `isDeleted` defaults; removed so the entity defaults apply.
- Unused query DTO removed.
- Feb 2026: update and get-all had no auth guard; `JwtAuthGuard` added.

**Interview questions.**
- Q: How do you make "one row per user per day"? A: Check for a row with the same user and date first. Update it if it exists, else insert. (A DB unique index would make this safe under race; not confirmed if one exists.)
- Q: Why not return the user relation? A: It leaks personal data and makes the response big. Return only the id.

---

### 4. Lawyer status API and online/offline heartbeat (Jan and Apr 2026)

**What was built.**
- `GET /lawyer/lawyerStatus` (JWT). Returns the `lawyerStatus` of the logged-in lawyer. If the user is not a lawyer type (`solo_practitioner` or `law_firm_lawyer`) it returns 404.
- Heartbeat fix (Apr 2026): `POST /lawyer/lawyerHeartbeat` only wrote to Redis (key with 60 second TTL). Now it also writes `lawyerStatus` (ONLINE / OFFLINE) to the `users` table. The route also had no guard and read `user.id` instead of `user.userId`; both fixed.

**Key files.** `src/lawyer/lawyer.controller.ts`, `src/lawyer/lawyer.service.ts` (`getLawyerStatus`, `lawyerHeatbeat`).

**Interview questions.**
- Q: Why keep status in both Redis and MySQL? A: Redis is fast and expires by itself if the app stops sending heartbeats. MySQL keeps the last known value so other queries and the admin panel can read it.
- Q: What bug did `user.id` cause? A: The JWT strategy returns `userId`, not `id`. So `user.id` was `undefined` and no row was updated.

---

### 5. Active courts in lawyer profile, and profile V2 (Jan and Aug 2026)

**Problem.** `getLawyerProfileData` returned every court mapping, including deleted or inactive ones.

**What was built.**
- A filter that keeps a court only if the mapping is not deleted, the mapping is active, and the court itself is not deleted. Duplicates removed with `Set`.
- Aug 2026: `GET /lawyer/lawyerProfileV2`. Same data, but courts come back as objects (`id`, `courtId`, `name`, `state`, `district`) and languages come back as `{code, name}`. The old route stays, so old app versions do not break. An optional `key` query returns just one section (`basicDetail`, `professionalDetail`, etc.); an invalid key gives 400.

**Interview questions.**
- Q: Why make a V2 route instead of changing V1? A: Old app builds still call V1 and expect the old shape. A new route lets both work.

---

### 6. Name and date-of-birth validation (Jan 2026)

**Problem.** Users could save empty names, names with only spaces, special characters, and a date of birth in the future.

**What was built.**
- Name: `@Transform(({value}) => value?.trim())` then `@Matches(/^[a-zA-Z0-9\s]+$/)` -> only letters, numbers, spaces. Applied in user and lawyer DTOs.
- Date of birth: a service check in create/update flows of `lawyer.service.ts` and `users.service.ts`: DOB cannot be today or in the future.

**Design decisions.**
- First version used four custom validator classes (not-empty, not-future, min age 18, max age 120) in one file of about 150 lines. After review this was replaced by a simple regex and a small service check. The 18+ age rule and the 100 character limit were removed. Reason for removing them: not confirmed (looks like a product decision from test cases).
- Later (Aug 2026) the admin edit DTO uses a slightly wider regex that also allows a dot after a letter (for names like "Adv. Kumar").

**Interview questions.**
- Q: Why `@Transform` before `@Matches`? A: Trim first, so " Ram " passes and "   " becomes empty and fails.
- Q: DTO validation or service validation? A: Shape and format go in the DTO. Rules that need "today" or the database go in the service.

---

### 7. KYC: auth, admin filter, notifications, audit (Jan - Aug 2026)

**What was built.**
- Lawyer search now requires `isVerified = true` and an approved KYC row (`EXISTS` sub-query). This filter was commented out before; it was turned back on (Jan 2026).
- Auth: whole KYC controller behind `JwtAuthGuard`. Approve route needs permission `kyc:approve`; read by user needs `kyc:read` (Feb / Apr 2026).
- Admin lawyer list filter `kycStatus` = `not_submitted | pending | approved | rejected`. Each one is an `EXISTS` / `NOT EXISTS` sub-query on the `kyc` table. "Pending" means not approved and no rejection reason. "Rejected" means not approved and a rejection reason is present.
- `Kyc.approver` relation added (`ManyToOne` to `User` on the `approvedBy` column) so the dashboard can show who approved.
- Push notifications for KYC: `kyc_submitted`, `kyc_re_review`, `kyc_approved`, `kyc_rejected` (with reason).
- Aug 2026: KYC approve is written to the admin audit log with the previous state (was it approved, by whom, rejection reason).

**Key files.** `src/kyc/kyc.controller.ts`, `src/kyc/kyc.service.ts`, `src/admin/admin.service.ts`, `src/database/entities/kyc.entity.ts`.

**Interview questions.**
- Q: Why `EXISTS` instead of a JOIN for the KYC filter? A: A JOIN can duplicate lawyer rows and break pagination counts. `EXISTS` only answers yes/no per lawyer.
- Q: Guard order? A: `JwtAuthGuard` runs first and sets `req.user`. `RolesPermissionsGuard` then checks the permission names from the `@Permissions()` decorator.

---

### 8. Auto un-verify a lawyer when the profile changes (Apr - Nov 2026)

**Problem.** A verified lawyer could change name, expertise or KYC documents after verification. The profile shown to clients was then no longer the one the admin checked.

**What was built.** `unverifyLawyer(lawyerId, context)` in `src/lawyer/lawyer.service.ts`.

**How it works.**
1. When a verified lawyer updates basic details, professional details or KYC, the code remembers `wasVerified` before saving.
2. If the lawyer was verified: set `isVerified = false`, set stage back to 5, and write stage 5 in `profile_details`.
3. Append an entry to `profile_details.updated[]` (a JSON history list): section, time, fields changed, payload. Nov 2026 also appends `profile_details.unverified[]` with reason and actor.
4. Send a push notification to the lawyer ("sent for re-review").
5. Send a Slack alert to the lawyer onboarding channel with the reason (Oct 2026).
6. If a KYC that was approved is edited, the KYC is auto-disapproved with a fixed rejection reason.

**Bugs / decisions.**
- First version un-verified and notified on every update, even for lawyers who were never verified. Fixed with the `wasVerified` check (May 2026).
- All side effects are "fire and forget" (`.catch(() => {})`). A Slack or push failure must not fail the lawyer's save.
- Slack de-dup: the first key was `lawyer-unverified-<id>` with 15 min TTL, which hid a second real change. Changed to include a timestamp with a 60 second TTL. No Slack alert when an admin rejects KYC by hand (they already know).
- Edits done by an admin on the lawyer's behalf do NOT un-verify (see feature 12).
- Bar registration number change by the lawyer: first it un-verified; that was removed the same day (Oct 2026).

**"Altered profile" admin filter (Oct - Nov 2026).** Finds lawyers who were verified, are not verified now, are active, and no admin revoked them: `isVerified = false AND verified_at IS NOT NULL AND (verification_revoked_at IS NULL OR verification_revoked_at < verified_at)`. The idea: an admin revoke stamps `verification_revoked_at`; an automatic un-verify stamps nothing.

**Interview questions.**
- Q: Why fire-and-forget for notifications? A: The main action is saving the profile. Alerts are extra. If Slack is down the user should still get a success.
- Q: What is the risk of fire-and-forget? A: Failures are silent. You need logs. For important events use a queue with retry.
- Q: How do you tell "admin revoked" from "auto un-verified" without a new column? A: Compare two timestamps that already exist. No revoke stamp newer than the last verify means it was automatic.

---

### 9. "Lawyer ready for verification" Slack alert and daily onboarding summary (Apr - Oct 2026)

**Problem.** The ops team did not know when a lawyer had finished all steps and was ready to be verified.

**What was built.**
- `VerifiabilityService.checkAndAlertVerifiable(userId)` in `src/common/verifiability/verifiability.service.ts`. It is a shared module used by KYC, pricing, lawyer and migration modules.
- Rule: the lawyer is not verified, not deleted, has milestones `basic_details`, `professional_details`, `kyc_details` in `profile_details.stages`, and has an active pricing row. Then it sends a HIGH Slack alert. De-dup key `lawyer-verifiable-<userId>` for 24 hours.
- It is called after KYC create, after pricing create/update, and after professional details are completed.
- Admin list filter `verifiable=true` uses the same rule in SQL (`JSON_EXTRACT` on stages + `EXISTS` on pricing + `isVerified = false`).
- Daily summary: `POST /cron/daily-onboarding-summary`, protected by `InternalApiKeyGuard` (header `x-api-key` compared with env `INTERNAL_API_KEY`). It is meant to be called by an external scheduler (code comment says a Jenkins job). It loads users created in the last 24 hours, builds a CSV in memory (with proper CSV escaping) and uploads it to Slack. Later it was split: one CSV for lawyers to the lawyer channel, one for clients to the user channel.
- Slack channels `LAWYER_VERIFICATION` then `LAWYER_ONBOARDING` added to config (env names `SLACK_CHANNEL_LAWYER_ONBOARDING_NAME / _ID / _BOT`). Helper `onboardingChannelFor(userType)` picks the channel. New-user alert text differs for lawyers.

**Design decision.** The first version put the check inside `kyc.service.ts` and looked at raw fields. It was moved to a shared service that reads the stage milestones, so all callers use one rule.

**Interview questions.**
- Q: Why an API endpoint for a cron instead of `@Cron` inside the app? A: With more than one pod, an in-app cron runs on every pod. One external trigger runs it once.
- Q: How do you stop duplicate Slack alerts? A: A de-dup key stored in Redis with a TTL. Same key inside the TTL is skipped.
- Q: Three DB queries in the summary used `Promise.all`. Why? A: They do not depend on each other, so run them in parallel. Then join in memory with `Map` to avoid N+1 queries.

---

### 10. Courts: search cities by state, atomic add-court, limits, client filter (Apr - Sep 2026)

**What was built.**
- `GET /.../cities/search-by-state?state=&search=` (permission `city:read`). Case-insensitive match, max 20 rows, for autocomplete.
- Add court rewritten to be atomic. Every court must be mapped to a city. The body takes either `cityId` (existing city) or `cityData` (latitude + longitude for a new city), never both.
- Latitude and longitude made required in the create-city DTO.
- Max courts per lawyer: was hard-coded 3, then 5, then moved to env `MAX_COURT_ASSIGNMENTS` (Joi default 5) read as `limits.maxCourtAssignments`. The value is also returned in the get-lawyer response so the dashboard can show the limit.
- Assigning a court now calls `syncProfessionalDetailsMilestone(userId)`. The professional details milestone is stamped only when experience, expertise, documents, language AND at least one court are all present.
- Client-side lawyer search: new `courtIds` filter in `FilterLawyerDto`, done with an `EXISTS` sub-query on `court_lawyer` joined to `court` (mapping active, not deleted, court not deleted). Added to both the location query and the fallback query.

**How add-court works.**
1. Validate: need `cityId` or `cityData`; not both; coordinates required for a new city.
2. `queryRunner.startTransaction()`.
3. If `cityId`: load city, check its state equals the court state. Else: find city by district + state, or create it.
4. Check duplicate court (name + state + district) -> `ConflictException` (409).
5. Save court. Create or re-activate the `court_city` mapping (primary).
6. Commit. On any error: rollback and rethrow. Always release the query runner.

**Key files.** `src/court-city/court-city.service.ts`, `src/court-city/dto/create-court.dto.ts`, `src/admin/admin.service.ts`, `src/config/configuration.ts`, `src/lawyer/lawyer.service.ts`.

**DB.** `court`, `city`, `court_city`, `court_lawyer`.

**Interview questions.**
- Q: Why a transaction here? A: Three writes (city, court, mapping). If the last fails we must not leave a court without a city.
- Q: Why `finally { release() }`? A: The query runner holds a DB connection. If we do not release it, the pool runs out.
- Q: Why move the limit to env? A: Business changed it twice. Config change needs no code change or review.

---

### 11. Admin dashboard: lawyer and client list filters (Apr - Nov 2026)

**Problem.** Ops needed to slice the lawyer and client lists in many ways.

**Filters added to `GET /admin/lawyers` (`LawyerFilterDto`).**
`kycStatus`, `noCourtAssigned`, `courtName`, `verifiable`, `basicDetailsCompleted`, `professionalDetailsCompleted`, `onboardingSource`, `registrationSource`, `isVerified`, `includeOtpOnly`, `dashboardApproved`, `barRegistrationNumber`, `alteredProfile`, `flaggedOnly`, `statusFlags`.

**Filters added to `GET /admin/clients` (`ClientFilterDto`).** `city`, `state` (the city text also matches state), `noName` (name is NULL), `flaggedOnly`, `statusFlags`.

**Other.** `userTypes` filter (comma list) on the user-roles list. Page `limit` max raised from 100 to 100000 (used for exports; a risk for big lists).

**How it works.**
- Query params -> DTO. Booleans arrive as strings, so `@Transform(({value}) => value === 'true' || value === true)`.
- Arrays can arrive as `a,b` or repeated params; a transform splits and trims them.
- Service builds one TypeORM `QueryBuilder` and adds `andWhere` per filter.
- JSON columns are read with MySQL `JSON_EXTRACT(userDetails.profile_details, '$.stages.basic_details') IS NOT NULL`.
- "Has no court" uses `NOT EXISTS (select 1 from court_lawyer ...)`.
- OR groups use `new Brackets(...)` so they do not break the other AND conditions.
- `registrationSource=admin_migration` also matches NULL (old rows had no value).

**Tricky bug.** `includeOtpOnly` first hid OTP-only lawyers unless the flag was set. For a release the logic was flipped: when the flag is true, the list shows only lawyers with no basic details. Easy to get wrong; the name and the behaviour are not a perfect match.

**Interview questions.**
- Q: Why are query booleans a problem? A: Everything in a query string is text. `"false"` is truthy in JS. You must convert it.
- Q: Why `Brackets`? A: `A AND B OR C` is not `A AND (B OR C)`. Brackets force the grouping.
- Q: Is filtering on a JSON path fast? A: No, it cannot use a normal index. For hot filters use a generated column or a multi-valued index (done for status flags, feature 18).

---

### 12. Admin edits on a lawyer's behalf: basic details, pricing, consultation preferences VS-684 (Aug - Nov 2026)

**Problem.** Call-center agents onboard lawyers by phone. They need to fill the lawyer's data from the dashboard, safely and with a record of who did it.

**What was built.**
- `PATCH /lawyer/basic-details?userId=` with permission `lawyer:edit-basic-details`. Partial update of name, email, gender, date of birth.
- `POST /lawyer-pricing/admin?userId=` with permission `lawyer:edit-pricing`. Reuses the same versioned pricing `create()` the lawyer app uses.
- `PATCH /lawyer-pricing/admin/questions?userId=` (VS-684, Nov 2026), same permission. Sets four yes/no answers: `testCall`, `proBono`, `lowFeeCase`, `homeVisit`. Only the fields sent are changed. They are stored in `lawyer.metadata`.
- `years_of_experience`: DB default 1 and `@Min(1)` in two DTOs, then the `@Min(1)` was removed again the same day.
- `aibe_pass` (`yes` / `no`) field added to professional details (Nov 2026), for both lawyer and admin flows.

**How basic-details edit works.**
1. Guards: JWT -> permission check. `AdminAuditInterceptor` wraps the call.
2. Remove undefined fields. If nothing is left -> 400.
3. Load user (404 if missing). Validate DOB (valid date, not today/future). `validateUser` checks uniqueness.
4. Put the old values on `req.auditPreviousState` for the audit log.
5. Update the `users` row.
6. If name + email + gender + DOB are all present after merge, stamp the `basic_details` milestone (stage 3).
7. Append to `profile_details.updated[]` with `actor` = admin id.
8. It does NOT un-verify the lawyer and does NOT notify the lawyer.

**DB.** New permissions seeded in `src/database/seeds/seed.ts` plus a SQL script `src/database/scripts/lawyer-basic-admin-permissions.sql`.

**Notes.** The pricing commit and the VS-684 commit were each reverted once and re-applied (git history shows commit, revert, commit).

**Interview questions.**
- Q: Why a separate permission and not `lawyer:update`? A: Least privilege. An agent can fix a name but cannot verify a lawyer.
- Q: Why reuse `create()` for admin pricing? A: One code path for the versioning rule. Only the "who is the target user" differs.
- Q: PATCH vs PUT? A: PATCH changes only the fields sent. PUT replaces the whole resource.

---

### 13. Admin audit log (May 2026)

**Problem.** No record of which admin did what. Needed for trust and for debugging.

**What was built.** A generic audit trail made of a decorator, an interceptor, a table and read APIs.

**How it works.**
1. Mark a route: `@AuditAction({ action: 'VERIFY_LAWYER', resource: 'lawyer', resourceIdFrom: 'lawyerId', captureBeforeState: true })`. This uses `SetMetadata`.
2. `AdminAuditInterceptor` reads the metadata with `Reflector`. No metadata -> do nothing.
3. Before the handler: take admin id, role, user type from `req.user`; client IP (from `x-forwarded-for` first); user agent; correlation id. Clean the body: fields like `password`, `otp`, `token`, `refreshToken` become `***REDACTED***` (recursive).
4. Find the resource id by a path name in params, then body, then query.
5. Services that change data put the old values on `req.auditPreviousState`.
6. After the handler (`tap`): save a success row with response as `newState`, status code and duration.
7. On error (`catchError`): save a failed row with the error message, then rethrow the error.
8. Saving is not awaited. If the audit insert fails it only logs an error.

**Read APIs.** `GET /admin/audit-logs` (filters: admin, role, action, resource, success, date range, page), `/:id`, `/admin/:adminId`, `/resource/:resource/:resourceId`. All need `audit:read`.

**Key files.** `src/admin/decorators/audit-action.decorator.ts`, `src/admin/interceptors/admin-audit.interceptor.ts`, `src/admin/audit/*`, `src/database/entities/admin-audit-log.entity.ts`, migration `1746403200000-CreateAdminAuditLogTable.ts`.

**DB.** Table `admin_audit_logs` with JSON columns `request_body`, `previous_state`, `new_state`, `metadata`. Indexes on admin id, action, resource, created_at and a composite (admin id, action, created_at). Permissions `audit:read`, `audit:export` seeded.

**Applied to.** Admin, roles, permissions, user-roles controllers; later KYC approve, lawyer basic details, pricing, update contact, status flags, dashboard approve, create package.

**Interview questions.**
- Q: Why an interceptor and not code in each service? A: It is a cross-cutting concern. One place, same format, and routes opt in with one decorator.
- Q: Interceptor vs middleware vs guard? A: Middleware runs before routing and does not know the handler. A guard only says allow/deny. An interceptor wraps the handler, so it sees both request and response.
- Q: How do you get the "before" state? A: Only the service knows it. The service attaches it to the request object and the interceptor reads it after the handler.
- Q: Why store the admin role in the row? A: Roles change later. The log must show the role at that time.

---

### 14. Logout and device de-register (Feb 2026)

**Problem.** After logout the phone still got push notifications, because its device token stayed active in comm-service.

**What was built.** `DeviceService.deregisterDevice(deviceId, userId)` in `src/internalService/otp/device.service.ts`. `AuthService.logout` calls it after revoking the refresh token.

**How it works.**
1. `POST /auth/logout` with `refreshToken` in the body and `deviceId` in a request header.
2. Find the refresh token row; 401 if missing. Mark `is_revoked = true`.
3. If a device id was sent, call comm-service `DELETE /api/devices/deregister` with `{deviceId, userId}`. The user id comes from the token row, not from the client.
4. The call returns `{success:false}` on failure instead of throwing, so logout still succeeds.

**Decision.** The device id was first a body field, then moved to a header the same day (the body DTO would otherwise need the field for all refresh-token calls).

**Interview questions.**
- Q: Why take userId from the token and not from the request? A: The client could send another user's id. The server-side value is trusted.
- Q: What if comm-service is down at logout? A: Logout still works. The device stays registered. That is a known trade-off; a retry or queue would fix it.

---

### 15. App config: constants, minimum build number, app-config table (Feb - Aug 2026)

**What was built.**
- Static config returned to the apps (`src/application/config/constants/app-constants.ts`, `config.service.ts`): documentation priority list, invoice item config, help and support block, lawyer app banners (image URL + deeplink), foreground ringtone audio URL, Haryanvi added to speaking languages.
- `minBuildNumber` in client and lawyer app config. The app compares it to its own build and forces an update. It was bumped by hand several times.
- Because a code change + deploy was needed for each bump, a DB table was built (Apr 2026): `app_configs`.

**app_configs APIs (`src/application/application.controller.ts`).**
- `POST /.../app-config` (roles admin / super_admin): body `appType` (android/ios), `appSubType` (client/lawyer), `appVersion` (semver regex), `minBuildNumber`, `maxBuildNumber`.
- `GET /.../app-config/active?appType=&appSubType=` (public): returns the active config or 404.
- `GET /.../app-config/history` (admin): all versions, newest first.

**How create works.** Inside `dataSource.transaction`: set the current active row to inactive, read the highest `version`, insert a new row with `version + 1` and `isActive = true`. So there is one active row per (platform, sub type) and full history.

**DB.** Entity `AppConfig`, migration `1777000000000-CreateAppConfigTable.ts`, two composite indexes.

**Other.** Dockerfile change: create a writable `/app/tmp` owned by the non-root user.

**Interview questions.**
- Q: Why keep old rows instead of updating one row? A: History and rollback. You can see who changed what and when.
- Q: Why a transaction? A: Without it two admins could both create an "active" row, or a failure could leave no active row.
- Q: What is a forced update check? A: The app sends or knows its build number. If it is lower than `minBuildNumber` the app blocks use and shows "update".

---

### 16. Push notification fixes (May 2026)

**Problem.** Profile and KYC notifications all used generic templates (`profile_update`, `kyc_update`) with a `status` variable. Messages were wrong or unclear.

**What was built.** Six clear template names sent from user-service to comm-service: `kyc_submitted`, `kyc_re_review`, `kyc_approved`, `kyc_rejected` (variable `reason`), `profile_basic_updated`, `profile_professional_updated`. Each has a `data.type` equal to the template so the app can route the tap. Also the `wasVerified` fix from feature 8.

**Key files.** `src/kyc/kyc.service.ts`, `src/lawyer/lawyer.service.ts`, sender `src/internalService/otp/notification.service.ts` (`sendPushNotification`).

**Interview question.**
- Q: Why one template per event instead of one template with a status variable? A: Text, translation and deep link can differ per event. It is easier to change one without breaking others.

---

### 17. Auth and JWT changes (Feb and May 2026)

- Guards added to routes that were open: admin create (`admin:create`), call quota, invoice controller, KYC controller, speaking languages, `POST /users/details`.
- `JwtStrategy.validate` now returns `user_type` from the token payload, so `req.user.user_type` is available (used by the audit interceptor and permission checks).
- UUID DTO validation relaxed from `@IsUUID(4)` to `@IsUUID('all')` in role / permission / user-role DTOs (Nov 2026), because ids created in SQL with `UUID()` are version 1 and were rejected.
- Migration `1793577600000-AddTelephonyProviderPermissions.ts` (Nov 2026): inserts permission `call_center:telephony:update` with `INSERT ... SELECT ... WHERE NOT EXISTS` so it is safe to run twice. `down()` removes role links first, then the permission.

**Interview questions.**
- Q: What does a Passport JWT strategy `validate()` return? A: The object that becomes `req.user`. Anything you need in controllers must be returned there.
- Q: What is an idempotent migration? A: Running it twice gives the same result. Here it inserts only if the row is missing.
- Q: Why did `@IsUUID(4)` fail? A: MySQL `UUID()` makes version 1 ids. Version 4 validation rejects them.

---

### 18. Update user email / phone, and "not a genuine user" flags (Sep and Nov 2026)

**Update contact: `PATCH /admin/update-user-contact`** (permission `user:contact:update`, a separate permission because phone and email are login identities).
1. DTO: `userId` UUID; `mobile_number` trimmed and matched with `/^\+?[1-9]\d{7,14}$/`; `email` trimmed, lower-cased, `@IsEmail`.
2. Need at least one field, else 400. Load user, else 404.
3. Save old values for the audit log.
4. For each changed field check no other user has it -> `ConflictException` (409).
5. If nothing really changed, return a "no changes" message.
6. Save. If the mobile changed, log the user out of all sessions (`authService.logoutAllSessions`). If that fails, log the error but keep the change.
7. Mirror only the changed fields to finance-service (`PUT /api/v1/users/update-contact`). Failure there is logged and does not fail the request.
8. Response includes `changedFields` and `sessionsRevoked`.
- Covered by a Jest spec with 12 cases (`src/admin/admin.service.updateUserContact.spec.ts`).

**Status flags: `PATCH /admin/users/status-flags`** (VS-676 in the Swagger text).
- Flags: `internal_team`, `automated_test`, `demo`, `partner_evaluation`, `just_exploring`, `research`, `duplicate_account`, `junk`, `other`. A note is required when `other` is used. Not allowed on internal users.
- Stored as one JSON column `users.user_status_flags` (`{flags, note, updated_by_user_id, updated_at}`); NULL means a genuine user. The call replaces the whole set; an empty set clears it.
- Flagged users are hidden from the admin lists by default. `flaggedOnly=true` shows them; `statusFlags=` narrows with MySQL `JSON_OVERLAPS`.
- Migration `1794355200000-AddUserStatusFlags.ts` adds the column and a multi-valued index on the JSON array.

**Interview questions.**
- Q: Why log out all sessions when the phone number changes? A: The phone number is the login identity. Old tokens belong to the old identity.
- Q: Two services store the phone number. How do you keep them in sync? A: Here it is a best-effort HTTP call after the save. It can drift if the call fails. Better: an outbox table or an event with retry.
- Q: Why JSON for flags and not a join table? A: Small fixed set, always read with the user, and "NULL = normal" keeps the common query simple. MySQL 8 can index JSON arrays.

---

### 19. Bar registration number (Oct 2026)

**What was built.**
- `lawyer.bar_registration_number` made unique. Migration `1786492800000-AddUniqueBarRegistrationNumber.ts`: turn empty strings into NULL, look for duplicates and stop with a clear error if any exist, then create the unique index.
- Helpers in `src/common/utils/bar-registration-number.ts`: `normalize` (trim, empty -> null) and `validate` (3 to 100 chars, letters, digits, `/ - .` and space).
- `resolveBarRegistrationNumber(value, userId, {revealOwner})` in `lawyer.service.ts`: checks for a clash. For a lawyer the 409 message is generic. For an admin the message names who owns it, so support can fix it.
- Admin filter by bar number (LIKE).

**Interview questions.**
- Q: How do you add a unique index to a table that already has data? A: Clean the data first (empty -> NULL, since NULLs do not clash), check for duplicates, fail early with a list, then add the index.
- Q: Why two different error messages? A: Telling a lawyer who owns a number leaks another person's data. An admin is allowed to see it.

---

### 20. Dashboard-approved lawyer flow (Oct 2026)

**Problem.** The call center wanted to route calls to lawyers who passed an internal check, without making them publicly "verified".

**What was built.**
- New columns on `users`: `dashboard_approved`, who/when approved, who/when revoked. Migration `1791448408000-AddLawyerDashboardApproval.ts` also back-fills: every already verified lawyer becomes dashboard-approved.
- `POST /admin/dashboardApproveLawyer` (`lawyer:update`, audited). `status: true` runs the same checks as verify; `false` revokes.
- Shared check `assertLawyerApprovable`: basic details milestone, professional details milestone, KYC approved, active pricing, wallet activation in finance-service. (The basic/professional checks were added to verify in Jul 2026.)
- Verifying a lawyer also sets dashboard approval.
- Admin filter `dashboardApproved`.
- A query gate (`isVerified OR isDashboardApproved`) was added to client lawyer search and then removed four days later. Final behaviour of the search gate: not confirmed beyond that removal.

**Interview questions.**
- Q: Why back-fill in the migration? A: New column defaults to false. Without back-fill all existing verified lawyers would look "not approved".
- Q: Why extract `assertLawyerApprovable`? A: Two flows need the same checks. One function keeps them the same.

---

### 21. Search relevance sorting and random lawyer ordering (Sep - Oct 2026)

**Relevance (city search).**
- Problem: typing "del" returned cities in plain A-Z order, so "Delhi" could be below a city that only contains "del" in the middle.
- Built `src/common/utils/search-relevance.util.ts`: `relevanceScore(value, key)` -> 0 if the name starts with the key, 1 if a word inside starts with it, `2 + position` for a match in the middle, a very large number for no match. `sortByRelevance` sorts by score, then name, then original position (stable).
- Used in `AuthService` city search: cities inside each group are sorted, then groups are sorted by their best city. A first try did it in SQL with `ORDER BY CASE WHEN ... LIKE 'key%'`; it was replaced by the in-code util.

**Random ordering (lawyer list).**
- Problem: the same lawyers were always on top of the list.
- Built `shuffleInPlace` (Fisher-Yates) in `src/lawyer/utils/shuffle.ts` with a unit test.
- A switch decides if shuffle is on. `LaService.getLawyerListOrderConfig()` reads it from the lawyer-appointment service, with a 2 second timeout and a 60 second Redis cache. On any error it returns "disabled" (fail safe = old static order).
- `POST` filter-lawyers response now has `meta: { listOrder: 'random' | 'static', configVersion }`.

**Other small search work.** `GET /package/by-type` (uses `ParseEnumPipe`), `GET /package/search?amount=`, `minimal` flag on `POST /users/details` to return only 5 columns (faster for call search).

**Interview questions.**
- Q: Why Fisher-Yates and not `sort(() => Math.random() - 0.5)`? A: The sort trick is biased and slower. Fisher-Yates is O(n) and every order is equally likely.
- Q: Why cache the config for 60 seconds? A: The list API is hot. One HTTP call per request to another service is slow and fragile.
- Q: What happens to pagination with random order? A: Pages can repeat or skip items between requests. Here the list is returned in one response. A seed per session would be needed for paged random.

---

### 22. Payment link over WhatsApp and call-center package (Jun - Sep 2026)

**What was built.**
- `POST /admin/create-package` (permission `call_center:admin`): creates a generic consultation package from a price in paise. Title and meta data are built on the server.
- Add-funds API extended: optional `sendWhatsapp`, `packageId`, `service`.
  1. Wallet top-up is created in finance-service; the payment link is read from the gateway response (`short_url`).
  2. If `packageId` is given, the server loads the package and checks `amount === package.pricing`, else 400. The label shown to the client is fixed on the server ("Consultation Package").
  3. If `sendWhatsapp` and a link exists, send WhatsApp template `payment_link` through comm-service. A failure returns `whatsappSent: false` and never fails the top-up.
- `src/common/utils/currency.util.ts`: `currencySymbol('INR')` and `toMajorUnit(paise)`.
- Structured logs for request, success and failure.

**Tricky bug.** The WhatsApp template placeholders were in a different order than the variable names. The keys named `amount` and `currency` carry the symbol and the number (swapped) so the final message reads correctly. This is documented in code comments.

**Interview questions.**
- Q: Why store money in paise? A: Integers avoid floating point errors.
- Q: Why does the server choose the label and check the amount? A: Never trust the client for price or message text. The client sends an id; the server owns the data.

---

### 23. Address CRUD (Aug 2026)

New `AddressModule` at `/addresses`: create, get all, by id, by user, update, soft delete (`active=false`, `isDelete=true`). DTOs with class-validator. JWT guard on write routes; auth added to the rest in a follow-up commit (exact final guard list not confirmed). Files: `src/address/*`.

---

### 24. Speaking languages and expertise at signup (Feb and Apr 2026)

- The `Language` enum in `src/database/entities/speakingLanguage.entity.ts` had only Hindi and English. It was extended to 14 Indian languages (Bengali, Telugu, Marathi, Tamil, Urdu, Gujarati, Kannada, Malayalam, Odia, Punjabi, Assamese, Maithili). Haryanvi was added in Apr 2026 to the enum and to the app constant list.
- The first step after OTP (`afterOtpVerify` section of create lawyer profile) now takes an `expertise` array. The service saves it with `upsertLawyerDetails` in the same `Promise.all` as the user update, address upsert and language rows.
- Note: the DTO marks `expertise` as required (`@IsNotEmpty`) while the Swagger text says optional. Which one is intended is not confirmed.

**Interview question.**
- Q: Why `Promise.all` here? A: The writes go to different tables and do not depend on each other. Running them together is faster. The risk: they are not in one transaction, so one can fail while others succeed.

---

### 25. Shareable lawyer profile PDF with deep link (Nov 2026)

**Problem.** Ops wanted to send a client a clean one-page profile of a lawyer before a call, without the lawyer's name or phone.

**What was built (user-service side).** `GET /admin/lawyer-profile-pdf?userId=` (permission `lawyer:read`, `ParseUUIDPipe`).

**How it works.**
1. Load user with lawyer and addresses; 404 if not a lawyer.
2. Pick the oldest active address, load speaking languages.
3. Expertise is stored as ids. Names are fetched from the lawyer-appointment service. If that fails -> 500 (a profile without expertise names is wrong).
4. Build anonymous data: `advocate_code` = `VSL` + numeric user id, experience, city, state, expertise names, languages, verified flag.
5. Deep link (`getLawyerConnectLink`): destination = app open URL (env `APP_OPEN_URL`) + screen + lawyer id. If the same destination is already saved in `lawyer.metadata.profileConnectLink`, reuse it. Else ask comm-service `POST /api/v1/deep-link/create` (10 s timeout) and save the result with MySQL `JSON_SET` so other metadata keys are not overwritten.
6. Call doc-service `POST /generate/sync/lawyerProfile` (20 s timeout). Check the response has an http(s) `document_url`.
7. Return `{ url, advocateCode, fileName }`.

**Key files.** `src/admin/admin.service.ts` (`getLawyerProfilePdf`, `getLawyerConnectLink`), `src/internalService/doc/doc.service.ts`, `src/internalService/otp/notification.service.ts` (`createDeepLink`), spec `src/admin/admin.service.getLawyerProfilePdf.spec.ts`.

**Interview questions.**
- Q: Why `JSON_SET` and not read-modify-write of the JSON? A: Two requests could overwrite each other's keys. `JSON_SET` changes one key inside the database in one statement.
- Q: Why cache the deep link on the lawyer row? A: Each create call makes a new short link at the provider. Reuse keeps one link per lawyer and saves calls.
- Q: Why timeouts on `fetch`? A: Without one, a slow service holds the request forever and ties up the server.

---

## Communication Service (comm-service)

### What this service does

- Central notification service (package name `central-notification-service`).
- Sends push notifications (Android via Firebase, iOS via APNs), SMS, WhatsApp and email.
- Stores each user's devices and push tokens, and logs every notification with status.
- Other services call it over HTTP with a template name and variables.
- Also creates short deep links through an external link provider.

### Tech stack and packages

| Package | What it is | Where it is used here |
|---|---|---|
| `express`, `body-parser` | Web framework (plain JavaScript, CommonJS) | `app.js`, `src/api/routes/*`, `src/api/controllers/*` |
| `sequelize`, `mysql2` | ORM and MySQL driver | `src/database/index.js` (`UserDevice` model, device queries) |
| `firebase-admin` | Firebase SDK for Android push (FCM) | `src/adapters/providers/pushProvider.js` |
| `@parse/node-apn` | Apple push (APNs) client | `src/adapters/providers/pushProvider.js` |
| `joi` | Request validation | `src/validations/notification.validation.js` |
| `axios` | HTTP client | External provider calls, e.g. the deep link adapter |
| `winston` | Logging | `src/config/winston`, used in adapters and services |
| `uuid` | Id generation | `src/api/controllers/deviceController.js` |
| `handlebars`, `ejs`, `nodemailer`, `node-cron`, `googleapis` | Templates, email, retry jobs | Used by the service; not touched by my commits |
| `jest`, `supertest` | Tests | `tests/unit/*`, e.g. `tests/unit/chottuLinkAdapter.test.js` |

### 1. Call push goes only to the latest device (Feb 2026)

**Problem.** A user logged in on two phones got the incoming-call push on both. Old phones rang.

**What was built.** In `src/adapters/pushAdapter.js`, `sendPushNotification` reads `variable.data.type`. If it is `call`, it sends only to the first (latest) device in the list. Other notification types still go to all devices.

**How it works.**
1. Load devices for the user ids where `isActive`, `isTokenValid` and `pushNotificationEnabled` are all true, newest first.
2. No device -> mark the notification log `FAILED` and return an error.
3. `type === 'call'` -> keep only index 0. Else keep all.
4. Send each through the push provider factory.

**Notes.** First version built a `Map` to get the latest device per user; it was simplified to "first row" (correct only for a single recipient, which is the call case). The sort column was changed from `lastActiveAt` to `createdAt` in this commit; current code sorts by `updatedAt` and has more logic (changed later by others).

**Validation fix.** Joi schema: `phoneNumber`, `senderEmail`, `receiverEmail` now accept an empty string (`.optional().allow('')`). Callers that sent `""` were getting a 400.

**Interview questions.**
- Q: Why does `Joi.string()` reject `""`? A: By default Joi treats an empty string as invalid. You must add `.allow('')`.
- Q: Why send calls to one device only? A: A call is real-time. Ringing an old phone confuses the user and the call state.

### 2. De-register device API (Feb and Apr 2026)

**Problem.** Needed by the logout flow in user-service (feature 14 above).

**What was built.** `DELETE /api/devices/deregister`, body `{ deviceId, userId }`.

**How it works.**
1. Route in `src/api/routes/deviceRoutes.js` -> `deviceController.deregisterDevice`.
2. Both fields required, else `AppError` 400.
3. `src/services/device.service.js` -> `db.deactivateDeviceByAppDeviceId(appDeviceId, userId)`.
4. Find the `UserDevice` row for that device and user. Not found -> 404.
5. Update `isActive = false`, `isTokenValid = false`. The row is kept.
6. Respond with the device ids and the new flags.

**Bug fixed (Apr 2026).** The lookup used the DB column `deviceId` (a server-side id). The app actually sends its own id, stored in `appDeviceId`. So nothing matched and logout returned 404. The lookup was changed to `appDeviceId`. The request field name is still `deviceId`.

**Interview questions.**
- Q: Why check userId together with the device id? A: So one user cannot de-register another user's device.
- Q: Why deactivate and not delete? A: The row is reused when the same phone logs in again, and it keeps history.

### 3. Latest user device API (May 2026)

`GET /api/devices/user/:userId/latest?appType=`. Returns the newest active device (`order by lastActiveAt desc, updatedAt desc`) with fields like device type, app version, OS version, token validity. Returns `data: null` with success when there is none. Used so other services / dashboard can know the user's app version and platform (related to the registration source and "has app login" work; exact caller not confirmed).

Route order matters: `/user/:userId/latest` is registered before `/user/:userId`.

### 4. Deep link fix (Nov 2026)

The deep link adapter read the link from `response.data.data.link`. The provider really returns it in `short_url` with status 201. So every create returned "no link". Fixed to read `short_url`, with a 122-line unit test. The default link domain was also moved into config so the env variable is optional (`CHOTTU_LINK_DOMAIN`). This commit is on the VS-672 feature branch and a November release branch; the adapter itself was written by someone else.

**Interview question.**
- Q: How did you find it? A: The PDF feature got `null` links. Reading the real provider response showed the field name was different. A test with the real response shape now protects it.

---

## Document Service (doc-service)

### What this service does

- A Python FastAPI service that makes documents.
- Renders HTML templates (Jinja2) to PDF with WeasyPrint and uploads them to S3.
- Document types are plug-in "modules": invoice, lawyer invoice, html-to-pdf, lawyer profile.
- Can run sync (`/generate/sync/{type}`) or async with Celery (`/generate/async/{type}` + `/status/{task_id}`).
- Also makes calendar invite (`.ics`) files.

### Tech stack and packages (from requirements.txt)

| Package | What it is | Where it is used here |
|---|---|---|
| `fastapi`, `uvicorn` | Async web framework and server | `app/main.py` |
| `pydantic`, `pydantic-settings` | Data validation and settings | Request models in `app/main.py`, `app/modules/lawyerProfile/schemas.py`, `app/core/config.py` |
| `jinja2` | HTML templates | `app/services/renderer.py`, `app/modules/lawyerProfile/templates/lawyer_profile.html` |
| `weasyprint` (>= 62) | HTML/CSS to PDF | `app/services/renderer.py`. Version 62 is needed for CSS grid used by the profile template |
| `boto3`, `botocore` | AWS SDK | `app/services/s3.py` (`put_object`, AES256 encryption at rest) |
| `ics` | Calendar file library | `app/main.py` (`Calendar`, `Event`, `DisplayAlarm`) |
| `sqlalchemy` (async), `aiomysql` / `asyncmy` | ORM and MySQL drivers | `app/core/db.py`, `app/models/stored_document.py` |
| `celery`, `redis` | Background task queue | `app/workers/*` (existing; not changed by me) |
| `pypdf`, `Pillow`, `num2words` | PDF merge, images, amount in words | Other modules (not my work) |
| `pytest`, `httpx` | Testing | Listed; no tests by me found in this repo |

### 1. Generate ICS (calendar invite) API (Jan 2026)

**Problem.** After an appointment is booked, client and lawyer should be able to add it to their phone calendar.

**What was built.** `POST /generate-ics` in `app/main.py`.

**How it works.**
1. Body (`ICSRequest`, Pydantic): `start_time`, `end_time` (ISO 8601), organizer name/email, attendee name/email; optional `title` (default "Consultation Meeting"), `description`, `location`, `reminder_minutes` (default 30).
2. Create `Calendar` and `Event`. `event.uid = uuid4`. Times parsed with `datetime.fromisoformat` after replacing a trailing `Z` with `+00:00`.
3. Set organizer and add the attendee.
4. Add a `DisplayAlarm` that fires `reminder_minutes` before the start.
5. Turn the calendar into text, encode to UTF-8 bytes.
6. Upload to S3 with content type `text/calendar` into a separate `calendars/` folder. `upload_file` got a new `folder` parameter for this (default stays `documents`).
7. Response: `success`, `event_uid`, `s3_url`, `message`.
8. Errors: bad date -> 400; anything else -> 500.

**History.** Three commits. The first returned the ICS text and a download path; the second moved to S3 and a URL; the third is the final version. The first commit also shows about 70 files changed with equal adds and deletes: that is a line-ending change, not real code change.

**Interview questions.**
- Q: What is an ICS file? A: A plain text calendar format (iCalendar). Calendar apps open it and create the event.
- Q: Why replace `Z` with `+00:00`? A: Older Python `fromisoformat` does not accept `Z`. Both mean UTC.
- Q: Why upload to S3 instead of returning the file? A: Other services need a link they can put in an email or WhatsApp message.
- Q: Why does the UID matter? A: The calendar app uses it to know if an event is new or an update of an old one.

### 2. Lawyer profile PDF module (Nov 2026)

**Problem.** See user-service feature 25. This is the part that draws the PDF.

**What was built.** New module `app/modules/lawyerProfile/` registered in `app/modules/factory.py` as `lawyerProfile`. It is called through the existing `POST /generate/sync/lawyerProfile`.

**How it works.**
1. `LawyerProfileSchema` validates input. On purpose there is no name, phone, email or bar number in the schema, so the PDF cannot leak them. `advocate_code` must match `^[A-Za-z0-9-]+$`; `connect_url` must be `https://`.
2. `process_data` builds the template context: location text (falls back to "India"), tidy lists (collapse spaces, title-case single-case text, drop blanks and case-insensitive duplicates), experience label (`<1 year`, `1+ year`, `12+ years`), `compact` and `dense` layout flags.
3. Fonts (DM Sans) and logo are bundled in the repo and passed as `file://` URIs so WeasyPrint needs no network.
4. Jinja2 renders `lawyer_profile.html`. The Connect button is a link only when `connect_url` is present.
5. WeasyPrint makes the PDF, it goes to S3, the URL is returned as `document_url`.

**Key files.** `app/modules/lawyerProfile/logic.py`, `schemas.py`, `templates/lawyer_profile.html`, `assets/`, `app/modules/base.py`, `app/services/generator.py`, `app/services/renderer.py`, `app/services/s3.py`.

### 3. "Show more" removed: page grows to fit content (Nov 2026)

**Problem.** The first design was a fixed A5 page. Long lists were cut to 8 expertise areas and 7 languages with a "+N more" line. Business wanted every detail shown.

**What was built.** A page that is A5 wide and at least A5 tall, and gets taller when needed. A complex "row budget" calculation (about 60 lines) was deleted.

**How it works (`html_to_fitted_pdf` in `app/services/renderer.py`).**
1. The template ends with an empty marker `<div id="page-fit-end">`.
2. Pass 1: inject an `@page` rule with a very tall page (5000 mm) and call `weasyprint.HTML(...).render()`.
3. Read the Y position of the marker from `pages[0].anchors`. Convert CSS px to mm (`25.4 / 96`). If the marker is missing or there is more than one page, raise an error.
4. Page height = max(210 mm, content height).
5. Pass 2: inject an `@page` rule with that exact size and write the PDF.
- A module opts in by returning `(width_mm, min_height_mm)` from the new hook `get_fitted_page()` in the base class. Other document types are not affected.
- More than 12 expertise areas -> three columns ("dense").

**Interview questions.**
- Q: Why render twice? A: You cannot know the height of HTML before layout. First measure, then render at the right size.
- Q: Why a hook in the base class? A: Open/closed principle. Invoice modules keep their behaviour; only modules that return a value get the new path.

### 4. Reuse the same file if nothing changed: fingerprint + stored_documents (Nov 2026)

**Problem.** Every click on "share profile" made a new PDF and a new S3 file. Slow, wasteful, and many links for the same profile.

**What was built.**
- New hook `get_storage_key()` in the base module. The profile module returns `lawyer-profiles/advocate_profile_<CODE>.pdf`.
- New table `stored_documents` (model `app/models/stored_document.py`, DDL in `init.sql`): `storage_key` (primary key), `document_type`, `fingerprint` (64 chars), `s3_url`, `data` (JSON), timestamps.
- `_generate_at_fixed_key` in `app/services/generator.py`.

**How it works.**
1. Render the template to HTML (cheap).
2. `fingerprint = sha256(html)`. It changes if the data changes or the template changes.
3. Load the row by `storage_key`. If the fingerprint is the same and the stored URL starts with the current public base URL -> return the stored URL. No PDF render, no upload.
4. Else render the PDF and upload it under a versioned key: the stable key plus a UTC timestamp before `.pdf`.
5. Upload with `Cache-Control: public, max-age=31536000, immutable`.
6. Only after a successful upload, insert or update the row. If the DB write fails, log a warning and still return the URL (next call just renders again).
7. No local-file fallback on this path (the normal path saves locally if S3 fails).

**Design history (3 commits in one week).**
- First: overwrite one fixed S3 key, and compare by reading S3.
- Second: compare using a DB row instead of S3.
- Final: never overwrite. Each change gets a new timestamped key. Reason: a CDN was put in front of the bucket (new setting `PUBLIC_FILES_BASE_URL`, `object_url()` in `s3.py`). Overwriting a key behind a CDN serves stale files until an invalidation. With immutable versioned keys, each URL always returns the same bytes, so no invalidation is ever needed, and links already shared keep working.

**Interview questions.**
- Q: Why hash the HTML and not the input JSON? A: The HTML includes the template. If the design changes, the hash changes too and the PDF is rebuilt.
- Q: What is cache busting / immutable assets? A: Put a version in the file name and cache it forever. A change means a new name, so caches never serve old content.
- Q: Why write the DB row after the upload? A: So a row always points to a file that exists. The other order could leave a row with a dead link.
- Q: Is there a race if two requests come together? A: Yes, both can render and upload. The result is two valid files and the last write wins the row. It is harmless here but could be closed with a lock.

### 5. Deep link in the PDF (Nov 2026)

`connect_url` added to the schema, the context and the template (the Connect button). It is optional; without it the button prints with no link. The link is created by user-service through comm-service (see above).

---

## Things not confirmed

- Production status of each commit (many are on feature or dev branches).
- Why the 18+ age rule and name length limit were removed (Jan 2026).
- Whether `call_quota` has a DB unique index on (user, date).
- Final state of the dashboard-approval gate in client lawyer search after its removal.
- Final guard list on the address read routes.
- Who calls the "latest device" API.
- The registration-source work in user-service is a plan document plus an admin filter; the code that writes `registration_source` at signup was not found in my commits.
- comm-service: the current release branch has later changes by others in the push adapter; only my own diffs are described.

---

# Part 5: Admin Panel

## Admin Panel (admin-panel, frontend)

Source: 192 non-merge commits by me in the `admin-panel` repo, from 16 Mar 2026 to 18 Nov 2026.
Everything below was checked in the commit diffs or in the code. Where something could not be checked, it says "not confirmed".

What the admin panel is: an internal web dashboard. Three kinds of people use it.
- Call-center agents. They pick up client calls, find a lawyer, book a consultation, and write a "disposition" (the result of the call).
- Call-center managers. They watch call logs, agent work, and exports.
- Operations / admin staff. They manage lawyers, clients, courts, cities, users, roles, and wallets.

Branch status (checked with `git merge-base`):
- Work up to mid Sep 2026 is on `main`.
- Later work is on `dev-main` and the release branch `release-171126`.
- The Bonvoice telephony UI (VS-647) is on `dev-main` only. It is not in `release-171126`.
- The consultation stage fix (VS-688) and the last scroll fix for the flags picker are on their feature branches only.

---

### Tech stack and packages in admin-panel

Read from `package.json` and from the imports in the files I changed.

| Package | What it is | Where it is used in this repo |
|---|---|---|
| `next` 15.4.4 | React framework. **App Router** (the `app/` folder). | Pages are in `app/dashboard/**/page.tsx`. Server routes are in `app/api/**/route.ts`. Dev server uses Turbopack. |
| `react` / `react-dom` 19.1 | UI library. | Every component. Most pages are client components (`"use client"`). |
| `typescript` 5.8 | Typed JavaScript. | Whole repo. Types live in `lib/types`, `lib/api/types`, `types/`. |
| `tailwindcss` 4 + `tailwind-merge` + `clsx` + `class-variance-authority` | Utility CSS and helpers to join class names. | All styling. `cn()` in `lib/utils.ts`. |
| `@radix-ui/react-*` (dialog, popover, select, tabs, tooltip, switch, accordion, checkbox, dropdown-menu, progress ...) | Headless, accessible UI parts. Wrapped shadcn-style in `components/ui/`. | Modals, dropdowns, tabs, tooltips everywhere. I added `components/ui/accordion.tsx` for the KYC section. |
| `cmdk` | Command / combobox list. | City picker in the court form, flag picker in `UserStatusFlagsEditor`. |
| `lucide-react`, `@tabler/icons-react` | Icon sets. | Buttons and badges. |
| `sonner` | Toast messages. | Success / error toasts. The toaster position was moved to bottom-left in `app/layout.tsx`. |
| `swr` (+ `swr/infinite`) | Data fetching with cache and revalidation. | Lawyers table, clients table, work queue V2 (`hooks/use-work-queue-v2.ts`), calendar, task reminders. |
| `@tanstack/react-query` | Another data-fetching library. | `providers/QueryProvider.tsx` wraps the app. Used by the court / city hooks (`hooks/court-management`). my own new hooks mostly use SWR. |
| `@tanstack/react-table` | Headless table logic (columns, sorting). | `components/lawyers-data-table.tsx`, `components/clients-data-table.tsx`, court and city tables. |
| `zustand` (+ `persist` middleware) | Small global state store. | `lib/stores/auth-store.ts` (permissions, roles, user) and `lib/stores/call-center-store.ts` (incoming call, call workflow). |
| `socket.io-client` | Real-time WebSocket client. | `lib/socket/call-center-socket.ts`, `hooks/use-call-center-socket.ts`. Incoming calls, AI call events. |
| `react-hook-form` | Form state and validation. | Court form and city form modals. |
| `zod` 4 | Schema validation. | Row schemas in the lawyers and clients tables (`z.object(...)`). |
| `date-fns` 4 | Date helpers. | Calendar grid (`startOfWeek`, `eachDayOfInterval`), filters (`startOfDay`, `endOfMinute`, `subDays`). |
| `react-datepicker` | Date and time picker. | `components/ui/date-range-picker.tsx`, used in the call-logs filters. Custom CSS in `app/globals-datepicker.css`. |
| `react-day-picker` | Calendar date picker. | In the UI kit. Not confirmed in my own files. |
| `jwt-decode` | Reads a JWT in the browser. | `lib/jwt.ts`. I use `decodeAccessToken()` to get the current user id for KYC approval. |
| `recharts` | Charts. | Dashboard and agent performance charts. Not touched by my commits (not confirmed). |
| `xlsx`, `papaparse`, `file-saver` | Excel / CSV libraries. | Installed. My exports do **not** use them. The exports build CSV text by hand and download it with a `Blob`. |
| `libphonenumber-js` | Phone number parsing. | `lib/phone-utils.ts`. Not confirmed in my commits. |
| `@exotel-npm-dev/exotel-ip-calling-crm-websdk` | Browser VoIP SDK of the telephony provider. | `lib/voip`. Not built by me. |
| `@openreplay/tracker` | Session replay. | Provider in the layout. Not built by me. |
| `@playwright/test` | End-to-end tests. | `e2e/`. Not confirmed in my commits. |
| Maps | There is **no** maps package. | "View map" opens a Google Maps search URL in a new tab. |

How the app is put together (short):
1. **Folders.** `app/dashboard/*` has the pages. `components/*` has UI (most call-center UI is in `components/call-center`). `hooks/*` has data hooks. `lib/api/services/*` has service files that call the API. `lib/stores/*` has Zustand stores. `app/api/*` has the server-side proxy routes.
2. **Auth.** Login gives an access token and a refresh token. They are kept in `localStorage` (not cookies). `lib/auth/token-manager.ts` is the single source of truth. `providers/PermissionProvider.tsx` runs at startup, refreshes an expired token, and fills the auth store. `app/dashboard/AuthCheck.tsx` wraps each page and sends the user to `/` if not logged in.
3. **API calls go through a proxy (BFF).** The browser never calls the backend directly. It calls same-origin routes like `/api/proxy/...` or `/api/call-center/...`. Each `route.ts` reads the `Authorization` header and forwards the request to the API gateway with `buildApiUrl(path)` from `lib/api-config.ts` (base URL comes from the `API_BASE_URL` env variable). Gateway paths start with a service prefix, for example `/g/us/...` (user-service) and `/g/cc/...` (call-center).
4. **Two HTTP helpers.** `apiClient` (`lib/api-client.ts`) and `authorizedFetch` (`lib/auth/authorized-fetch.ts`). Both add the Bearer token and, on a 401, refresh the token once and retry. My early commits used plain `fetch` with a token from `localStorage`; later commits use these helpers.
5. **Permissions (PBAC).** Permission strings look like `resource:action` (for example `lawyer:update`). They travel inside the JWT. `lib/permissions.ts` lists them. `hooks/use-permissions.ts` gives `useHasPermission`, `useHasAnyPermission`, and `usePermissionGate`. `lib/module-permissions.ts` maps pages to permission gates, and the sidebar uses it.
6. **Important rule.** Hiding a button is only UI. The backend checks the same permission string again. A new permission does nothing until it is granted to a role and the user logs in again (because it is in the JWT).
7. **Gateway query params.** The gateway only passes query params that are on its allow list. A new filter needs three changes: backend service, gateway config, and the frontend caller. Array params are sent as repeated keys (`x=1&x=2`), never `x[]=1`.

---

### 1. Manager call-logs page: fast list, real stats, filters (Mar - Aug 2026)

**Problem.** The manager call-logs page was slow and the numbers were wrong. To show stats (total calls, missed calls, average time) it downloaded up to 1000 call rows and counted them in the browser. So stats were only right for the first 1000 rows.

**What was built.**
- A first attempt (27 Mar) loaded 20 rows fast and then 1000 rows in the background as a cache, with client-side paging and "stale response" checks. This was then replaced.
- The final design (31 Mar - 1 Apr) uses **two separate API calls**: one for the page of rows, one for the stats. A new proxy route `app/api/call-center/manager/call-logs-stats/route.ts` forwards to the backend stats endpoint.
- Stats are fetched only when filters change, not when the page number changes.
- Filters added over time: agent, call type, disposition category, disposition sub-category (the sub-category list depends on the chosen category), and a multi-select call status filter with groups (Aug).
- Stats cards were redone in Aug into two rows: a "success" row and a "needs attention" row (short calls, no answer, busy, cancelled). Talk time is shown as hours and minutes.
- Time filtering (Oct, VS-644): "Last 1 / 2 / 4 hours" buttons and a date picker that also lets you pick a time.

**How it works.**
- Page: `app/dashboard/call-center/manager/call-logs/page.tsx`. State is plain `useState`: `filters`, `currentPage`, `itemsPerPage`, `stats`, `callLogs`.
- `fetchCallLogs` and `fetchStats` are `useCallback` functions. Two `useEffect` hooks call them. One depends on filters + page, the other only on filters.
- Service: `lib/api/services/call-center/call-logs-service.ts` (`getAllCallLogs`, `getCallLogsStats`). It builds the query string and skips empty or `all` values.
- Filters component: `components/call-center/manager/CallLogsFilters.tsx`.
- With no date filter, the backend defaults to "today". The frontend first computed "today" itself, then that code was removed so both calls agree.
- For the time filter, "day" ranges are widened to whole days (`startOfDay` / `endOfDay`). "Exact" ranges (hour buttons or a picked time) are widened to whole minutes (`startOfMinute` / `endOfMinute`).

**Key files.** `app/dashboard/call-center/manager/call-logs/page.tsx`, `components/call-center/manager/CallLogsFilters.tsx`, `lib/api/services/call-center/call-logs-service.ts`, `lib/api/types/call-center.types.ts`, `app/api/call-center/manager/call-logs-stats/route.ts`, `components/ui/date-range-picker.tsx`, `app/globals-datepicker.css`.

**Packages.** `date-fns` (date math), `react-datepicker` (date + time picker), `sonner` (toasts), Radix select / popover.

**Tricky parts.**
- Agent filter bug: the dropdown sent `agent.user_id` but the API filters on the agent row id. Fixed by sending `agent.id` as `agent_id`.
- "Reset" bug: reset used to call both APIs by hand and the effects called them again. Fixed by letting the effects be the only place that fetches.
- Changing "items per page" must reset to page 1 (commit "setCurrentPage=1"), or the page can point past the end.
- The date + time picker popup wrapped the time list under the calendar. Fixed with CSS that uses two class names so it beats the library's own CSS.

**Interview questions.**
1. *Why did you move stats to the backend?* Counting in the browser only sees the rows you downloaded. The backend can count all rows with one SQL query. It is correct and much faster.
2. *Why two API calls and not one?* Stats change only when filters change. Rows change when the page changes too. Splitting them avoids re-counting stats on every page click.
3. *How do you stop a stale response from showing?* In the first version I saved the filters as a string before the request and compared after. If they changed, I threw the response away. Now the effects depend on the filters, so the latest call wins.
4. *What is a dependent dropdown?* The sub-category options come from a map keyed by category. When the category changes, the sub-category is cleared.

---

### 2. CSV export: all rows, in pages of 2000, with progress, cancel, and masked numbers (Apr - Sep 2026)

**Problem.** Export only saved the current page. A later quick fix asked for `limit=100000` in one request, which is slow and can time out. Also, exports with real phone numbers were being shared outside the team.

**What was built.**
- Export buttons on three pages: call logs, lawyers, clients. Each is behind its own permission.
- A shared helper `lib/utils/csv-export.ts` (`exportToCSV(data, columns, filename)`) for the lawyers and clients tables.
- For call logs: `CallLogsService.exportAllCallLogs()` fetches **2000 rows per page, 3 pages at a time**, shows a progress dialog ("X of Y rows fetched"), and has a Cancel button.
- Extra columns: disposition category, sub-category, resolution status, customer name, city, state, notes, disposition notes, call category.
- A dropdown with two choices: "Export" (full numbers) and "Export with masked numbers" (only the last 4 digits are shown).

**How it works.**
- Page 1 is fetched first. It tells the total rows and total pages.
- A small worker pool is made with `Promise.all` over 3 worker functions. Each worker takes the next page number from a shared counter.
- Each page's CSV lines are stored in an array slot by page number. So the file order is right even if page 5 finishes before page 4.
- One `AbortController` is used for the whole run. The Cancel button aborts it. If any page fails (after one retry), the run is aborted too, so no half file is written.
- `date_to` is pinned to "now" at the start, and the sort order is fixed, so new calls arriving during the export do not shift the pages.
- CSV cells follow RFC 4180: wrap in quotes and double any quote inside. A BOM is added at the start so Excel reads UTF-8.
- Masking: keep digits only, replace all but the last 4 with `X`. It matches the way the backend masks numbers.
- Download: build a `Blob`, make an object URL, click a hidden `<a download>`.

**Permission checks.** `call_logs:export`, `lawyer:export`, `client:export`, and `recording:export` (see feature 3). Defined in `lib/permissions.ts` under `EXPORT`.

**Key files.** `lib/api/services/call-center/call-logs-service.ts`, `lib/utils/csv-export.ts`, `app/dashboard/call-center/manager/call-logs/page.tsx`, `components/lawyers-data-table.tsx`, `components/clients-data-table.tsx`.

**Packages.** None for CSV (hand-written). Radix `Dialog`, `Progress`, `DropdownMenu` for the UI.

**Tricky parts.**
- A `"` inside an agent name shifted all later columns in that row. Fixed by proper quoting.
- New lines and `|` inside notes broke Excel rows. Fixed in the same quoting work (commit "update the new line and pipe in excel").
- The lawyers export first read a `kycStatus` field that did not exist. It now derives the status from `kycRecords` (approved / rejected / pending / not submitted).

**Interview questions.**
1. *Why 2000 by 2000 and not one big request?* One big request is slow, uses a lot of memory on the server, and can time out. Small pages are safe, and I can show progress and let the user cancel.
2. *How do you keep the rows in order when pages load in parallel?* I store each page's lines at index `page - 1` and join them at the end.
3. *How does cancel work?* I pass an `AbortSignal` to every `fetch`. Calling `abort()` stops all running requests and the code throws an `AbortError`, which I show as "Export cancelled".
4. *Why mask on the client?* The same data is already on the screen for this user. Masking is a choice for a file that will leave the team, so it is done when the file is built.

---

### 3. Permission-based hiding of details (Apr - Aug 2026)

**Problem.** Not every user should hear recordings, export data, see call details, or act on another agent's work.

**What was built.**
- Recording play buttons are shown only with `recording:export`. This was applied in four places: manager call logs, the agent's call-logs section, the client journey modal, and the call-log details modal. The raw recording URL text was replaced with a "Play Recording" button.
- When the recording column is hidden, the row grid uses one less column so the table still lines up.
- The "View details" button on the manager call-logs page needs `call_logs:export`.
- The main `/dashboard` page is now gated like the Lawyers page. A user without that permission is redirected to the first sidebar page they can open (`firstAccessibleNavUrl()` in `components/app-sidebar.tsx`).
- In the work-queue calendar, looking at another agent's items is read-only (see feature 9).

**How it works.** Each component calls `useHasPermission(PERMISSIONS.X.Y)` and renders the control only if it is true. The hook reads the permission list from the Zustand auth store, which is filled from the JWT.

**Key files.** `lib/permissions.ts`, `lib/module-permissions.ts`, `hooks/use-permissions.ts`, `components/call-center/CallLogsSection.tsx`, `components/call-center/ClientJourneyModal.tsx`, `components/call-center/call-log-details-modal.tsx`, `app/dashboard/page.tsx`, `components/app-sidebar.tsx`.

**Interview questions.**
1. *Is hiding a button enough for security?* No. It is only for a clean UI. The backend checks the same permission on every endpoint.
2. *Where do the permissions come from?* From the JWT. After login they are decoded and kept in a Zustand store. Hooks read from that store.
3. *What happens if I add a new permission?* It must be granted to roles in the database, and users must log in again to get a new token.

---

### 4. Lawyer details modal: expertise edit, KYC, pricing, full profile (Mar - Apr 2026)

**Problem.** Staff could only read a lawyer's profile. To fix data or approve KYC they needed a developer or another tool.

**What was built** (all inside `components/call-center/LawyerDetailsModal.tsx`):
- **Edit areas of expertise.** An Edit button shows a checkbox list of all expertise areas. Save sends a `PATCH`.
- **KYC section.** Loads the lawyer's KYC when the modal opens. Shows ID proof front and back, PAN, bank details, cancelled cheque, and court orders, each with a View button. Approve, Reject (with a reason), and later **Revoke approval** buttons.
- **Pricing label.** Shows "Rs X / Call" and the package title, or "Pricing not set".
- **Full profile.** About me, addresses, career, education, case highlights. Only active and not-deleted items are shown.
- Registration source and "latest device" info (May).
- Verified toggle fix and KYC date detail.

**How it works.**
- State is local `useState` (for example `kycData`, `isLoadingKyc`, `showRejectDialog`, `pricingData`).
- A `useEffect` on `[lawyer.user.id, isOpen]` fetches KYC and pricing.
- New proxy routes: `app/api/proxy/kyc/route.ts`, `kyc/pending`, `kyc/user`, `kyc/approve`, `app/api/proxy/lawyers/lawyer-pricing/route.ts`, `app/api/proxy/devices/latest/route.ts`, and a `PATCH` handler in `app/api/user-management/lawyers/route.ts`.
- Approve sends `{ isApproved, approvedBy, rejectionReason }`. `approvedBy` is the current user id, read from the JWT with `decodeAccessToken()`.
- Revoke uses the same approve endpoint with `isApproved: false` and a fixed reason.
- Prices are stored in paise. The UI divides by 100 to show rupees.
- KYC document values can be a plain URL string or an object `{ url, name }`. Helper functions in `types/kyc.types.ts` (`parseKycDocuments`, `getDocumentUrl`) handle both.

**Permission checks.** `lawyer:update` for the expertise edit and verify toggle. `kyc:approve` for approve / reject / revoke.

**Key files.** `components/call-center/LawyerDetailsModal.tsx`, `types/kyc.types.ts`, `components/ui/accordion.tsx`, `lib/api-client.ts` (`getKycByUserId`, `approveKyc`), the proxy routes above.

**Tricky parts.**
- "Verified button" bug: a `useEffect` reset the local verified state every time the parent re-rendered, so a fresh toggle could jump back. Fixed by depending only on `lawyer.user.id` (reset only when a different lawyer is opened).
- "KYC not found" is normal for a lawyer who has not submitted yet. The code treats that error as "no KYC", not as a failure.
- Stale modal data: the modal shows a snapshot taken at row click. After an edit, the table refetches, and an effect re-selects the same lawyer from the fresh data so the open modal updates (commit 12 Aug).

**Interview questions.**
1. *Why did the verified toggle jump back?* The effect depended on the whole `lawyer` object. Every refetch made a new object and reset my local state. I changed the dependency to the lawyer id.
2. *Why store money in paise?* Whole numbers avoid floating point errors. Convert to rupees only when showing.
3. *How do you handle two shapes of the same field?* A small helper checks `typeof value === 'string'` and returns the URL in both cases, so the JSX stays simple.

---

### 5. Lawyers page and Clients page filters (Apr - Nov 2026)

**Problem.** Staff could not find lawyers by city, court, onboarding step, or verification state. Free-text city and state boxes gave bad matches.

**What was built.**
- **City search with suggestions.** `components/ui/city-search-input.tsx`: a text box with a dropdown of city + state suggestions, debounced by 300 ms, keyboard navigation, and close on outside click.
- **Court search with suggestions.** `components/ui/court-search-input.tsx`, same pattern.
- **Filter only on select or blur.** The table keeps two values: what the user is typing, and the "committed" value. The API is called only with the committed value.
- **Expertise filter** fixed (correct param name) and the old expertise text search removed. Expertise chips wrap to new lines.
- **Checkbox filters on lawyers:** no court assigned, basic details completed, professional details completed, verifiable, verified only, show OTP-only registered, dashboard approved, altered profile (needs re-verification). Plus onboarding source and bar registration number.
- **Clients page:** city suggestions (state box removed), and a **"Show clients with no name"** checkbox (`noName=true`).
- New columns: Source, Dashboard (approved), Verified. Column order was changed so the status columns are on the left.
- A Refresh button and a Retry button on the error screen.
- An (i) help popover next to "Reset Filters" that explains: all filters are AND, values inside one multi-select are OR (`components/user-management/FilterHelpPopover.tsx`).

**How it works.**
- Tables: `components/lawyers-data-table.tsx` and `components/clients-data-table.tsx`. They use TanStack Table for columns and sorting.
- Data: `useSWR` with a key that holds the filters. Changing a filter changes the key, so SWR refetches. `keepPreviousData` keeps old rows on screen during a refetch.
- Filters live in separate `useState` values. A `useEffect` copies them into one `filters` object and resets the page to 1.
- Row data is described with a `zod` schema. The schema was widened (many fields `.optional()` / `.nullable()`) to match the real API response.
- Suggestion proxy routes: `app/api/location/search-cities/route.ts`, `app/api/location/search-courts/route.ts`, `app/api/location/search-lawyer-cities/route.ts`.

**Packages.** `swr`, `@tanstack/react-table`, `zod`, Radix checkbox / select.

**Tricky parts.**
- Typing in the city box used to call the API on every key. The "committed value" fix removed these calls.
- The refresh button looked broken: with `keepPreviousData`, SWR's `isLoading` stays false on a refetch. The fix uses `isValidating` to show the spinner.
- A strict zod schema rejected rows when the backend added or dropped fields. Fields were made optional.

**Interview questions.**
1. *What is debouncing and why use it?* Wait a short time after the last key press before calling the API. It saves requests and avoids flicker.
2. *How does SWR know when to refetch?* By the key. I put the filters in the key. A new key means a new request, and the old result stays cached.
3. *Why keep a typed value and a committed value?* So the list does not reload on every letter. It reloads when the user picks a suggestion or leaves the box.
4. *`isLoading` vs `isValidating` in SWR?* `isLoading` is true only for the first load with no data. `isValidating` is true for any request, including a refresh.

---

### 6. Courts and cities: view-map button, required coordinates, better court form (Apr 2026)

**Problem.** Staff had no quick way to check where a city or court is. Cities without latitude / longitude broke location features. Adding a court for a new city took several steps.

**What was built.**
- **View map button** in the cities and courts tables. It opens Google Maps in a new tab.
- Latitude and longitude are now **required** in the city form, with range checks (-90..90 and -180..180).
- `components/court-management/court-form-modal-enhanced.tsx`: pick a state, type a district, get city suggestions for that state. If the city is new, latitude / longitude fields appear. The backend then creates the court, the city, and the mapping together. The current user id is sent as `assignedBy`.

**How it works.**
- Map link: `https://www.google.com/maps/search/?api=1&query=...`. For a city it uses `lat,lng` if present, else "city, state". For a court it uses "name, district, state". Opened with `window.open(url, '_blank', 'noopener,noreferrer')`.
- Forms use `react-hook-form` (`register`, `handleSubmit`, `watch`, `setValue`, validation rules).
- City suggestions use the hook `useSearchCitiesByState` in `hooks/court-management/use-cities.ts` and `services/court-management/city.service.ts`.
- The current user id comes from the Zustand auth store.

**Key files.** `components/court-management/cities-data-table.tsx`, `courts-data-table.tsx`, `city-form-modal.tsx`, `court-form-modal-enhanced.tsx`, `hooks/court-management/use-cities.ts`, `services/court-management/*.ts`, `types/court-management/court.types.ts`.

**Interview questions.**
1. *Why no maps SDK?* We only need to show a place. A link is free, has no API key, and adds no bundle size.
2. *Why `noopener,noreferrer`?* So the new tab cannot reach back into our page through `window.opener`.
3. *Why create court + city + mapping in one backend call?* So it is atomic. If one part fails, nothing is half saved.

---

### 7. Agent "My Dashboard" call logs: red status, claim, AI calls, blocked numbers (May - Oct 2026)

**Problem.** On the agent's call-center page, missed inbound calls were easy to miss, two agents could call the same client back, and calls answered by the AI voice bot had no owner.

**What was built** (mainly `components/call-center/CallLogsSection.tsx` and `app/dashboard/call-center/page.tsx`):
- **Red status / "Needs Callback" badge.** Status colours were redone for the full status list (`missed`, `no_answer`, `failed` are red). An inbound call with status missed / no_answer / failed / queued / initiated shows a "Needs Callback" badge.
- **Claim button.** An agent clicks Claim on such a call. The row then shows "Picked by <agent code>". The button shows only when the call has no agent and no claim yet.
- **AI call claim flow.** A socket event tells agents a new AI-handled call is available. A toast with "View Details" opens `AiCallDetailModal` with the AI intake summary. "Proceed" looks up the user by phone, claims the call, then opens the normal consultation flow. If the user does not exist, `AiCallUserCreationModal` opens. If someone else claimed first, `AiCallClaimedModal` says who.
- "Only unclaimed" checkbox, "Claimed by <code>" badge, and the claim name also on the manager page.
- **Blocked badge.** Calls from a blocked number show "Blocked" and never show "Needs Callback".
- Infinite scroll fix for the list.

**How it works.**
- Sockets: `lib/socket/call-center-socket.ts` and `hooks/use-call-center-socket.ts` got two new handlers, `onAiCallAvailable` and `onAiCallClaimed`.
- Proxy routes: `app/api/call-center/ai-calls/unclaimed/route.ts`, `app/api/call-center/ai-calls/[callLogId]/claim/route.ts`, `app/api/call-center/calls/[callLogId]/claim/route.ts`.
- Claim is **first come, first served on the backend**. The backend returns HTTP 409 if another agent already claimed. The UI shows the reason and reloads the list.
- After a good claim, the row is updated in local state right away (the claim is stamped on `call_metadata.callback_claim`), so there is no wait for a refetch.
- Blocked calls are saved by the backend as `failed` with `call_metadata.blocked = true`. The UI reads that flag.
- List paging uses an `IntersectionObserver` on a sentinel at the bottom of a scroll box.

**Tricky parts.**
- Infinite scroll kept firing or never fired. Causes: the observer watched the page viewport and not the scroll box, and the fetch function was re-created when a checkbox changed. Fix: pass `root: scrollContainerRef.current` and keep the checkbox values in `useRef` so the fetch function is stable.
- A callback claim does not set `agent_id` (it is only a marker). An AI claim does set a real `agent_id`. The UI handles both.

**Interview questions.**
1. *Two agents click Claim at the same time. What happens?* The backend does an atomic check (agent is null) and update. One wins. The other gets 409 and sees who took it.
2. *Why use a ref for the checkbox value?* A state value in the dependency list re-creates the callback and re-creates the observer. A ref gives the latest value without that.
3. *What is an optimistic update?* Change the UI right after success (or even before) without a refetch. Here I update only the one row in state.
4. *How does the agent learn about a new AI call?* The server pushes a socket.io event. The page shows a toast with an action button.

---

### 8. Incoming call notification: DTMF priority badge (Oct 2026)

**Problem.** Callers can press a digit (1-9) after dialling. That digit routes them to agents who serve that code. The agent could not see which digit the caller pressed, or whether the call "spilled over" to them because nobody for that code was free.

**What was built.** A small badge "P<digit>" in `components/call-center/IncomingCallNotification.tsx`.
- Solid blue badge: this agent serves that code.
- Amber outline badge: no agent for that code was free, so the call came here.
- A tooltip explains both cases.

**How it works.** Two new optional fields, `dtmfPriority` and `dtmfPriorityMatched`, were added to the socket payload type in `lib/socket/call-center-socket.ts` and to the incoming-call type in `lib/stores/call-center-store.ts`. The component maps them from the socket event into the store and renders the badge only when `dtmfPriority` is a number. The ringtone audio source was also moved to a CDN URL (Sep).

**Interview questions.**
1. *Why optional fields?* Old backends do not send them. Optional fields keep old and new versions working together.
2. *Where is the incoming call kept?* In a Zustand store, so any component (the popup, the page) can read it without prop drilling.

---

### 9. Work queue: clustering, V2 with tabs, stats, infinite scroll, calendar view (Apr - Aug 2026)

**Problem.** "My Work" lists an agent's pending dispositions and follow-up tasks. The old page loaded everything and sorted / filtered in the browser. It was slow, and one client with many items filled the screen.

**9a. Clustered (grouped) view - Apr 2026**
- `components/call-center/clustered-work-queue-view.tsx` (new, about 700 lines).
- Toggle between List and Grouped. Group by customer, lawyer, or disposition.
- Built with a `Map`: key is the phone number or disposition id. Each group has sub-groups by disposition.
- Each group shows the highest priority and the earliest due date. Groups are sorted by priority, then due date, then size.
- The chosen view and group mode are saved in `localStorage` (`workQueue_viewMode`, `workQueue_groupBy`).

**9b. Work queue V2 (backend-driven) - Jul 2026**
- New page component `components/call-center/work-queue/WorkQueueV2Page.tsx`.
- Five tabs: Dispositions, Pending Follow-ups, Overdue, In Progress, Completed.
- Only the active tab is fetched. Filters, sorting, and paging are done by the backend.
- Hooks in `hooks/use-work-queue-v2.ts`: `useWorkQueueV2List`, `useWorkQueueV2InfiniteList` (uses `useSWRInfinite`), `useWorkQueueV2Stats`.
- Service `lib/api/services/work-queue-v2.service.ts`: `getMyDispositions`, `getMyTasks(bucket, ...)`, `getStats`, `updateDispositionPriority`.
- Types in `lib/types/work-queue-v2.types.ts`.
- Each tab keeps its own filters. The Completed tab starts with "last 30 days" so it does not load all history.
- Filters (`WorkQueueFilters.tsx`): priority, category, task type, dates, sort. Customer filter (`CustomerFilter.tsx`): type a phone number to filter directly, or type a name to get user suggestions from `app/api/call-center/user-search/route.ts` (debounced 500 ms; only lawyers and clients).
- Summary cards and tab counts come from one stats call (`WorkQueueV2Summary.tsx`, `WorkQueueV2HeroCards.tsx`). The same hero cards replaced the old task widget on the main call-center page (`components/call-center/disposition/FollowUpTaskWidget.tsx`).
- Infinite scroll: `WorkQueueInfiniteFooter.tsx` shows "Showing X of Y", auto-loads with an `IntersectionObserver` (250 px before the bottom), and has a "Load more" button as a fallback. Page size is 20.
- Inline priority edit on task cards and disposition cards (`components/call-center/task-card.tsx`, `disposition-card.tsx`).
- It started behind an env flag (`NEXT_PUBLIC_WORK_QUEUE_V2`). The flag was removed on 27 Jul and V2 became the only work queue.

**9c. Calendar view - Aug 2026**
- `components/call-center/work-queue/WorkQueueCalendarView.tsx`, `CalendarAgentFilter.tsx`, `hooks/use-work-queue-calendar.ts`, `lib/utils/work-queue-calendar.ts`.
- Month / week / day modes. The grid is hand-built with `date-fns`. No calendar library.
- Dispositions are placed on their `created_at` date. Tasks are placed on their `due_date`.
- Both kinds of rows are turned into one `CalendarItem` shape by mapper functions.
- Scope: "My calendar", one agent, or "All agents" (managers only).
- Viewing other agents is read-only. `TaskCard` got a `lockPriority` prop and `TaskDetailsModal` got a `hideActions` prop for this.

**How state and cache work in V2.**
- No polling in V2. Data refreshes on a button click or after a change.
- SWR keys: `['wq2-inf', tab, filters, pageIndex, limit]`, `['wq2-stats', ...]`, `['wq-calendar', scope, from, to, filters, ...]`.
- After a change (complete, reschedule, priority): refetch the active tab and the stats, and clear the cached lists of the other tabs with a global `mutate` that matches keys by a filter function.
- The calendar has its own cache. The page bumps a `refreshSignal` number to make it refetch. The calendar calls `onMutated` to tell the page to refresh stats.

**Tricky parts.**
- **Tab switch showed the wrong card type.** With `keepPreviousData`, SWR briefly returns the old tab's rows. A disposition row was rendered in a task card. Fix: tag each response with `_tab` and show rows only when the tag matches the active tab.
- **`bucket=all` was dropped.** The query builder skips the value `all` (it means "no filter"). But `bucket` is a routing param. Without it the backend fell back to "pending". Fix: add `bucket` to the URL directly.
- **Timezone off-by-one day.** `toISOString()` gives a UTC date, so an item at 1 am IST landed on the day before. Fix: `dayKey()` builds `yyyy-MM-dd` from local year / month / day.
- **Wrong date column.** The cross-agent tasks endpoint filtered by `created_at`, but tasks are shown by `due_date`. The first version fetched a wider window and bucketed in the browser. Later the request sends `dateField: 'due_date'` so the exact window is fetched.
- **Backend page limit is 100.** The calendar loops pages (up to 5). "All agents" is capped at 40 agents so it cannot fire hundreds of requests.
- **Refresh loop.** If the calendar's own change also bumped `refreshSignal`, it would refetch twice. The two paths were kept apart.

**Interview questions.**
1. *Why move filtering and paging to the backend?* The browser had to download everything first. With server paging the first screen is small and fast, and counts are right for all data.
2. *How does `useSWRInfinite` work?* You give a `getKey(pageIndex, previousPage)` function. Return `null` to stop. `setSize(n)` loads more pages. The data is an array of pages that you flatten.
3. *How did you fix the wrong rows on tab switch?* I tag every response with the tab it belongs to and ignore data whose tag is not the active tab.
4. *Why did items show on the wrong calendar day?* I used a UTC date string as the day key. I changed it to a local date key.

---

### 10. Follow-up task reminders: bounded polling and toast changes (Jul 2026)

**Problem.** The reminder widget on the call-center page polled every 30 seconds. Each poll pulled the agent's **whole** task history plus a dispositions list that was not even used. Reminder toasts also stayed on screen forever and covered the UI.

**What was built.**
- New hook `hooks/use-task-reminders.ts`. It polls every 30 s but asks only for tasks with a due date between **1 hour ago and 15 minutes ahead**.
- `hooks/use-upcoming-tasks.ts` now reads from this hook.
- Toast changes: reminder toasts auto-close after 15 seconds (was never). The toaster moved to the bottom-left.
- The reminder dialog now shows the client or lawyer name and number, and has a **Call** button that opens the manual call modal (`components/call-center/TaskReminderWidget.tsx`).
- The "pending calls" banner was removed from the page.

**How it works.**
- `useSWR('task-reminders', fetcher, { refreshInterval: 30000, revalidateOnFocus: true, keepPreviousData: true })`.
- The key is a fixed string. The time window is computed inside the fetcher at fetch time. So SWR keeps one cache entry and just polls.
- It reuses the existing V2 "my tasks" endpoint with `bucket=all`, `date_from`, `date_to`. No new backend endpoint was needed.
- The UI shows up to about 10 minutes ahead. The fetch looks 15 minutes ahead, so a task cannot be missed between two polls.
- After a task action in the widget, it refreshes its own list and also the stats cards, using SWR's global `mutate` with a key filter (`key[0] === 'wq2-stats'`).
- For a lawyer follow-up task the Call button uses the lawyer's number. For other tasks it uses the customer's number.

**Interview questions.**
1. *Polling or sockets - why polling here?* Reminders depend on the clock, not on a server event. A small poll every 30 s is simple and cheap after the window was bounded.
2. *Why is the time window not in the SWR key?* The window moves every second. In the key it would create a new cache entry on every render. A fixed key with the window computed in the fetcher is stable.
3. *How did you reduce the load?* Before: full history + unused dispositions each poll. After: one request for a 75-minute window, usually a few rows.

---

### 11. Dispositions: manager notes, categories, no-call-log mode, claimed and missed calls (Mar - Oct 2026)

**Problem.** A disposition is the record of what happened on a call. Managers wanted to add review notes. Some conversations happen outside the dialler (lawyer onboarding calls, international clients on WhatsApp), so there is no call log to attach.

**What was built.**
- **Manager notes** (Mar). In `components/call-center/disposition-card.tsx`, users with `call_center:manage` see "Add Manager Notes". The note is saved as `Manager (<name>): text`. The backend appends it to the agent notes. The manager name comes from `useAuth()` with fallbacks (name, email, user type, short id).
- **New categories.** "test disposition" (Jul), "lawyer onboarding" and "international client" (Oct), "callback claimed" and "AI call claimed" (Oct). They were added to every place that lists categories: filters, calendar, clustered view, complete modals, reminder widget (`lib/config/disposition-config.ts`, `lib/types/disposition.types.ts`).
- **Optional lawyer follow-up** (Aug). For "Consultation Created", a checkbox "Also create a Lawyer Follow-up task" sends `create_lawyer_follow_up`.
- **Disposition without a call log** (Oct). `components/call-center/ManualDispositionModal.tsx` got new props: `allowWithoutCallLog`, `categoryAllowlist`, `title`, `direction`. With no call, `call_log_id` is left out of the payload and the phone number is always sent.
- A "Log Onboarding Outcome" action on the lawyers table row and in the lawyer details modal. It opens the same modal with only the "lawyer onboarding" category and direction "outbound".
- An info text that tells the agent what the "Schedule follow-up" tick will really do for the chosen category.
- The "replace all" button was removed from the add-notes modal for overdue tasks (Mar).

**How it works.**
- The modal keeps its form in one `values` object with a `patch()` helper.
- If the call history for a number is empty, the modal no longer blocks. It switches to the no-call path (`noCallsAvailable`).
- Quick-action buttons are filtered by the allow list, so a hidden category cannot be picked by a shortcut.
- The proxy `app/api/proxy/call-center/dispositions/[...path]/route.ts` was extended for the new calls.

**Tricky parts.**
- Backward safe by design: all new props have defaults that keep the old behaviour, so the other callers of the modal were not touched.
- After saving a manager note, the card is updated from the API response (`onDispositionUpdate`) and the list is refreshed in the background (`onRefresh`).

**Interview questions.**
1. *How did you add a new mode to a modal used in many places?* With optional props that default to the old behaviour. Only the new caller passes them.
2. *Why always send the phone number when there is no call?* Without a call log, the number is the only link that keeps the record searchable in the work queue.
3. *Why prefix notes with "Manager (name):"?* The backend stores one notes text field. The prefix lets the UI find and show manager lines without a schema change.

---

### 12. Consultation booking flow: lawyer search modal, packages, call limits (Apr - Oct 2026)

This is the agent flow: client details -> search lawyers -> pick a lawyer -> pick a package -> start the call.

**12a. Lawyer search modal (`components/call-center/LawyerResultsModal.tsx`)**
- **Verified badge** (May) and **Approved badge** (Oct). After feedback both are shown, stacked in the Status column.
- **Source badge.** First "App / DB" from lawyer metadata, then fixed to read `user.registration_source`: App, Admin, or Website (Jun).
- **Anonymity** (Aug). With the env flag `NEXT_PUBLIC_ANONYMIZE_LAWYERS=true`, the modal shows `VSL<userId>` and not the lawyer's name, hides the email, and shuffles the list. The helpers are in `lib/utils/anonymize.ts` (`vslLabel`, `seededShuffle`, a `mulberry32` random generator). The name filter matches the shown label. Toasts also use the label. The recording column in the agent call list is hidden under the same flag.
- This was first also applied to the lawyers management table and then removed from there the same day. It stays only in the call-center modal.
- The flag is set at **build time** in `Dockerfile`, `Dockerfile.dev`, and the compose files, because Next.js inlines `NEXT_PUBLIC_*` values into the browser bundle.
- **Random list badge** (Oct, VS-631). The backend now decides the order. The response has `meta.listOrder` (`random` or `static`). The modal shows "Random order ON" or "Static order". The value travels through `UserDetailsModal` -> page -> the workflow stage in the store.
- **Consultation count** (Oct, VS-638). After the list is on screen, one request loads "consultations in the last 7 days" and "distinct clients" for all lawyers in the list (`app/api/call-center/lawyer-consultation-stats/route.ts`, `apiClient.getLawyerConsultationStats`). If it fails, the line is just not shown.
- Layout work: alignment and column re-order (VS-641).

**12b. Court search in the client details step (`components/call-center/UserDetailsModal.tsx`, Sep)**
- The agent can search and pick one or more courts (debounced 300 ms). The search sends `courtIds`.
- State is required only when no court is picked.
- Saved preferences now include courts. Old saved rows without courts still load.

**12c. Package selection (`components/call-center/PackageSelectionModal.tsx`)**
- **Package creation** (Jun). The modal shows the lawyer's price and any client-specific price as chips. Clicking a chip searches for a package at that amount. If none exists, it creates one. The agent can also type an amount.
- Both price requests run together with `Promise.allSettled`, so one failing does not block the other.
- Tax and commission values were moved to the server route (`app/api/call-center/packages/create/route.ts`). The browser now sends only `{ pricing }` in paise.
- **Call time limit.** A checkbox "end in 10 min" became radio options (None / 5 / 10), then 5-30 minutes, then chips for 5 to 60 minutes in steps of 5 (VS-643). The value is sent as a string of seconds (`max_time_limit`).
- **DND message** (Apr). If the backend says `DND_BLOCKED` (or 403), the agent sees "DND/NDNC Blocked" with a clear reason. Same in `ManualCallModal.tsx`.

**12d. Workflow store refactor (commit 22 Jul, "fixed the modal appear workflow stage")**
- Shared types moved to `lib/types/call-center-domain.types.ts`.
- The page now renders the modal straight from the focused workflow's `stage` in `lib/stores/call-center-store.ts`. The old extra copy of the modal data in React state was removed, so a page reload cannot bring back a stage with missing data.

**Key files.** `components/call-center/LawyerResultsModal.tsx`, `UserDetailsModal.tsx`, `PackageSelectionModal.tsx`, `ManualCallModal.tsx`, `lib/utils/anonymize.ts`, `lib/stores/call-center-store.ts`, `lib/types/call-center-domain.types.ts`, `lib/api/packages.ts`, proxy routes under `app/api/call-center/` (`lawyer-pricing`, `lawyer-user-pricing`, `packages/search`, `packages/create`, `filter-lawyers`, `lawyer-consultation-stats`).

**Tricky parts.**
- The shuffle must not change while the agent filters or calls. A seed is made once per modal open (`useMemo(() => Math.random(), [isOpen])`) and a seeded shuffle gives the same order for the same seed.
- A leftover merge conflict marker from the VS-643 merge broke the package modal. Fixed in a follow-up commit.

**Interview questions.**
1. *Why hide lawyer names from agents?* To keep the choice fair and stop agents from always picking lawyers they know. The agent sees a code, not a name.
2. *Why a seeded shuffle and not `Math.random()` in sort?* A plain random sort changes on every render. A seed gives a stable order for one modal open.
3. *Why must `NEXT_PUBLIC_` variables be set at build time?* Next.js replaces them with their values when it builds the browser bundle. Changing them at run time has no effect.
4. *`Promise.all` vs `Promise.allSettled`?* `all` fails if any one fails. `allSettled` waits for all and gives each result, so I can use the ones that worked.

---

### 13. Editing a lawyer's data on their behalf (Aug - Nov 2026)

**Problem.** Many lawyers do not finish their profile in the app. Agents needed to fill it for them, but only with the right permission.

**What was built.**
- **Basic details editor** (Aug): `components/call-center/lawyer/BasicDetailsEditor.tsx`. Edits name, email, gender, date of birth. Proxy: `app/api/proxy/lawyer/basic-details/route.ts`. Permission: `lawyer:edit-basic-details`.
- **Pricing editor** (Aug): `components/call-center/lawyer/PricingEditor.tsx`. Sets the consultation fee. Entered in rupees, sent in paise, max 1,00,000 rupees. Permission: `lawyer:edit-pricing`.
- **Professional details editor** (existing file, extended by me): added bar registration number (Oct), AIBE pass yes / no (Nov, VS-612), and a minimum experience fix. File: `components/call-center/lawyer/ProfessionalDetailsEditor.tsx`.
- **Consultation preferences** (Oct - Nov, VS-684): the four yes / no answers (free pre-engagement call, pro-bono, low fee case, home visit). First shown as badges, then made editable in `components/call-center/lawyer/ConsultationPreferencesEditor.tsx` with `app/api/proxy/lawyers/lawyer-pricing/questions/route.ts`.
- Customer ID and Lawyer ID columns in the call logs, click to copy (Aug).
- Service file: `lib/api/services/lawyer/lawyer.service.ts` (`updateBasicDetails`, `updateLawyerPricing`, `updateConsultationPreferences`) and types in `lawyer.types.ts`.

**How it works.**
- Each editor is a small component inside the details modal. It returns `null` if the user lacks the permission.
- **Per-field dirty tracking.** Each field has its own "changed" flag. Only changed fields are sent. An untouched field is never overwritten.
- Checks in the browser: valid email, date of birth in the past, bar number at least 3 and at most 100 characters. An empty bar number means "remove it".
- After save, `onSaved()` tells the parent to refetch.

**Tricky parts.**
- Expertise names showed as raw IDs in some screens. Different callers passed the mapping in two shapes (`{id: name}` or `{id: {id, name}}`). `normalizeExpertiseMapping()` accepts both.
- Consultation answers come from two places (pricing API and lawyer metadata), and neither is always there. After a save, the saved answers are kept in local state and used first.
- The AIBE change was committed, reverted, and committed again on its own branch (VS-612). It is on `dev-main` now.

**Interview questions.**
1. *Why send only changed fields?* It avoids overwriting data that someone else changed, and the backend can tell exactly what was edited (some edits trigger re-verification).
2. *How do you gate an editor by permission?* `useHasPermission(...)` at the top; return `null` when false. The API checks the same permission.
3. *Why does the editor not update the list itself?* The table owns the data. The editor calls `onSaved` and the table refetches with SWR `mutate()`.

---

### 14. Approved lawyer flow, altered profile, share profile PDF, notes in the lawyer modal (Oct - Nov 2026)

**What was built.**
- **Dashboard approved** (Oct). A second flag next to "Verified". A button in the lawyer details modal toggles it through `/api/proxy/admin/dashboardApproveLawyer`. Verifying a lawyer also sets approved. Un-verifying does not remove it. New table column, filter, and badge in the search modal. Needs `lawyer:update`.
- **Altered profile** (Oct - Nov, VS-630 / VS-675). A filter "Altered Profile (Needs Re-verification)" for lawyers who lost verification because of an edit. The details modal shows a small timeline: when verified, when and why un-verified (basic details updated, professional details updated, KYC updated, KYC rejected), and which fields changed. Times are shown in IST.
- **Share profile** (Nov, VS-672). A button calls `/api/proxy/admin/lawyer-profile-pdf?userId=...`, gets a link to an anonymous PDF (advocate code, experience, city, expertise, languages; no name, phone, or email), and offers Copy Link, Open PDF, and a WhatsApp share link.
- **Notes in the lawyer modal** (Oct, VS-636). Notes can be edited in place in `components/call-center/CallNotesDisplay.tsx`. Only the author can edit, within 24 hours; the backend returns `can_edit`. "Load older notes" paging was added in `hooks/use-call-notes.ts`. Max 2000 characters.
- **Call status badge** (Oct): `components/call-center/CallStatusBadge.tsx`. A badge, an (i) tooltip that explains the status in plain words, and a refresh button for "live" statuses. Refresh calls `app/api/call-center/calls/[callLogId]/reconcile-status/route.ts`, which asks the telephony provider if the call has ended.

**Tricky parts.**
- **Late response under the wrong lawyer.** The PDF link can arrive after the user has opened another lawyer. A `useRef` holds the id of the lawyer on screen. If the ids differ when the response comes back, the result is dropped.
- The status explanation is always read from the row's current status. If the `outcome` object is for an old status, a fallback text by status is used.
- A short cooldown stops a double click from calling the provider API many times.
- An error card used to replace the whole notes list when one edit failed. Now the card shows only when there are no notes; other errors are toasts.

**Interview questions.**
1. *What is a race condition in the UI and how did you stop it?* A slow response arrives after the user moved on. I keep the current id in a ref and ignore responses for another id.
2. *Why two flags, verified and approved?* Verified means documents were checked. Approved means operations allow consultation calls. Approved can stay on when verification drops after a profile edit.
3. *Who decides if a note can be edited?* The backend. It sends `can_edit`. The UI only shows or hides the button.

---

### 15. Send payment link modal (Sep 2026)

**Problem.** The "create package and charge the client" logic was only inside the consultation booking modal. Staff wanted to send a payment link from the wallet page and the client page too.

**What was built.**
- `components/payments/SendPaymentLinkModal.tsx` (new, about 540 lines) and a small trigger button.
- `lib/api/packages.ts`: shared helpers `searchPackagesByAmount`, `createPackageForAmount`, `resolvePackageForAmount`. The booking modal now uses these too, so the code is not duplicated.
- `lib/api/payment-link.ts`: `sendPaymentLink()` posts to `/api/proxy/admin/add-funds-to-client-wallet`.
- `lib/money.ts`: rupee / paise helpers.
- The modal is opened from the wallet details sheet (client wallets only), from `components/user-management/ClientDetailsModal.tsx`, and from the call-center user details modal.

**How it works.**
- The agent types a price. If a package at that price exists it is shown. If not, the agent must click to create one. Creating is never automatic.
- The request sends the user id, the amount in paise, and the `packageId`. The server decides the label the client sees. WhatsApp sending is opt-in and unchecked by default.
- The modal is mounted as a sibling of the sheet, not inside it, so two overlays do not fight over focus.

**Tricky parts.**
- First the browser built the "service" label from the package title. This was moved to the backend so the message and the package can never disagree.
- The server's error text is at `error.message`. Reading `error` itself gave "[object Object]". Fixed in the helper.

**Interview questions.**
1. *Why move logic into `lib/api/packages.ts`?* Two screens needed the same search-or-create steps. One shared function means one place to fix bugs.
2. *Why send `packageId` and not a label?* The server is the source of truth for price and label. The client cannot send a wrong amount or text.

---

### 16. User management: edit email / phone, user status flags (Sep - Nov 2026)

**16a. Edit user email and mobile (Sep)**
- `components/user-roles/edit-contact-modal.tsx`, opened from the row menu in `components/user-roles/user-roles-table.tsx`.
- Service: `usersService.updateContact()` in `lib/api/services/users.service.ts` -> `PATCH /api/proxy/admin/update-user-contact`.
- New permission `user:contact:update`. It is separate on purpose: mobile and email are login identities, so the normal `user:update` does not include it.
- Only the changed field is sent. Mobile must be 8-15 digits with an optional `+`. Email is lower-cased.
- A warning tells the admin that changing the mobile logs the user out of all devices. The success toast confirms it when the API returns `sessionsRevoked`.
- The form is re-filled every time the dialog opens, so one user's edit never leaks into the next user's form.

**16b. User status flags (Nov, VS-676)**
- Purpose: mark a lawyer or client as "not a genuine user" (internal team, automated test, demo, partner evaluation, just exploring, research, duplicate account, junk, other). Flagged users are hidden from the admin lists by default, so the lists and exports stay clean.
- `lib/user-status-flags.ts`: the nine flag values, labels, descriptions, groups, and helpers (`flagsOf`, `noteOf`, `isFlagged`, `statusFlagsSummary`, `appendStatusFlagParams`).
- `components/user-management/UserStatusFlagsEditor.tsx`:
  - `UserStatusFlagChips`: amber chips on table rows and modal headers. Hover shows the meaning and the note.
  - `StatusFlagPicker`: a grouped multi-select built with Radix `Popover` + `cmdk`. Each flag has an (i) tooltip.
  - `UserStatusFlagsEditor`: pick flags, add a note, save. It sends the whole set to `PATCH /api/proxy/admin/users/status-flags`. An empty set clears the flags and the note. "Other" needs a note.
- Used in the lawyer details modal and in `ClientDetailsModal.tsx`. Editing needs `lawyer:update` / `user:update` (passed in as `canEdit`).
- Tables: a "Show flagged" checkbox and a flag filter on both the lawyers and clients tables. Two new CSV columns (flags and note).

**Tricky parts.**
- **Could not scroll the flag list inside a dialog.** A Radix Dialog locks scrolling for everything outside its content. The popover is rendered in a portal, outside the dialog, so the mouse wheel did nothing. Fix: add the `modal` prop to the `Popover`.
- Array filter params must be repeated (`statusFlags=a&statusFlags=b`). The backend framework reads `statusFlags[]` as a literal key.
- The API wraps replies as `{ success, data }`. The editor reads the saved value from either shape.

**Interview questions.**
1. *Why a separate permission for contact edits?* Email and mobile are how a user logs in. Changing them is risky, so it must be granted on purpose.
2. *Why could the dropdown not scroll, and how did you fix it?* The dialog's scroll lock blocked wheel events for elements outside it, and the popover was portaled outside. Making the popover modal fixed it.
3. *Why send the full flag set and not add / remove calls?* It is simple and idempotent. The server stores exactly what it receives. An empty set means "clear".
4. *How do you send an array in a query string?* `URLSearchParams.append` with the same key many times.

---

### 17. Telephony provider UI - Bonvoice integration (Oct - Nov 2026, VS-647, on `dev-main`)

**Problem.** All calls went through one telephony provider (Exotel). The company is adding a second one (Bonvoice). Managers need to switch without a deploy and test the new one safely with a few agents first.

**What was built.**
- **`components/call-center/TelephonyProviderCard.tsx`** on the call-center page:
  - A switch: which provider starts **new outbound calls**.
  - A second switch "Agents may choose": lets each agent pick their own provider for their client calls.
  - A settings form for Bonvoice: caller ID, two channel ids, event relay URLs (JSON), DID-to-environment map (JSON), and an inbound test field.
  - Shows "Last changed <time> by <user> (v<version>)".
- **Per-agent provider** in `components/call-center/AgentStatusToggle.tsx`: a line "Next call via <provider>" with a reason, and a dropdown when the agent is allowed to choose.
- `components/call-center/AgentListModal.tsx`: a "calls via ..." badge per agent.
- **`components/call-center/ProviderBadge.tsx`**: a small badge on each call row that says which provider carried the call. Old rows without the field read as Exotel.
- `CallStatusBadge` texts were made provider-neutral (a `{P}` placeholder is replaced by the provider name).
- New permission `call_center:telephony:update` in `lib/permissions.ts`.

**How it works.**
- Proxy routes: `app/api/call-center/telephony/provider/route.ts` (GET / PUT), `app/api/call-center/telephony/bonvoice-settings/route.ts` (GET / PUT), `app/api/call-center/agents/preferred-provider/route.ts` (PATCH).
- The card loads both configs together with `Promise.all`. It only loads if the user has the permission. Without it the card renders nothing.
- Settings are saved in the backend config table, so they apply without a deploy. The success toast says they go live within 30 seconds.
- The two JSON fields are parsed with `JSON.parse` in a `try / catch` before saving. Bad JSON shows a toast and nothing is sent.
- **Cross-component refresh with a browser event.** After a switch flip, the card fires `window.dispatchEvent(new CustomEvent('vs:telephony-provider-changed'))`. The agent toggle listens and reloads its "Next call via" line. No shared store was needed.
- The agent's data comes from the existing agent-details endpoint, which now has a `telephony` object (`switchOutbound`, `agentChoice`, `canChoose`, `preferred`, `nextCallVia`, `reason`). The dropdown shows only when `canChoose` is true. A telephony manager can always choose.

**Tricky parts.**
- The "Next call via" line went stale when the manager flipped the switch on the same page. The custom event fixed it.
- The rule changed during testing with the provider: first agents always had a "Default" option, then the dropdown showed the real provider, then `canChoose` was added for managers.
- The switch changes only the **next** call. Running calls finish on the provider they started with. Consultation calls always use the company switch.

**Interview questions.**
1. *How do two unrelated components talk without a store?* With a `CustomEvent` on `window`. One dispatches, the other adds a listener in `useEffect` and removes it on unmount.
2. *Why keep provider settings in the database and not in env variables?* So a manager can change them at run time, with a version and "who changed it", and no deploy.
3. *How do you roll out a new provider safely?* A global switch plus a per-agent choice. A few agents test first. Old calls and old rows keep working because the default is the old provider.
4. *How is the card protected?* The UI checks `call_center:telephony:update`. The backend checks the same string on the PUT calls.

---

### 18. Page titles (Jun 2026)

**Problem.** Every browser tab and every page header said the same thing. Staff with many tabs open could not tell them apart.

**What was built.**
- `hooks/use-document-title.ts`: `useDocumentTitle("Lawyers")` sets `document.title` to "Lawyers - VakeelSaab" in a `useEffect`.
- `lib/page-titles.ts`: one map from route path to title, and `getPageTitle(pathname)` with a fallback.
- `components/site-header.tsx` now shows the title from `usePathname()` (or a `title` prop).
- The hook was added to 18 pages. Pages that were server components got `"use client"`.

**Interview questions.**
1. *Why a hook and not Next.js `metadata`?* These pages are client components behind login. `metadata` works in server components. A small `useEffect` is the simplest fit.
2. *Why one central map?* The header and the tab title read the same source, so they cannot drift apart.

---

### 19. Smaller items

- **Consultation table fix** (Nov, VS-688, feature branch only): duration is shown as `m:ss`, a "Not answered" / "Too short (8s)" reason is shown, and a new stage `networkDisconnected` was added so the "failed" count and the filtered list match. Files: `components/consultations/ConsultationTable.tsx`, `ConsultationStatsBar.tsx`, `lib/types/consultation.types.ts`.
- **Court limit per lawyer** raised to 5 and made a prop (`maxCourts`) in `components/call-center/LawyerCourtManagement.tsx` (May).
- **Agent name fallback**: if the agent's user record did not load, show the agent code, not "Unassigned" (Oct).
- **Next.js 15 fix**: route `params` is a Promise and must be awaited (`app/api/proxy/call-center/calls/[id]/route.ts`, Aug).
- **Task card heading** for lawyer follow-up via WhatsApp (May).
- **Plan documents**: I wrote several plan files under `docs/` before building (KYC approval, clustered view, export button, dynamic page titles, work queue backend-driven, calendar view, call-time limit, call-logs status filtering).

---

### Patterns worth saying in an interview

1. **Backend for frontend (BFF) proxy.** The browser calls same-origin Next.js routes. The route adds nothing secret but hides the gateway and avoids CORS. I wrote about 20 such `route.ts` files.
2. **Server-side work over client-side work.** Stats, filtering, paging, list order, and labels were moved to the backend step by step (features 1, 9, 12, 15).
3. **Additive, backward-safe changes.** New props and new response fields are optional with safe defaults (features 8, 11, 12, 17).
4. **Bounded data.** Never load "everything": page size 2000 for export, 20 for lists, a 75-minute window for reminders, 30 days for completed work.
5. **Cache keys as the reload trigger.** With SWR the key holds tab + filters. Change the key and the data reloads. Use `mutate` with a key filter to refresh many caches.
6. **Race guards.** Tag responses (tab tag, lawyer id in a ref, `cancelled` flag in effects) and drop late ones.
7. **Permissions twice.** UI hides; API enforces.

### Not confirmed

- Whether each feature is live in production. Only branch membership was checked.
- Business numbers (how many agents, how much faster). No numbers were found in the code.
- Backend behaviour described in code comments (for example atomic claim, 409 rules, "live within 30 s") was read from the frontend code and comments only, not from the backend repos.
- `recharts`, `react-day-picker`, `libphonenumber-js`, `xlsx`, `papaparse`, `file-saver`, and Playwright use by me: not found in my commits.
- `ProfessionalDetailsEditor.tsx`, `authorizedFetch`, the token manager, and the PBAC base (`lib/permissions.ts`, hooks) existed before or were created by others; I extended and used them.

---

# Part 6: API Gateway

## API Gateway (vs-gateway)

### 1. My role in this repo

I added and maintained gateway routes for almost every backend feature I built in user-service and lawyer-appointment. About 72 non-merge commits from 23 Dec 2025 to 15 Nov 2026. Most of my work is in `config/services/**`. I also did one large refactor (splitting the route config into one file per controller) and one piece of core work on array query params.

Rule I learned early: **a backend API does not exist for the apps until it has a gateway route.** And a query param does not reach the service until it is in the route's allow list.

### 2. How a gateway route is defined (general pattern)

A route is one object in a `Route[]` array. Sanitized sketch:

```ts
// config/services/<service>/<something>-controller.ts
import { Route } from '../../../src/types/service';

export const thingRoutes: Route[] = [
  {
    id: 'get-things',                    // unique name, used in logs
    path: '/api/v1/things',              // same path as the upstream controller
    methods: ['GET'],
    description: 'List things',
    middleware: ['auth'],                // 'auth' = JWT check. [] = public route
    timeout: 30000,                      // ms
    retries: 1,                          // total attempts. Use 1 for writes
    headers: {
      required: ['Authorization'],       // 400 if missing
      filter: { allow: ['Authorization', 'Content-Type', 'Accept'] } // forward only these
    },
    query_params: {
      allowed: ['page', 'limit', 'status'],          // everything else is dropped
      defaults: { page: 1, limit: 10 },
      validation: { page: { type: 'integer', min: 1 } }
    },
    body: {                              // for POST / PATCH / PUT
      content_types: ['application/json'],
      max_size: '1MB',
      required: true
    }
  }
];
```

Then the array is spread into the service's `index.ts`:

```ts
// config/services/<service>/index.ts
const config: ServiceConfig = {
  service: {
    name: 'some-service',
    base_route: '/g/xx',                                   // public prefix
    upstream: { base_url: process.env.SOME_SERVICE_URL, timeout, retries, keep_alive },
    global_headers: { add: {...}, remove: ['X-Internal-Token'], forward: [...] },
    routes: [...thingRoutes, ...otherRoutes]
  }
};
```

What happens at runtime (`src/core/router.ts`): for each route the router builds a Fastify route with (1) a JSON schema for query and path params, (2) a list of pre-handlers: core middlewares first (request context, logging, security headers, basic validation), then the middlewares named in the route (`auth`, `rate-limit`), and (3) one generic handler that validates and then calls `proxyRequest`.

Steps I follow to add a route:
1. Read the upstream controller: path, method, guards, query params, body.
2. Add the route object in the right controller file. Pick `middleware: ['auth']` if the upstream has a JWT guard.
3. List every query param in `allowed`. Add `Content-Type` to required headers only for JSON bodies.
4. For write APIs set `retries: 1` so the call is not repeated.
5. Test with the dev server (config hot-reloads). Remember production needs a rebuild.

### 3. Auth and permission middleware used

- `auth`: JWT verify (HS256, issuer, audience) plus the deactivated-user check. Used on almost all my routes.
- `rate-limit`: listed on call-center routes to match the file's existing style. In current code it only logs.
- No middleware (`[]`): for public reads (restricted chat config, packages by type, package search) and for provider webhooks and internal cron endpoints, which are protected upstream by an internal secret or provider signature instead of a JWT.
- Fine-grained permissions (for example `lawyer:edit-pricing`, `lawyer:read`, `lawyer:update`) are enforced upstream. I wrote that as a comment next to the lawyer-pricing admin routes.

### 4. Features I built (grouped, with rough dates)

Paths below are shown without the `/g/us`, `/g/la`, `/g/cc`, `/g/cs` prefix.

#### 4.1 Lawyer profile sections: career, education, case highlights (Dec 2025 - Feb 2026)
- **Added** (23 Dec 2025): 21 routes in one commit. Lawyer profile (`lawyerProfile`, `createLawyerProfile`, `lawyerProfileDetails`) and full CRUD for `/api/v1/careers`, `/api/v1/educations`, `/api/v1/case-highlights` (create, list, by-id, by-user, update, delete).
- **Auth hardening** (7 Jan and 13 Feb 2026): the first version had many routes with `middleware: []`. I changed them to `['auth']` and added `Authorization` to required headers, so the gateway rejects anonymous calls before they reach the service.
- **Files**: `config/services/user-service/career-controller.ts`, `education-controller.ts`, `case-highlight-controller.ts`, `lawyer-controller.ts`.

#### 4.2 Restricted chat config (26 Dec 2025)
- **Added**: `GET /api/v1/chats/restricted-chat-config` to lawyer-appointment config. Public route (no middleware). It returns the restricted chat configuration.
- **File**: `config/services/lawyer-appointment-service/chats-controller.ts` (first added in the old single file).

#### 4.3 Call quota APIs (7 Jan 2026)
- **Added**: `POST`, `PATCH`, `GET /api/v1/call-quota` and `GET /api/v1/call-quota/all`. Create and "get for user" need `auth`.
- **File**: `config/services/user-service/call-quota-controller.ts`.

#### 4.4 Packages (Jan - Jun 2026)
- `GET /api/v1/packages/by-type` with allowed query `type` (7 Jan 2026).
- `GET /api/v1/packages/search` with allowed query `amount` (3 Jun 2026).
- `POST /api/v1/admin/create-package` for call-center packages (20 Jun 2026). I first put it in the old file, then moved it to the right controller file the same day.
- **Files**: `package-controller.ts`, `admin-controller.ts`.

#### 4.5 Lawyer status and heartbeat (Jan and Apr 2026)
- `POST` and `GET /api/v1/lawyer/lawyerStatus` (9 Jan 2026): lawyer sets and reads online/offline status. Short 15s timeout.
- `POST /api/v1/lawyer/lawyerHeartbeat` (6 Apr 2026): heartbeat call that keeps the lawyer's online/offline status fresh.
- **File**: `config/services/user-service/lawyer-controller.ts`.

#### 4.6 Big refactor: split config into one file per controller (9 Jan 2026)
- **Problem**: two huge files. `user-service.ts` was about 2,400 lines. `lawyer-appointment-service.ts` was about 700 lines. Hard to find routes, easy to get merge conflicts.
- **What I did**: created a folder per service with one file per upstream controller (27 new files, about 2,500 lines), each exporting a `Route[]`. An `index.ts` spreads them into the service config. Changed `config/services.ts` to point at `services/user-service/index.ts` and `services/lawyer-appointment-service/index.ts`.
- **Result**: the file layout in the gateway now mirrors the controllers in the services. Later, new controllers followed this pattern (availability, recent-activity, documentation-service, invoice, admin-audit, address).
- **Follow-up** (12 Jan): added more routes to the new files after syncing my branch, and removed two route-list text files that had been committed by mistake.

#### 4.7 Documents (Jan and Aug 2026)
- Generic upload flow (12 Jan): `POST /api/v1/documents/generic/initiate-upload`, `/generic/upload-complete`, `/generic/s3-url`. This is the pre-signed S3 upload pattern: ask for an upload URL, upload from the client, then confirm.
- `GET /api/v1/documents/:id` (13 Jan).
- `POST /api/v1/documents/delete` (12 Aug).
- **File**: `config/services/lawyer-appointment-service/documents-controller.ts`.

#### 4.8 Admin APIs and verify-lawyer endpoints (Jan 2026, then ongoing)
- 12 Jan: `create-client`, `update-client`, `add-funds-to-client-wallet`, `update-lawyer`, `toggle-user-status`, and the lawyer migration route (with `retries: 1`).
- 22 Jan: `verifyLawyer`, `reset-password`, `assign-court` (POST, PATCH, DELETE), `search-courts`. Also moved older admin routes from no-auth to `auth`. The "get lawyers" and "get clients" style GET routes were wrongly declared with a required JSON body. I replaced the body block with a proper `query_params` allow list, defaults (`page`, `limit`, `sortBy`, `sortOrder`) and type rules.
- Later single routes: `update-user-contact` (11 Sep, change email/phone), `dashboardApproveLawyer` (8 Oct), `lawyer-profile-pdf` (5 Nov), `users/status-flags` (11 Nov).
- **File**: `config/services/user-service/admin-controller.ts`.

#### 4.9 Availability, slots and slot confirmation (Jan - Feb 2026)
- 22 Jan: new `availability-controller.ts` with `getAllLawyersAvailability`, `getLawyerAvailability` (query: `lawyerId`, `date`, `timezone`), `createLawyerAvailability`, `updateLawyerAvailability`. In `slot-confirmations-controller.ts`: get by id, update changed from PUT to PATCH, list slots, slot by id, reschedule/cancel.
- 11-12 Feb: three small commits changing the allow list of "get all slot confirmations" (added `date`, then `timezone`, then removed both). This followed the upstream API changing its filters. Good example of why gateway and service must change together.
- 23 Feb: `GET /api/v1/slot-confirmations/schedule-call-list` (query `date`) and `GET /api/v1/cases/case-transfer-list`.
- 16 Feb: `PATCH /api/v1/cases/proceedings`.

#### 4.10 Recent activity (29 Jan 2026)
- New `recent-activity-controller.ts`: `schedule-requests`, `case-acceptance-requests`, `performance` (query `type`, `timezone`). 30s timeouts.

#### 4.11 Documentation services (4 Feb 2026)
- New `documentation-service-controller.ts`: create ticket, list tickets, update by `:ticketID`.
- The list route has the richest validation I wrote: allow list of `agentID`, `clientID`, `lawyerID`, `ticketID`, `serviceID`, `status`, `priority`, `created_at`, `updated_at`, with type rules for each.

#### 4.12 Invoice APIs (10 Feb 2026)
- New `user-service/invoice-controller.ts` with 8 routes (create, update, get by id / user / chat, list, delete, stats), wired into `index.ts`.

#### 4.13 Device registration for push (Mar and May 2026)
- `POST /api/devices/register` (25 Mar) and `GET /api/devices/user/:userId/latest` with query `appType` (27 May), both in `config/services/comm-service.ts`.
- These have no JWT middleware. The repo's own future-task note (FT-002) marks unauthenticated device registration as an open security item. Good thing to mention honestly in an interview.

#### 4.14 Call-log stats route and its validation (31 Mar and 14 May 2026)
- 31 Mar: `GET /api/v1/call-center/calls/call-logs/stats` with `auth`, and an allow list (`agent_id`, `customer_number`, `call_type`, `status`, `from_date`, `to_date`, `search`).
- 14 May: I removed the `query_params` block. The dashboard had started sending new filters and the gateway was silently dropping them. With no block, the gateway passes all query params and the service validates them.

#### 4.15 Lawyer list filters: KYC, expertise, city/state, and the array-param problem (Apr - May 2026)
- Small commits adding params to the admin "get lawyers" allow list: `kycStatus` (2 Apr), court filters (10 Apr), city/state lists (16 Apr), a "no name" filter (17 Apr), then `verifiable`, `professionalDetailsCompleted`, `basicDetailsCompleted`, `onboardingSource`, `isVerified` (27 Apr).
- 11 Apr: `GET /api/v1/court-city/cities/search-by-state` (query `state`, `search`).
- 30 Apr: raised the `limit` max from 100 to 100000 for admin lawyer and client lists, so the dashboard can load a full list in one call (exact reason not confirmed in code).
- **The array problem** (10 Apr): filters like `expertise=2&expertise=4` broke. Fastify parses repeated keys into an array. The route schema said `type: string`, so it failed. Also, Axios re-writes arrays as `expertise[]=2`, which the NestJS side does not read. I wrote a fix in the core: (a) keep the original query string as-is when building the upstream URL instead of passing parsed `params` to Axios, (b) a new validation type `string_array` with no type limit in the schema, (c) fixed a bug where `min: 0` was ignored because the code tested `validation.min &&` instead of `!== undefined`. I also wrote `docs/ARRAY_QUERY_PARAMS_FIX.md`.
- 13 Apr: I reverted the core change ("removed the unnecessary changes"). On the branch I read, `proxy.ts` again passes `params: request.query`.
- 18 and 27 May: final simpler approach. I removed the whole `query_params` block from the lawyers and clients list routes. Now the gateway does not filter these two routes, and the service DTO does all validation.
- Note: the repo's CLAUDE.md hazard list describes the raw-query approach as current. That does not match the code on the branch I read. Treat the exact current behaviour on other branches as not confirmed.

#### 4.16 KYC content-type handling (30 Apr 2026)
- **Problem**: KYC routes had `headers.required: ['Content-Type']`. GET calls and file uploads from the apps do not always send a JSON content type, so the gateway rejected them.
- **Fix**: removed the required header from the 8 KYC routes that had it (this included GET routes), then put it back on the 3 write routes that send a body (create, update, approve).
- **File**: `config/services/user-service/kyc-controller.ts`.

#### 4.17 App config (4 May 2026)
- `POST /api/v1/application/app-config` with `auth` and a 10s timeout. File: `application-controller.ts`.

#### 4.18 AI-call routes (5 May 2026)
- `POST /api/v1/call-center/ai-calls/completed` (the AI calling system reports a finished call), `GET /ai-calls/unclaimed` (query `page`, `limit`, retries 3 because it is a safe read), `POST /ai-calls/:callLogId/claim` (an agent claims an AI lead, retries 1).
- First place I set `retries` on purpose: reads can retry, writes must not.

#### 4.19 Admin audit log (9 May 2026)
- New `admin-audit-controller.ts`: list, by admin (`:adminId`), by resource (`:resource/:resourceId`), by id. All `auth`.

#### 4.20 Call-center work queue and call tools (Jul - Oct 2026)
- 23 Jul: `PATCH /dispositions/:id/priority`, and v2 work-queue reads: `tasks/v2/my-tasks`, `tasks/v2/my-dispositions`, `tasks/v2/stats`.
- 4 Aug: internal `backfill-stuck-call-dispositions` route.
- 5 Aug: `POST /calls/:callId/claim` for callback claim. A second commit renamed the path param from `callLogId` to `callId` to match the controller.
- 16 Oct: **cleanup call status API**: `POST /exotel/webhook/internal/cleanup-stale-calls`. No JWT at the gateway because a scheduler calls it. It is protected upstream by the internal secret. `retries: 1`.
- 21 Oct: `POST /calls/:callId/reconcile-status` (re-check one call's real status with the provider).
- 24 Oct: `POST /calls/lawyer-consultation-stats` (consultation numbers per lawyer).
- **File**: `config/services/call-center.ts`.

#### 4.21 Lawyer basic details, pricing, profile V2 (Aug - Nov 2026)
- 13 Aug: `PATCH /api/v1/lawyer/basic-details` (query `userId`) so an admin can edit a lawyer's basic details.
- 26 Aug: `POST /api/v1/lawyer-pricing/admin` (query `userId`). 15 Nov: `PATCH /api/v1/lawyer-pricing/admin/questions` for the editable consultation pricing features. Both carry a comment that `lawyer:edit-pricing` is enforced upstream.
- 31 Aug: `GET /api/v1/lawyer/lawyerProfileV2` (query `key`).

#### 4.22 Address CRUD and auth (27 Aug 2026)
- New `address-controller.ts`: create, list, by-id, by-user, update, delete on `/api/v1/addresses`.
- Second commit the same day: the three read routes were public by mistake. I added `auth` and the required `Authorization` header. Addresses are personal data, so reads must be protected too.

#### 4.23 Lawyer PDF generation (5 Nov 2026)
- `GET /api/v1/admin/lawyer-profile-pdf` (query `userId`, `auth`).
- Flow behind it: user-service checks `lawyer:read`, builds an anonymised lawyer profile, asks doc-service to render the PDF (HTML template to PDF), stores it in S3 and returns a link. It re-renders only if the stored file is out of date. Long timeout (100s) because PDF rendering can be slow.

#### 4.24 Telephony / Bonvoice routes (29 Oct - 2 Nov 2026)
Gateway side of adding a second telephony provider next to Exotel.
- `GET` and `PUT /telephony/provider`: read and set the active provider (the switch).
- `POST|GET /bonvoice/webhook/call-events` and `/bonvoice/webhook/incoming-route`: provider webhooks. No JWT (the provider cannot send our token). Both methods are allowed because the provider can call either way.
- `POST /bonvoice/webhook/internal/cleanup-stale-calls`: internal cleanup, with `auth`.
- `GET` and `PUT /telephony/bonvoice-settings`: provider settings from the dashboard.
- `PATCH /agents/me/preferred-provider`: each agent chooses a provider for their own calls. A later commit updated the description after the rule changed to "always honoured".
- **Bug I found and documented**: setting `retries: 0` on a write route crashed the gateway with "Cannot read properties of undefined (reading 'message')". Reason: the HTTP client loop is `for (attempt = 1; attempt <= maxRetries; ...)`. So `retries` is really "total attempts". With 0 the loop never runs and the code then reads an undefined error. I used `retries: 1` with a comment and wrote `docs/future-tasks/FT-003-retries-zero-crashes-route.md` with two fix options.

### 5. Timeline (short)

| Period | Main gateway work |
|---|---|
| Dec 2025 | First routes: lawyer profile, career, education, case highlights, restricted chat config |
| Jan 2026 | Call quota, packages by type, lawyer status, config split refactor, documents, admin and verify-lawyer routes, availability and slots, recent activity |
| Feb 2026 | Documentation services, invoices, slot confirmation filters, proceedings, case-transfer and schedule-call lists, auth hardening |
| Mar - Apr 2026 | Device register, call-log stats, heartbeat, lawyer list filters, array query param work, city/state search, KYC header fix |
| May - Jun 2026 | App config, AI-call routes, admin audit log, query validation cleanup, device lookup, package search, call-center package |
| Jul - Sep 2026 | Work queue v2, callback claim, document delete, basic details, lawyer pricing, address CRUD, profile V2, user contact update |
| Oct - Nov 2026 | Dashboard approve lawyer, cleanup stale calls, reconcile status, consultation stats, telephony / Bonvoice, lawyer PDF, status flags, pricing questions |

### 6. Key files to remember

- `config/services.ts`: service registry.
- `config/services/user-service/*.ts`, `config/services/lawyer-appointment-service/*.ts`: one file per controller (my refactor).
- `config/services/call-center.ts`, `config/services/comm-service.ts`: single-file configs.
- `src/core/router.ts`: turns config into Fastify routes, validates requests.
- `src/core/middleware.ts`: `auth` (JWT), `rate-limit` placeholder, logging, security headers.
- `src/core/proxy.ts`, `src/utils/httpClient.ts`: forward to upstream, retries, error mapping.
- `src/utils/validation.ts`: query and path param rules.
- `src/utils/inactive-users.ts`: deactivated-user list from Redis.
- `docs/ARRAY_QUERY_PARAMS_FIX.md`, `docs/future-tasks/FT-003-*.md`: docs I wrote.

### 7. Interview questions with simple answers

**Routing and design**

Q1. What does your API gateway do?
A. It is the single entry point. It checks the JWT, checks headers and query params, and forwards the request to the right service. It also adds a request id and logs every request. It holds no business logic.

Q2. How do you add a new route?
A. I add one config object with id, path, methods, middleware, timeout, allowed query params and body rules. The router reads it at startup and creates the Fastify route. No new handler code is needed.

Q3. Why config-driven routes instead of writing handlers?
A. All routes behave the same way: validate, then proxy. So one generic handler is enough. Config is easy to review and hard to get wrong. The cost is that the config must be kept in sync with the services.

Q4. Why did you split the config into many files?
A. One 2,400-line file was hard to search and caused merge conflicts. One file per controller mirrors the service code, so anyone can find a route fast. Behaviour did not change because the index file spreads all arrays into the same list.

**Auth and security**

Q5. Where is authentication done and where is authorization done?
A. Authentication (is the token valid) is done at the gateway with `@fastify/jwt`, and again in the service. Authorization (does this user have permission X) is done in the service with guards, because the service knows its own permission rules.

Q6. Why verify the JWT twice?
A. Defense in depth. The gateway stops bad traffic early and cheaply. The service still protects itself if someone reaches it without the gateway.

Q7. A user is deactivated but their JWT is still valid. How do you block them?
A. user-service writes the user id to a Redis sorted set and publishes a message. The gateway keeps the list in memory, updates it from pub/sub right away, and re-syncs every minute. The `auth` middleware rejects ids in that list.

Q8. Why do some routes have no auth middleware?
A. Public reads, provider webhooks, and scheduler endpoints. Webhooks cannot send our JWT, so they are checked upstream by signature or internal secret.

**Validation and bugs**

Q9. A filter works in Postman against the service but not from the app. What do you check first?
A. The gateway allow list. If a query param is not in `query_params.allowed`, the gateway drops it silently. The request still succeeds, but the filter is ignored. I hit this many times with lawyer list filters and call-log stats.

Q10. What was the array query param problem?
A. `expertise=2&expertise=4` becomes an array in Fastify. The schema expected a string, and Axios re-wrote it as `expertise[]=2`. I built a fix that forwards the raw query string. Later we chose a simpler way: remove the gateway allow list for those list routes and let the service DTO validate.

Q11. Why remove gateway validation instead of extending it?
A. For fast-changing admin filters, two copies of the rules (gateway and service) kept going out of sync. The service DTO is the real source of truth. Keeping one copy removed a whole class of bugs.

Q12. What was the KYC Content-Type issue?
A. The routes required a `Content-Type` header. GET requests and some uploads do not send one, so they failed at the gateway. I made the header required only on routes that take a JSON body.

**Reliability**

Q13. How do retries work, and when should you not retry?
A. The HTTP client tries up to N times with exponential backoff. 4xx is never retried. Writes like payments, claims, or provider switches should use one attempt, or the action can happen twice.

Q14. Tell me about a bug you found in the gateway itself.
A. `retries: 0` crashed the route. The field really means total attempts, so 0 meant the loop never ran and the code read an undefined error. I used `retries: 1`, added a comment, and wrote a task doc with two fix options.

Q15. What happens when a service is down?
A. Axios fails or times out, the client retries, then the gateway returns 502 or a timeout error with the request id. 4xx bodies from the service are passed through unchanged so the app can show the real message.

Q16. What would you improve in this gateway?
A. Make rate limiting real (it is a placeholder now), wire in the circuit breaker and health checks that already exist as code, validate `retries >= 1` at config load, and add tests for route config.

---


---

# Part 7: Tech Stack and Packages

## Tech Stack and Packages (All Repos)

### 1. vs-gateway packages (from `package.json`)

| Package | What it is | Where it is used |
|---|---|---|
| `fastify` (5) | Fast Node.js web framework | `src/app.ts`: the server. `src/core/router.ts`: route registration |
| `@fastify/jwt` | JWT plugin | Registered in `app.ts`. `request.jwtVerify()` in the `auth` middleware |
| `@fastify/cors` | CORS plugin | `app.ts`, origins from `CORS_ORIGINS` |
| `@fastify/helmet` | Security headers | `app.ts` |
| `@fastify/multipart` | File upload parsing | `app.ts` (10MB file limit, 5 files) |
| `axios` | HTTP client | `src/utils/httpClient.ts`: calls upstream services, retries, keep-alive |
| `ioredis` | Redis client | `src/utils/redis.ts`, `src/utils/inactive-users.ts` |
| `pino`, `pino-pretty` | JSON logger | `src/utils/logger.ts` |
| `ajv` (+ `ajv-formats`, imported but not a direct dependency) | JSON schema validator | `src/utils/validation.ts`, `src/config/validator.ts` |
| `chokidar` | File watcher | `src/config/watcher.ts`: config hot reload in dev |
| `dotenv` | Loads `.env` | Startup |
| `tsconfig-paths` | Path alias support | Build/run |
| `@fastify/rate-limit`, `@fastify/swagger`, `@fastify/swagger-ui`, `@fastify/sensible` | Rate limit, API docs, helpers | Installed, but no import found in `src` (not in use) |
| `jsonwebtoken`, `uuid`, `fastify-plugin` | JWT lib, id generator, plugin helper | Installed, no direct import found in `src` (request ids use Node `crypto.randomUUID`) |

Dev: TypeScript 5, `ts-node-dev` (dev server), ESLint with `@typescript-eslint` and Prettier plugin, Jest + `ts-jest` + Supertest (configured in scripts; no test files found), Nodemon.

Scripts: `dev`, `build` (tsc), `start`, `test`, `lint`, `format`. Shell test scripts in repo root: `test-api.sh`, `test-jwt-auth.sh`, `test-service-registry.sh`.

### 2. Cross-repo table

| Repo | Framework | Language | DB / ORM | Top dependencies and purpose |
|---|---|---|---|---|
| **vs-gateway** | Fastify 5 | TypeScript | Redis only (ioredis) | `fastify` server; `@fastify/jwt` token check; `@fastify/cors` CORS; `@fastify/helmet` headers; `@fastify/multipart` uploads; `axios` upstream calls; `ioredis` Redis; `pino` logs; `ajv` validation; `chokidar` config reload; `dotenv` env |
| **user-service** | NestJS 10 (Express platform) | TypeScript | MySQL, TypeORM 0.3, `mysql2` | `@nestjs/core`/`common` framework; `@nestjs/typeorm` + `typeorm` ORM; `@nestjs/jwt`, `passport`, `passport-jwt`, `passport-local` auth; `@nestjs/config` + `joi` env config and checks; `@nestjs/swagger` API docs; `@nestjs/throttler` rate limit; `@nestjs/event-emitter` in-app events; `class-validator` + `class-transformer` DTO validation; `bcryptjs` password hashing; `ioredis` / `redis` cache; `winston` + `nest-winston` logs; `newrelic` APM; `@slack/web-api` alerts; `@google/genai` Gemini (translation); `libphonenumber-js` phone parsing; `helmet`, `compression` HTTP hardening; `moment-timezone` dates |
| **lawyer-appointment** | Express 5 | TypeScript | MySQL, Sequelize 6, `mysql2` | `express` server; `sequelize` ORM; `socket.io` real-time; `ioredis` Redis; `jsonwebtoken` JWT check; `joi`, `class-validator` validation; `node-cron` schedulers; `livekit-server-sdk` rooms and recording; `@aws-sdk/client-s3` + `s3-request-presigner` file storage and pre-signed URLs; `axios` service and provider calls; `@google/genai`, `openai`, `@anthropic-ai/sdk` AI; `@slack/web-api` alerts; `winston` logs; `newrelic` APM; `helmet`, `cors`, `express-rate-limit` security; `rrule` repeating availability rules; `moment-timezone` time zones; `swagger-ui-express` API docs; `libphonenumber-js` phone parsing |
| **comm-service** | Express 4 | JavaScript (CommonJS) | MySQL, Sequelize 6, `mysql2` | `express` server; `sequelize` ORM; `firebase-admin` Android/FCM push; `@parse/node-apn` iOS push; `nodemailer` + `googleapis` email; `axios` SMS and WhatsApp provider calls; `ejs`, `handlebars` message templates; `joi`, `express-validator`, `class-validator` validation; `node-cron` WhatsApp retry job; `winston` logs; `uuid` ids; `dotenv` env |
| **doc-service** | FastAPI + Uvicorn | Python 3.11 | MySQL, SQLAlchemy 2 async (`aiomysql`, `asyncmy`), `alembic` | `fastapi` API; `uvicorn` server; `pydantic` + `pydantic-settings` schemas and config; `sqlalchemy` ORM; `celery` background jobs; `redis` broker; `flower` Celery monitor; `jinja2` HTML templates; `weasyprint` HTML to PDF; `pypdf` merge PDFs; `Pillow` image to PDF; `boto3` S3 upload; `ics` calendar files; `num2words` amount in words on invoices; `pytest`, `httpx` tests |
| **admin-panel** | Next.js 15 (App Router, standalone output), React 19 | TypeScript | None of its own (`mongodb` driver present, usage not confirmed) | `next`, `react` UI; `@tanstack/react-query` and `swr` data fetching; `zustand` state; `react-hook-form` + `zod` forms and validation; `@radix-ui/*` UI primitives; `tailwindcss` 4 styling; `@tanstack/react-table` tables; `recharts` charts; `socket.io-client` live updates; Exotel IP-calling web SDK for in-browser calls; `@openreplay/tracker` session replay; `jwt-decode` read token claims; `xlsx`, `papaparse`, `file-saver` export; `date-fns` dates; `libphonenumber-js` phone input; `dompurify` safe HTML; `@dnd-kit/*` drag and drop; `sonner` toasts |

### 3. Developer tooling seen in the repos

| Tool | Where | Notes |
|---|---|---|
| TypeScript | gateway, user-service, lawyer-appointment, admin-panel | `tsc` build. `tsc-alias` and `tsconfig-paths` for path aliases |
| ESLint | all four TypeScript repos | `.eslintrc.*` or `eslint.config.mjs` |
| Prettier | gateway (script), user-service and lawyer-appointment (`.prettierrc`) | |
| Jest | user-service (unit `*.spec.ts`, e2e script), comm-service (`jest.config.js`, `tests/`), gateway (configured only) | lawyer-appointment runs test files directly with `ts-node` scripts |
| Supertest | user-service, comm-service, gateway (dev dep) | HTTP tests |
| Playwright | admin-panel (`e2e/`, `playwright.config.ts`) | End-to-end tests |
| pytest | doc-service | |
| Docker / docker-compose | all six repos | Multi-stage builds in user-service, lawyer-appointment, admin-panel |
| Bitbucket Pipelines | user-service (`bitbucket-pipelines.yml`) | Build image, push, update Kubernetes manifests |
| Jenkins | lawyer-appointment (`Jenkinsfile`) | Scheduled-call poller job |
| Swagger / OpenAPI | user-service (`@nestjs/swagger` at `/<prefix>/docs`), lawyer-appointment (`swagger-ui-express`), doc-service (FastAPI `/docs`) | |
| Postman collections | user-service (several `.postman_collection.json`, `postman/`), comm-service (`postman/`) | |
| Migrations | user-service: TypeORM CLI (`migration:generate`, `migration:run`, `migration:revert`). lawyer-appointment: `sequelize-cli` (`db:migrate`) plus SQL files. comm-service: own `migrations/migrate.js`. doc-service: Alembic in requirements, `init.sql` | Team rule: an agent never runs a migration; SQL is given to a human |
| Nodemon / ts-node-dev | all Node backends | Dev reload |
| New Relic | user-service, lawyer-appointment (`newrelic.js`) | APM and logs |
| Husky / lint-staged | not found | |
| `patch-package` | admin-panel | Patches the Exotel web SDK after install |

### 4. Not confirmed (be careful in interviews)

- finance-service internals and the payment provider name (repo not in my folder).
- The exact Kubernetes setup for repos other than user-service (only user-service has the pipeline file).
- Whether `main` and other branches of the gateway use the raw-query-string proxy described in the repo's CLAUDE.md. On `release-250926` and `origin/main` the proxy passes parsed query params through Axios.
- Whether the `mongodb` driver in admin-panel is actively used.
- Bonvoice code in lawyer-appointment was only seen on its feature branch, not on the branch that was checked out.
- Gateway Jest tests: scripts exist, no test files found.

---

# Part 8: Month-by-Month Timeline

| When | What I worked on |
|---|---|
| **Dec 2025** | Joined. Swagger docs (auto-generated for `lawyer-appointment`, decorators for `user-service`). Restricted chat config with gateway routes. Lawyer profile sections: career, case highlights, education. First Slack alert module. Daily reset of agent assignment counts. |
| **Jan 2026** | Lawyer status API. Name and date-of-birth validation. Call quota bug fixes. Schedule requests and slot confirmation rules. Chat notes privacy fix. Generate-ICS (calendar invite) API in `doc-service`. Gateway route file split into one file per controller. Verify-lawyer and admin routes in the gateway. |
| **Feb 2026** | Case management (case confirmation, participants, proceedings, case-transfer list). Schedule call list API. Recent activity and performance APIs. Documentation-service tickets. Document upload file-type check. Logout and device de-register. Call push only to the latest device. Auth added to profile routes. Speaking languages and expertise at signup. |
| **Mar 2026** | Started on the admin panel. Manager call-logs page and call-log stats API. Agent search shows only socket-connected agents. Call-log search rewritten in raw SQL. Disposition fixes (one per call, right agent). Slack alert on count reset. |
| **Apr 2026** | Auto disposition when Exotel retries a call. City / state / expertise filters for lawyers and clients (backend, gateway, UI). KYC section with approve / reject. CSV export with permissions. Work queue clustering. Courts: view-map button, atomic add-court. Auto un-verify on profile change. "Ready for verification" Slack alert. DND error. |
| **May 2026** | AI-call intake (receive, socket broadcast, atomic claim). Admin audit log. Max courts from env. Push notification fixes. `user_type` in the JWT. Latest user device API. Verified badge in the lawyer search modal. Call status rework started. |
| **Jun 2026** | Exotel webhook service work. Missed / unanswered leg handling (`from_leg_unanswered`, `to_leg_unanswered`). Cron job routes. Page titles in the admin panel. Search package by lawyer route. |
| **Jul 2026** | Work Queue V2 (backend-driven tabs, stats, priority). Follow-up reminder polling with a bounded time window. Call status and timing fixes for consultation calls. Lawyer number search. Red status call logs on My Dashboard. |
| **Aug 2026** | Callback claim route. Stuck-call disposition backfill. Call-log stats work. Calendar view for the work queue. Admin edits of lawyer basic details and pricing. Address CRUD. App config table. Permission-based hiding of details. Anonymised names in lawyer search. |
| **Sep 2026** | Blocked numbers (inbound deny list). Disposition notes. Update user email / phone with session revoke. Court filter and search relevance sorting. Payment link over WhatsApp and the send-payment-link modal. Consultation package search-or-create. |
| **Oct 2026** | Bonvoice integration started (VS-647): outbound, then inbound, provider switch. DTMF priority routing and badge. Stale-call cleanup with agent names. Queue calls to failed calls. Dashboard-approved lawyer flow. Random lawyer ordering. Bar registration number. Call notes 24-hour rule. Call-log detail modal. Dispositions without a call log. |
| **Nov 2026** | Bonvoice per-agent provider and telephony update permission. User status flags (VS-676). Admin edit of consultation preferences (VS-684). Lawyer profile PDF with deep link, growing page and fingerprint reuse. Consultation stage fix. Call landing bug fix (VS-677). |

---

# Part 9: Interview Preparation

## 9.1 Stories for "tell me about a hard problem"

Use the STAR shape: Situation, Task, Action, Result. Keep each answer under 2 minutes.

**Story 1: Two calls got the same agent**
- *Situation:* Inbound routing read the list of available agents and then wrote the assignment. Two calls arriving together could both pick the same agent.
- *Action:* I replaced "read then write" with one SQL statement: `UPDATE call_center_agents SET status='on_call', current_call_id=? WHERE id=? AND status='available'`. Only one call gets `affectedRows = 1`. The loser tries the next agent. The release also checks `current_call_id`, so an old call cannot free an agent who is on a newer call.
- *Lesson:* Let the database decide the winner. Do not check in code and then write.

**Story 2: Calls stuck in "ringing" forever**
- *Situation:* Sometimes the final webhook never arrived. The call log stayed "ringing" or "answered" and the agent stayed "on call", so they got no new calls.
- *Action:* I built a reconcile job. It finds rows older than 20 minutes that are not final, asks the provider's Call Details API for the real state, and updates the row and frees the agent. It is called from a cron route. There is also a manual refresh button for managers.
- *Lesson:* Webhooks can be lost. Always have a way to pull the truth. Never guess a final status.

**Story 3: Dashboard shows a call but the phone never rings**
- *Situation:* Agents saw an incoming call banner, but no call came.
- *Action:* I traced it. The caller had hung up during the greeting, but the provider still polled our routing URL with the call already completed. Our code claimed an agent anyway. I added an "abandoned poll" guard as a pure function with its own test, behind a database switch that is off by default.
- *Lesson:* Read the real payloads. Put risky fixes behind a flag.

**Story 4: The same webhook arrives twice**
- *Situation:* The provider retries and polls. Two requests for the same call could both try to insert a call log.
- *Action:* A unique index on the provider call id, and catching the unique-constraint error and turning it into an update. For routing, a Redis lock per call id with a cached reply, read from the Redis master to avoid replica lag.
- *Lesson:* Idempotency comes from unique keys, not from "check if exists" code.

**Story 5: Times were 5 hours 30 minutes wrong**
- *Situation:* Call start and end times were off.
- *Action:* The provider sends IST times with no timezone. `new Date()` read them as UTC. I wrote one parser that adds `+05:30` when no offset is present and used it everywhere.
- *Lesson:* Always know the timezone of data that comes from outside.

**Story 6: The manager page was slow**
- *Situation:* The call-logs page fetched 1000 rows and computed stats in the browser. Search did one query per row for names and dispositions.
- *Action:* I wrote a separate stats API, rewrote search in raw SQL, and replaced per-row lookups with batch lookups in chunks.
- *Lesson:* Do aggregation in the database. Batch, do not loop.

**Story 7: A gateway setting meant something else**
- *Situation:* During the Bonvoice work a route with `retries: 0` crashed.
- *Action:* I found that `retries` in our gateway really means "total attempts", so 0 means the request is never sent. I fixed the route and wrote it up so others do not hit it.
- *Lesson:* Read the code behind a config name before trusting the name.

**Story 8: A fix I built and then replaced**
- *Situation:* Array query parameters were dropped by the gateway. I built a core fix in April, and it was reverted three days later.
- *Action:* In May the problem was solved a simpler way, by removing the gateway allow-list on the lawyer and client list routes.
- *Lesson:* A good answer for "tell me about a time your solution was not used".

## 9.2 Topics to revise (each one is used in my work)

**Node.js and TypeScript**
- Event loop, promises, `async/await`, error handling in async code.
- Difference between Express, Fastify and NestJS. Middleware vs NestJS guards, interceptors, pipes and decorators.
- Dependency injection in NestJS (modules, providers).

**Databases**
- Transactions and when to use them (my add-court API).
- Row locking and conditional `UPDATE` (my agent claim).
- Unique indexes for idempotency. What a unique-constraint error is.
- N+1 queries and how to fix them with batch queries or joins.
- ORM vs raw SQL: when I chose raw SQL and why.
- Migrations: adding an ENUM value, adding a column safely on a busy table.
- TypeORM vs Sequelize basics, since I used both.

**Redis**
- Distributed lock (`SET key value NX EX`), why a lock needs an expiry.
- Caching a reply, master vs replica lag.
- Redis hash for counters (my assignment counts).
- "Fail open" vs "fail closed" when Redis is down.

**Webhooks and integrations**
- What a webhook is, retries, duplicates, out-of-order events.
- Idempotency. Signature checks (HMAC). *Be honest: our live webhooks do not verify a signature today. Say what you would add.*
- Webhook (push) vs polling an API (pull), and why I use both.

**Real-time**
- Socket.IO: namespaces, rooms, events, reconnects.
- WebSocket vs polling. Where I chose bounded polling (follow-up reminders) and why.

**Auth**
- JWT parts, HS256, issuer and audience, access vs refresh tokens.
- Role-based vs permission-based access (`resource:action`).
- Why the UI hides a button but the API must still check.

**API gateway**
- What a gateway does: one entry point, auth, validation, proxy.
- Rate limiting, retries, circuit breaker as ideas. *Be honest: in our gateway rate limiting only logs and the circuit breaker is not wired in.*

**Frontend**
- React hooks, `useEffect` cleanup, race conditions with late responses.
- SWR: cache keys, `mutate`, `useSWRInfinite`.
- Next.js App Router, route handlers as a backend-for-frontend proxy.
- Controlled forms with react-hook-form and zod.

**Other**
- Cron jobs and why a job should be safe to run twice.
- AWS S3 presigned URLs.
- Feature flags and staged rollout.
- Docker basics and what a CI pipeline does.
- Python: FastAPI, Pydantic models, Jinja2 templates, HTML to PDF.

## 9.3 Things to be careful about in interviews

Interviewers ask follow-up questions. Only claim what you can explain.

1. **Exotel:** study Part 2 fully. You should be able to draw the inbound and outbound flows on paper and explain the agent claim, the status mapping and the reconcile job without notes.
2. **Bonvoice** is built and tested on the dev environment. It is on its feature branch and dev branches, not in production yet. Say "integrated and tested on dev".
3. **Some Aug – Oct 2026 features** are in the release branch but not in `main` in the local clone. Check what is live before you say "in production".
4. **Webhook security:** an HMAC signature check exists in the code but only the old handlers call it. The live webhook routes have no signature check. If asked, say so and say what you would add.
5. **Gateway:** rate limiting only logs, and the circuit breaker and health checks are not connected. Do not say "I built rate limiting".
6. **No real business numbers** (for example "made it 40% faster") exist in the code. Do not invent them. If you want numbers on your resume, measure them first or ask your team.
7. **Unit tests:** there is little test code in my commits. If asked about testing, talk about the pure-function tests that exist and how you test with Postman and on dev, and say you want to write more tests.
8. **Things marked "not confirmed"** in each part: read those lists once before an interview.
9. **Do not share company code, secrets or customer data** in any interview or on GitHub. This file is safe, but the repos are not yours to share.

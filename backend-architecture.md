# Daily Quiz & Challenge — Backend Architecture & Security

**Last reviewed:** 2 November 2026  
**Status:** Current V1 implementation documented; future controls are explicitly marked as planned.

This document describes the deployed/code-backed architecture of Daily Quiz & Challenge. It is not a target-state design presented as if already implemented. The app uses Cloudflare Workers as its backend intermediary.

## 1. System overview

```text
Android application (Capacitor)
  └─ React 19 + TypeScript + Vite
       ├─ Quiz UI and answer interaction
       ├─ Question Engine
       │    ├─ Remote Worker requests
       │    └─ IndexedDB: DailyQuizDB / recent_history
       ├─ localStorage: local streak and best-score values
       └─ AdMob integration
              │ HTTPS
              ▼
Cloudflare Worker: daily-quiz-intermidiary
  ├─ Request routing / validation / response normalization
  ├─ Open Trivia DB ─ General Knowledge, Science
  ├─ Africa API ─ Africa & Nigeria
  ├─ Current Affairs ─ verified fact bank + generators
  └─ News Quiz ─ separate NewsData.io provider + generator
```

The client communicates with the Worker over HTTPS. The Worker mediates external providers and keeps provider credentials out of the Android client.

## 2. Frontend and Android client

- React 19, TypeScript 5.7 and Vite 6.
- Capacitor Android 8.4.2.
- Android application ID: `com.richard.dailyquizchallenge`.
- Package version: `1.1.9` (from `package.json`; release/build version synchronization should be checked separately).
- The app currently exposes General Knowledge, Bible, Africa & Nigeria, Science and Current Affairs categories.
- AdMob is integrated through `@capacitor-community/admob`.

### Client persistence

`src/questionEngine.ts` opens IndexedDB database `DailyQuizDB`, version 3, and maintains a `recent_history` object store. The client sends recent question IDs to the Current Affairs Worker so the server can avoid returning recently seen items.

`src/App.tsx` currently stores streak and best-score values in localStorage. These values are device-local and user-editable in principle; they are not trusted account records, synchronized progress or server-authoritative achievements.

The Question Engine validates and normalizes remote question payloads before use. Client-side history is an anti-repetition aid, not an authoritative record of every user's quiz activity.

## 3. Cloudflare Worker

### Configuration

Source: `wrangler.toml`

- Worker name: `daily-quiz-intermidiary`
- Entry point: `worker/index.js`
- Compatibility date: `2026-09-17`
- Required secrets: `AFRICA_API_KEY`, `NEWSDATA_API_KEY`
- No D1, KV or R2 binding is currently declared.

Production Worker URL used by the Question Engine:

`https://daily-quiz-intermidiary.richardoha25.workers.dev`

A separate Africa & Nigeria test Worker URL is also present in the client. Treat it as a test endpoint and review whether it should remain in production builds.

### Routes and category handling

The Worker exposes:
- `GET /api/health` — health/status response.
- `GET /api/test/africa-api` — provider connectivity test.
- `GET /api/questions` — category question request.

The questions route validates category, difficulty and requested limit (bounded to 1–20), then dispatches to category-specific handlers. The code includes General Knowledge, Science, Bible, Africa & Nigeria, Current Affairs and News Quiz handling.

Current provider/implementation status:

| Category / route | Current implementation |
|---|---|
| General Knowledge | Open Trivia DB |
| Science | Open Trivia DB |
| Africa & Nigeria | Africa API-backed retrieval/normalization and question handling |
| Current Affairs | Version-controlled verified-fact Question Bank, validators, assembly and request-scoped runtime generation |
| News Quiz | Separate NewsData.io provider and generator in `worker/news-quiz/` |
| Bible | No completed provider integration; treat as placeholder/future work |

External fetches use an 8-second timeout in the inspected Worker implementation. Provider errors should be surfaced as controlled API errors rather than exposing secrets or raw upstream internals.

## 4. Current Affairs and News Quiz boundaries

These are distinct content systems and must remain isolated.

### Current Affairs

Current Affairs is a fact-first knowledge system, not a NewsData headline quiz. Its domains include Nigeria, Africa, World, International Organizations, Economy & Business, Geography, Sports, and Science & Technology.

Its code path imports a version-controlled Question Bank, validates the collection, applies recent-history cooldowns, and invokes request-scoped runtime generation when the eligible seeded pool is insufficient. Generated fallback records are validated and assembled for the current request; they are not persisted.

The implementation phases documented on 24 September 2026 include:
1. Verified fact data and source registry.
2. Fact/concept/question-family identity model.
3. Deterministic question generation and difficulty.
4. Distractor generation.
5. Conservative quality validation.
6. Duplicate/family detection and anti-repetition.
7. Quiz assembly.
8. Question Bank contract, manager, population and audit.
9. Worker integration.
10. Request-scoped runtime-generation recovery.

The Question Bank's curated records and the client IndexedDB history are separate. The recorded active FREE seed count (29) is a seed snapshot, not the size of the full verified fact dataset.

### News Quiz

News Quiz is a separate route implemented under `worker/news-quiz/`. It fetches articles from NewsData.io and transforms them into quiz questions. Its API key and provider-specific code must not be mixed into the Current Affairs fact registry or generation pipeline.

### Persistence boundary

The Current Affairs module comments describe a possible future D1-backed persistence step. That is not implemented. The current `wrangler.toml` has no D1 binding and the Worker imports the Question Bank from JavaScript files in the repository.

## 5. External providers and secrets

| Provider | Use | Credential |
|---|---|---|
| Open Trivia DB | General Knowledge and Science | No private key in current integration |
| Africa API | Africa & Nigeria | `AFRICA_API_KEY` Worker secret |
| NewsData.io | Separate News Quiz | `NEWSDATA_API_KEY` Worker secret |

Rules:
- Configure secrets in Cloudflare's secret-management interface/CLI, never in source files.
- Never embed provider keys in Vite environment variables that are bundled into the Android client.
- Never commit API tokens, passwords, keystores or signing material.
- Keep provider-specific adapters isolated so a provider change does not leak into unrelated category logic.
- Validate, bound and normalize all upstream data before returning it to the client.

## 6. API and transport security — current state and review items

The Worker returns JSON with `Cache-Control: no-store`, permits GET and OPTIONS, and handles preflight requests. The inspected response helper reflects the incoming `Origin` header (or uses `*` when absent) rather than enforcing a configured origin allowlist.

**Important:** reflected-origin CORS is not an authentication or authorization boundary. It does not prevent non-browser clients from calling the Worker. Before treating the API as hardened, implement and test an explicit origin policy appropriate to the app's actual web origins, while recognizing that an origin check alone cannot authenticate a public mobile app.

The Worker has request validation and bounded question limits. Continue to:
- Reject unsupported categories/difficulty values and malformed history.
- Enforce request-size and input-length limits.
- Apply per-IP/device/account rate limits where technically appropriate.
- Add abuse monitoring and provider quota/cost protection.
- Return stable error codes and avoid leaking upstream credentials or sensitive diagnostics.
- Review caching carefully; user-specific responses/history must not be shared across users.
- Keep external provider timeouts and bounded retries; do not allow unbounded fan-out.

No user authentication/session service or server-side per-user authorization was identified in the current implementation. Do not imply that the public questions endpoint is an authenticated private API.

## 7. Scoring, answer keys and trust boundaries

The current public question response includes `correctAnswer`. The client therefore has access to the answer key before or during quiz interaction. Current local score, streak and best-score values are client-side state, not tamper-resistant awards.

This is acceptable only as a known V1 limitation for a casual quiz experience. It is not sufficient for cash prizes, paid competitions, transferable points, valuable rewards or fraud-sensitive leaderboards.

Before introducing valuable rewards or competitive ranking:
1. Create server-issued quiz sessions with unpredictable identifiers and bounded expiry.
2. Keep answer keys server-side until the answer is submitted.
3. Validate each answer and calculate scores on the trusted Worker.
4. Enforce replay protection, request idempotency, rate limits and suspicious-activity controls.
5. Persist authoritative session results and award ledgers in a properly selected database.
6. Audit award decisions and provide a support/dispute process.

These are future security requirements; they are not implemented in V1.

## 8. Data storage map

| Data | Current location | Trust / limitation |
|---|---|---|
| Current Affairs seed Question Bank | Version-controlled Worker JavaScript | Curated static content; updated through code deployment |
| Current Affairs runtime fallback | Worker request memory | Request-scoped; not persisted |
| Recent question IDs | Client IndexedDB `DailyQuizDB` v3 | Per-installation, client-controlled |
| Streak and best score | Client localStorage | Device-local, not authoritative |
| Provider credentials | Cloudflare Worker secrets | Server-side secret configuration |
| User accounts / cloud profiles | Not implemented | No authenticated account store identified |
| Quiz sessions / authoritative scores | Not implemented | No trusted result ledger |
| D1 / KV / R2 | Not configured | Future infrastructure only |
| Purchases / entitlements | Not implemented | Future commercial backend |

## 9. Future persistence decision

Do not add a database merely because a previous proposal named one. Select storage only when a concrete feature requires it and after evaluating cost, operational complexity, privacy and migration needs.

Potential future responsibilities:
- **D1 or another relational store:** user/account records, question metadata, quiz sessions, authoritative results, purchase verification and entitlement records.
- **KV:** carefully scoped ephemeral/rate-limit/session lookup data where consistency requirements permit.
- **R2:** larger content assets or archival datasets, not small transactional score updates.

Any adoption requires bindings, migrations, data ownership rules, access-control design, retention/deletion policy, backup/recovery plan, environment separation and integration tests. Until then, documentation must continue to state that these bindings are not configured.

## 10. Monetization and entitlement security

AdMob is integrated in the current V1 client. Google Play Billing, subscriptions, Remove Ads purchases, premium entitlements and server-side purchase verification are not implemented.

When introduced:
- Verify purchase tokens with the relevant platform through a trusted server-side flow.
- Do not trust a client-supplied `isPremium` flag.
- Store entitlement state server-side and define expiry, cancellation, refund and grace-period handling.
- Make entitlement checks consistent across app reinstalls/devices.
- Keep ad removal and premium content access distinct from content-generation logic.
- Do not introduce cash prizes or paid competitive rewards until server-authoritative scoring and fraud controls exist.

## 11. Environments, deployment and operational safety

The repository contains Cloudflare Worker configuration, GitHub Actions Android workflows and a production Worker URL. Keep development/test and production endpoints clearly separated. Secrets must be configured independently per environment.

### GitHub Actions trigger behavior

- `.github/workflows/android-debug.yml` supports manual `workflow_dispatch` and automatic pushes to `main` only. A push to a feature/working branch does not automatically start this debug build.
- `.github/workflows/android-release.yml` supports manual `workflow_dispatch` only. Do not use it for routine documentation validation; it requires release signing secrets and builds production-signed artifacts.
- To validate this working branch, open **GitHub → Actions → Android Debug APK → Run workflow**, select `docs/project-documentation-realignment` in the branch selector, and start the run. The GitHub connector available in this session can inspect workflow runs but does not expose a workflow-dispatch action, so I have not claimed to start it.
- The latest documentation commit has no associated workflow run yet. The documentation-only changes do not alter application source, but the user's requested branch build remains a separate validation step.

Before release:
- Confirm the intended Worker environment and deployed commit.
- Check required secrets without printing their values.
- Exercise health and provider routes.
- Test missing-secret, provider-timeout, invalid-input and upstream-error behavior.
- Verify Android build and category flows against the intended Worker.
- Review logs/monitoring for unexpected errors and abuse without logging sensitive user data.
- Preserve signing configuration and never commit private signing assets.

## 12. Known limitations and follow-up priorities

1. Replace reflected-origin CORS with an explicit, reviewed origin policy where browser access is required; do not mistake CORS for mobile API authentication.
2. Decide whether the Africa/Nigeria test Worker URL should be removed from production client builds.
3. Keep Bible clearly marked unavailable until a real provider/content implementation is approved.
4. Add automated tests for request validation, provider failures, timeout handling, Current Affairs generation, deduplication and response contracts.
5. Add rate limiting/abuse protection and provider-cost safeguards.
6. Design server-authoritative sessions/scoring before valuable leaderboards or rewards.
7. Choose and configure persistent storage only when account history, authoritative results or entitlements are ready to ship.
8. Add monitoring, operational runbooks, backup/restore and data-retention policies alongside any future persistence layer.

## 13. Architecture change control

- This document describes Daily Quiz & Challenge only.
- Superseded V2 backend proposals must not be reintroduced as current infrastructure.
- RichInsights is a separate product/repository; do not copy its backend or modify its code as part of Daily Quiz work.
- Keep confirmed implementation, planned work and proposals explicitly labelled.
- Update this document whenever Worker routes, provider credentials, storage bindings, authentication, scoring or billing boundaries change.


## V2 target architecture and account/security plan — 3 November 2026

**Status: approved target direction; not implemented or deployed.** The current V1 facts above remain unchanged. V2 uses native Kotlin/Compose and Cloudflare remains the primary backend platform.

### V2 service boundaries

The existing Worker and provider adapters remain the starting point. V2 should introduce a separately versioned API surface rather than breaking V1 routes. Planned service domains include:
- Identity and authentication (email/password and Google sign-in).
- Account/profile and preferences.
- Quiz catalog, categories and topics.
- Quiz-session creation, question delivery and answer submission.
- Server-authoritative result/scoring ledger.
- Progress, streaks and achievements.
- Challenge participation and leaderboard queries.
- Future entitlement and purchase verification.

Exact endpoint names, request/response schemas, token format, database schema and deployment topology must be approved in Phase 1 before implementation. Existing V1 `/api/health` and `/api/questions` contracts must remain compatible during V2 development.

### Cloudflare persistence evaluation

Cloudflare D1 is the leading candidate for relational account/profile data, quiz sessions, authoritative attempts/results, progress and leaderboard source records. This is a V2 recommendation under evaluation, not an existing binding. The current V1 Worker has no D1, KV or R2 binding.

Before configuring D1, document plan limits/cost, schema/indexes, transactional requirements, migrations, backup/restore, retention/deletion, data export, environment separation and operational monitoring. Use KV only for use cases whose consistency/expiry requirements fit it; use R2 for larger assets rather than transactional score writes. No additional Cloudflare service should be introduced without a defined requirement.

### Authentication and account controls

V2 supports:
- Email/password registration and login.
- Google sign-in.

The Worker/backend must verify Google identity tokens before issuing an application session. Passwords must be stored only using a vetted password-hashing implementation supported by the Workers runtime; exact algorithm/library selection belongs to Phase 1 technical validation. Use revocable, expiring sessions and protect refresh credentials. Enforce authorization on every account-specific route, validate inputs, rate-limit sensitive operations, prevent account enumeration and replay, and never trust client-supplied user IDs, roles, premium flags or score totals.

The Android client must not contain a shared secret or provider API key. CORS/origin checks are supplementary and do not authenticate a native mobile client. Keep all secrets in Cloudflare secret configuration.

### Server-authoritative quiz and leaderboard model

The current V1 client receives `correctAnswer` and stores streak/best-score values locally. These remain known V1 limitations. V2 must use server-issued quiz sessions, withhold answer keys from the client, validate each answer against the active session, calculate score on the Worker, enforce expiry/idempotency/replay controls, and persist authoritative results before updating rankings or achievements.

Initial competition remains non-cash. Do not add cash prizes or valuable rewards until scoring, fraud controls, auditability and applicable policy/legal requirements are separately reviewed.

### Phase 1 backend deliverables

1. Inventory and contract documentation for existing Worker routes and provider adapters.
2. V2 API versioning and endpoint contract proposal.
3. Authentication/session flow and threat model.
4. D1 suitability/cost evaluation and proposed relational ERD/schema.
5. Authorization matrix and personal-data lifecycle rules.
6. Quiz-session, answer-submission, score and leaderboard trust model.
7. Rate-limit, abuse, monitoring, backup and recovery requirements.
8. Migration/compatibility strategy that preserves V1.

No account service, D1 binding, new production endpoint or V1 route change is implied by this planning document.


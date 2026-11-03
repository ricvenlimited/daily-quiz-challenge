# Daily Quiz & Challenge — Native Android V2 Architecture

**Decision date:** 3 November 2026  
**Status:** Approved architecture direction and Phase 1 planning baseline. This document describes the V2 target architecture; it does not claim that the native application, account system, database bindings or new APIs have been implemented.

## 1. Product and branch boundaries

Daily Quiz & Challenge V2 is a major native Android evolution of the existing product. It is not a replacement repository and it must not destabilize V1.

- Repository: `ricvenlimited/daily-quiz-challenge`.
- V1 stable branch: `main`.
- Documentation alignment baseline: `docs/project-documentation-realignment`.
- V2 native development branch: `v2-development`.
- Existing V1 application ID: `com.richard.dailyquizchallenge`.
- Existing V1 stack: React, TypeScript, Vite and Capacitor Android 8.4.2.
- Approved V2 stack: native Android with Kotlin and Jetpack Compose.
- Primary backend infrastructure: Cloudflare.
- Product direction: global quiz and learning platform under the Ricven brand.

The V2 branch was created from `docs/project-documentation-realignment` so it inherits the aligned project, UI/UX, commercial and backend documentation. The V2 branch is the working line for native architecture and implementation. Do not merge V2 into `main during V2 development. Any eventual production migration/release and branch integration require a separate explicit review and approval.

Preserve the V1 code, production signing identity, package/application ID, AdMob configuration, working Cloudflare Worker routes, provider integrations and release history. Do not perform a destructive rewrite of V1. Do not copy V1's web UI into the native V2 client.

## 2. Confirmed product decisions

| Area | Approved decision |
|---|---|
| Client technology | Native Android |
| Language | Kotlin |
| UI toolkit | Jetpack Compose |
| Primary navigation | Five destinations |
| Authentication | Email/password and Google sign-in |
| Backend platform | Cloudflare |
| Repository | Existing `ricvenlimited/daily-quiz-challenge` |
| V1 treatment | Preserve `main` as the stable V1 baseline |
| V2 workflow | Separate `v2-development` branch |
| Product scope | Global quiz and learning platform |

### Five primary navigation destinations

1. **Home** — dashboard, daily challenge entry, streak/progress overview and quick actions.
2. **Explore** — categories, subjects, topics, content discovery and quiz setup.
3. **Challenges** — daily/special challenges, participation and challenge history.
4. **Rankings** — leaderboards, rankings and competitive progress.
5. **Profile** — account/profile, personal quiz history, statistics, achievements and settings entry points.

Use a persistent five-destination bottom navigation pattern for the main app shell. Each destination owns its own nested navigation graph/back stack where appropriate. Detail screens such as quiz setup, active quiz, results, category/topic detail, achievement detail, account settings and authentication must not become extra primary tabs. Define selected state, deep-link behavior, back/up behavior, restoration and authentication gates before implementation.

### Account and authentication scope

Support both:
- Email/password registration and sign-in.
- Google sign-in.

The account lifecycle must also cover sign-out, session renewal/expiry, password recovery, email verification where required, account/profile updates, account deletion/data-retention policy, and safe handling of provider or network failures. Google sign-in is an identity-provider flow; it must be verified by the trusted backend before an account session is issued. Do not trust a client-supplied user ID, email, role, premium flag or score.

Authentication screens are part of the V2 product flow, not primary bottom-navigation destinations. Users may explore appropriate public content before signing in, but account-dependent actions such as syncing progress, submitting authoritative results, and accessing personal history must require a valid authenticated session.

## 3. Native Android client architecture

### Approved core stack

| Layer | Technology / responsibility |
|---|---|
| Programming language | Kotlin |
| UI | Jetpack Compose |
| Design system | Material 3, customized with approved Ricven brand tokens |
| Navigation | Navigation Compose |
| Architecture | MVVM with clear UI, domain and data boundaries |
| State | ViewModel + StateFlow; immutable UI state |
| Async work | Kotlin Coroutines and Flow |
| Networking | Select Retrofit or Ktor during implementation based on project fit; use one consistent HTTP client |
| Local relational persistence | Room where offline/cache requirements justify it |
| Preferences | DataStore |
| Dependency injection | Select and document a single approach before broad implementation |
| Build | Gradle with centralized dependency/version management |
| Testing | Kotlin unit tests, Compose UI tests, repository/API contract tests and Android instrumentation tests |
| CI | GitHub Actions with separate V2 debug validation |

Do not introduce both Retrofit and Ktor, multiple DI frameworks, or redundant persistence libraries. Phase 1/2 must settle exact versions and libraries against current Android compatibility, maintenance and build constraints before implementation.

### Layering

```text
Compose UI / Navigation
        ↓ user actions, observes state
ViewModel (StateFlow)
        ↓
Use cases / domain rules
        ↓
Repository interfaces
        ↓
Data implementations
   ├── Cloudflare API client
   ├── Room local data source (when required)
   └── DataStore preferences
```

Compose screens should render state and dispatch user actions. They should not directly issue network requests, own business rules, store credentials, or calculate trusted competitive scores. ViewModels coordinate screen state; repositories abstract remote/local data; domain use cases hold reusable application rules.

## 4. Design system and UI/UX

Use the approved shared Ricven/RichInsights palette documented in `ui-ux-project.md`. Do not invent a competing palette. Preserve accessible contrast, scalable typography, predictable touch targets, light/dark themes and coherent component behavior.

The native UI should be designed in Penpot before detailed screen implementation. Approved screens and interaction decisions must be recorded in `ui-ux-project.md`. Build reusable Compose components and theme tokens rather than styling each screen independently.

The five-tab information architecture is approved, but detailed screen layouts, icon choices, tab labels, badges, empty states and transitions remain subject to wireframing and design review.

## 5. Cloudflare backend architecture

Cloudflare remains the primary infrastructure. V2 should extend the existing backend deliberately rather than replacing working provider integrations.

### Existing V1 backend facts

- Existing Worker: `daily-quiz-intermidiary`.
- Existing production base URL: `https://daily-quiz-intermidiary.richardoha25.workers.dev`.
- Existing routes include `GET /api/health` and `GET /api/questions`.
- Existing provider/content paths include Open Trivia DB (General Knowledge and Science), Africa API (Africa & Nigeria), fact-first Current Affairs, and a separate News Quiz/NewsData.io route.
- Existing V1 question response includes `correctAnswer`; V1 local streak/best-score values are device-local.
- No D1, KV or R2 binding was configured in the backend configuration reviewed on 2 November 2026.
- No authenticated account/session service or authoritative per-user result ledger was identified in the reviewed V1 Worker.

These are current V1 facts, not a description of completed V2 infrastructure. Preserve category-specific provider boundaries, especially the separation between fact-first Current Affairs and News Quiz.

### Target backend responsibilities

Cloudflare Workers will provide versioned HTTPS API endpoints for:
- Identity/authentication integration and session lifecycle.
- Account/profile management.
- Category/topic/catalog data.
- Quiz-session creation and lifecycle.
- Question delivery and answer submission.
- Server-authoritative scoring and result persistence.
- User progress, streaks and achievements.
- Challenge participation and leaderboard queries.
- Entitlement/access checks when commercial features are implemented.
- Abuse controls, validation, audit events and operational health.

### Persistence decision

Cloudflare D1 is the leading relational database candidate for V2 accounts, profiles, quiz sessions, authoritative attempts/results, progress and leaderboard source records because these features need structured relationships and transactional consistency. This is a target recommendation for Phase 1 validation, not a claim that D1 is currently configured or deployed.

Before creating production bindings, Phase 1 must validate current Cloudflare plan limits/pricing, SQLite/D1 transaction and indexing needs, data volume assumptions, migration/backup/restore strategy, privacy and deletion requirements, and environment separation. KV may be considered for suitable ephemeral lookups or rate-limit/session support only where its consistency model fits. R2 is for larger assets/content files, not transactional score updates. Do not add services without a defined requirement.

### API compatibility and evolution

Keep existing V1 endpoints operational while V2 is developed. Define a versioned V2 API contract (for example, a clearly namespaced `/api/v2/...` surface) rather than silently changing existing response contracts. Exact route names and payload schemas must be documented before implementation. Add contract tests and explicit compatibility rules.

The V2 client must not call third-party question providers directly when credentials, normalization, quotas or business rules belong on the Worker. Provider credentials remain server-side Cloudflare secrets.

## 6. Authentication and security requirements

Authentication implementation must be selected after a focused Phase 1 technical review. The design must support email/password and Google sign-in without embedding private secrets in the Android application.

Required security properties:
- Verify Google identity tokens on the trusted backend using the supported Google verification flow; validate issuer, audience, signature and expiry.
- Use a vetted password hashing approach suitable for the Cloudflare Workers runtime; never store plaintext or reversibly encrypted passwords. Select and document the exact implementation after runtime/library verification.
- Use short-lived access credentials and a revocable/rotatable session or refresh-token strategy; store only appropriately protected token representations server-side where applicable.
- Enforce authorization on every protected Worker route. Authentication alone does not authorize access to another user's records.
- Validate request schemas, body sizes, field lengths and allowed state transitions.
- Apply rate limits and abuse controls to registration, login, password recovery, token refresh, quiz creation, answer submission and other sensitive routes.
- Avoid account enumeration in recovery/login error behavior.
- Protect against replay, duplicate submission and race conditions using session expiry, idempotency and server-side state checks.
- Keep secrets in Cloudflare secret configuration; never commit them or bundle them in Vite/Android assets.
- Use TLS for all network traffic and avoid logging passwords, tokens, full identity assertions or unnecessary personal data.
- Define privacy notice, consent, retention, account deletion and data export requirements before launch.
- Test missing/expired/revoked sessions, invalid Google tokens, brute-force/rate-limit behavior, cross-account access and backend outages.

CORS is not mobile-app authentication. A public Android client cannot keep a shared API secret confidential. Origin checks, if used, are supplementary controls only.

## 7. Trusted quiz sessions, scores and competition

V1 exposes correct answers to the client and stores some score/streak values locally. That is a known V1 limitation. V2 must not treat client-calculated scores or client-submitted totals as authoritative for rankings, achievements or valuable rewards.

Target flow:

```text
Authenticated user
   ↓
Worker creates a bounded quiz session
   ↓
Worker delivers eligible questions without answer keys
   ↓
Client submits an answer for the active question
   ↓
Worker validates session, timing, question and submission
   ↓
Worker evaluates answer and calculates score
   ↓
Worker records authoritative attempt/result
   ↓
Progress, streak and eligible leaderboard views update
```

Define session expiry, question ordering, answer deadlines, retry behavior, idempotency, reconnect handling, abandoned sessions, timeout rules, scoring, tie-breaking and auditability. Do not launch cash-prize competition. Initial rankings are non-cash and must still use trusted server-side result data.

## 8. Data domains to model

Phase 1 must produce an entity relationship diagram and initial schema proposal for at least:
- User/account identity and linked sign-in providers.
- User profile and preferences.
- Sessions, refresh/session revocation and verification state.
- Categories, topics and question-source metadata (while preserving provider ownership/provenance).
- Quiz definitions/configurations and quiz sessions.
- Question delivery records and answer attempts.
- Authoritative results and score ledger.
- Daily/weekly challenge definitions and participation.
- Streak/progress snapshots or derivable progress records.
- Achievements and award records.
- Leaderboard periods, entries or query strategy.
- Entitlements/purchases as a future domain, with server verification requirements.

Do not persist redundant derived data without a clear query/performance reason. Define ownership, indexes, uniqueness, retention, deletion behavior and migration strategy. Separate user PII from public leaderboard display data where practical.

## 9. Integration and migration strategy

- Keep V1 operational on `main`.
- Build V2 independently on `v2-development`.
- Reuse and extend stable Cloudflare question/provider logic through documented API contracts.
- Do not assume V1 IndexedDB/localStorage data is automatically trustworthy or safely migratable.
- Decide which V1 user-local data, if any, can be imported after account creation; label it as unverified legacy data unless independently validated.
- Preserve existing application ID/signing considerations until an explicit release/package migration plan is approved. A native V2 may retain the existing application ID for an in-place upgrade only if package identity, signing continuity, data migration and compatibility are verified; otherwise document the consequences before changing it.
- Keep V1 AdMob behavior documented; evaluate the appropriate native Google Mobile Ads SDK integration and policy requirements separately for V2.
- Do not remove V1 files or alter production Worker behavior as a side effect of V2 foundation work.

## 10. Phased implementation roadmap

### Phase 1 — Architecture and documentation (current)
- Confirm native Kotlin + Jetpack Compose.
- Confirm five primary navigation destinations and nested navigation rules.
- Confirm email/password and Google sign-in.
- Document V1/V2 branch and release boundaries.
- Review existing Worker routes, providers, secrets and current persistence.
- Define V2 API boundaries and compatibility approach.
- Produce account/authentication flow and threat model.
- Produce data model/ERD and evaluate D1/KV/R2 needs and cost.
- Define server-authoritative quiz-session, scoring and leaderboard rules.
- Record open decisions, assumptions, acceptance criteria and test strategy.
- Keep this phase documentation/design-only; no production backend or V1 behavior changes.

### Phase 2 — Native Android foundation
- Create/configure the native Kotlin Android application foundation in the existing repository without disturbing V1.
- Configure Gradle, Kotlin, Compose, Material 3, dependency/version management and build variants.
- Establish app module/package structure and MVVM/repository boundaries.
- Configure themes/design tokens and reusable components from approved Ricven identity.
- Configure Navigation Compose shell with five destinations and nested navigation.
- Configure networking, serialization, dependency injection and local persistence choices.
- Add baseline unit/UI tests and V2 GitHub Actions debug build.
- Demonstrate a reproducible debug APK build before feature implementation.

### Phase 3 — Navigation and screen development
- Implement Home, Explore, Challenges, Rankings and Profile destination shells.
- Implement nested flows, deep links where required, back/up behavior and state restoration.
- Add loading, empty, offline, error and retry states.
- Add authentication/onboarding screens and route guards.
- Implement the approved Penpot UI specification in Compose.
- Validate navigation accessibility and behavior on real Android devices.

### Phase 4 — User accounts and authentication
- Implement email/password registration and login.
- Implement Google sign-in and backend token verification.
- Implement verification/recovery, session refresh/revocation, sign-out and account deletion flows.
- Implement profile and preferences synchronization.
- Add authorization tests, abuse controls and privacy lifecycle behavior.
- Do not expose protected user records to unauthenticated or cross-account requests.

### Phase 5 — Cloudflare backend integration
- Establish V2 Worker API contracts and environment configuration.
- Configure D1 only after Phase 1 approval, then apply versioned migrations.
- Implement account/profile/session services and protected endpoints.
- Implement quiz-session and server-authoritative result APIs.
- Integrate existing category/provider adapters without breaking V1 routes.
- Add rate limiting, validation, monitoring, logs that avoid sensitive data, backup/recovery and contract tests.
- Keep secrets isolated by environment.

### Phase 6 — Quiz and learning experience
- Implement category/topic discovery and quiz configuration.
- Integrate question delivery and session lifecycle.
- Implement answer submission, timer/timeout behavior and feedback.
- Implement trusted results, explanations and quiz history.
- Synchronize progress/streaks and provide useful performance insights.
- Validate online/offline transitions, provider failure and recovery.

### Phase 7 — Challenges and rankings
- Implement daily/special challenge lifecycle and participation.
- Implement server-authoritative points, achievements and award records.
- Implement leaderboard periods, ranking/tie rules and privacy-safe public profiles.
- Add anti-abuse, replay protection, idempotency and auditability.
- Keep competition non-cash in the initial release.

### Phase 8 — Monetization and release testing
- Evaluate and implement native AdMob integration with appropriate Android SDK and consent/policy handling.
- Preserve a useful free experience and keep rewarded ads optional.
- Implement billing/entitlement only after the commercial specification and trusted verification architecture are approved.
- Run security, privacy, accessibility, performance, device compatibility, backend resilience and migration tests.
- Validate V1-to-V2 upgrade/signing/data behavior before any production rollout.
- Produce signed APK/AAB only through the approved release process.
- Require explicit review before any V2-to-main merge or production release.

## 11. Phase 1 deliverables and exit criteria

Phase 1 is complete when the repository contains reviewed documentation for:
1. Approved V2 architecture and branch boundaries.
2. Five-tab navigation and nested information architecture.
3. Email/password + Google identity and session lifecycle.
4. V2 API surface and compatibility contract.
5. Data model/ERD and a documented Cloudflare persistence decision.
6. Security/threat model and trust boundaries.
7. Quiz-session/scoring/leaderboard authority rules.
8. Phases 2–8 implementation plan.
9. Test strategy, acceptance criteria and unresolved decisions.

No Kotlin feature implementation, D1 production binding, account endpoint deployment or V1 code modification is required to declare Phase 1 complete. The next work after approval is Phase 2.

## 12. Change control

- Mark every feature as implemented, in progress, planned or proposed.
- Update this document and the relevant UI/UX/backend documents when decisions change.
- Do not treat a proposal as a deployed feature.
- Do not modify `main` or merge V2 into it without explicit approval.
- Preserve the existing V1 product, provider and monetization behavior while building V2.
